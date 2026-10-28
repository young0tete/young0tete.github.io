---
title: "Pruning Granularity"
author: young0tete
date: 2026-09-23
categories: [Efficient AI, Pruning]
tags: [Efficient AI, Pruning, Model Compression]
pin: false
math: true
mermaid: false
---

{% include navbox.html id="pruning" %}

## 1. What is Pruning?
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

- 뉴럴넷의 가중치(weight) 또는 구조적 단위(뉴런, 필터, 레이어 등)를 **제거**하여 모델을 경량화하는 기법
- 핵심 가정: 학습된 모델은 **over-parameterized** → 실제로 중요한 가중치는 일부에 불과함
- 일반적인 파이프라인:

  $$\text{Train} \rightarrow \text{Prune} \rightarrow \text{Fine-tune}$$

- 제거 기준(criterion)으로는 weight의 크기(magnitude), gradient, 활성화값 등이 사용됨
- <span style="color:red">**Granularity**</span>: 제거하는 단위의 크기에 따라 **Structured**와 **Unstructured**로 나뉨
  - <span style="color:red">**Structured**</span>: 필터, 채널, 레이어 등 구조적 단위를 제거
  - <span style="color:red">**Unstructured**</span>: 개별 weight를 제거. 구조 자체는 변하지 않음.

## 2. Granularity Types
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

- 프루닝 단위. 개별 weight만 지울 수도 있고, 채널 등 구조 자체를 바꿀 수도 있음.

### 2.1 Unstructured Pruning
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

- <span style="color:red">**Element pruning**</span>: weight matrix의 개별 원소를 독립적으로 제거
- 제거 위치에 제약이 없으므로 높은 sparsity를 달성하면서 accuracy 유지에 유리
- 단점: irregular sparse matrix → 일반 GPU의 dense 연산 유닛을 활용하지 못해 실제 속도 향상이 어려움
  - cuSPARSE 등 sparse 연산 라이브러리 필요, 오버헤드 있음

### 2.2 Semi-structured Pruning
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

Unstructured와 Structured의 중간으로, **일정한 패턴**을 가진 sparsity를 강제함

- <span style="color:red">**Pattern pruning (N:M sparsity)**</span> ⭐: 연속된 M개의 weight 중 N개만 남기는 방식
  - ex) NVIDIA의 {% include memo.html word="2:4 sparsity" text="Accelerating Sparse Deep Neural Networks\n(Mishra et al., NVIDIA 2021)" %} — 4개 중 2개 제거 → Sparse Tensor Core로 2× 속도 향상
  - Unstructured에 비해 하드웨어 친화적, Structured에 비해 accuracy 유지 유리
- <span style="color:red">**Block (Tile) pruning**</span>: weight matrix를 고정 크기의 블록으로 나누고, 블록 단위로 제거
  - 하드웨어의 tiling 연산 구조와 맞아 cache 효율을 높일 수 있음

### 2.3 Structured Pruning
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

구조적 단위를 통째로 제거 → dense matrix 형태를 유지하므로 추가 라이브러리 없이 직접적인 속도 향상 가능.

- <span style="color:red">**Neuron / Head pruning**</span> ⭐: FFN의 뉴런 또는 Attention head 단위 제거
  - 일부 head는 redundant하다는 {% include memo.html word="연구 결과" text="Are Sixteen Heads Really Better than One? (Michel et al., NeurIPS 2019)\nAnalyzing Multi-Head Self-Attention (Voita et al., ACL 2019)" %}에 근거
- <span style="color:red">**Width pruning**</span>: hidden dimension(d_model)을 전체 레이어에 걸쳐 균일하게 줄임
  - d_ff(FFN intermediate) 축소는 Neuron pruning과 동일하므로 여기서는 d_model 축소를 의미
- <span style="color:red">**Layer (Block) pruning** ⭐</span>: Transformer block 전체를 제거
  - 가장 coarse한 단위 → accuracy drop이 크지만 속도 이득도 가장 큼
  - LLM에서 최근 활발히 연구됨 (ex. ShortGPT, LaCo)
- <span style="color:red">**Expert pruning (MoE)**</span>: Mixture-of-Experts 모델에서 일부 expert를 제거
  - 활성화 빈도가 낮은 expert를 제거해 모델 크기 감소


## 3. Trade-offs
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

### 3.1 핵심 축: 자유도 ↔ 규칙성
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

- **Granularity가 작을수록**(fine) → accuracy·압축률에 유리, **클수록**(coarse) → 실제 속도 향상에 유리
- Fine(element) 쪽은 중요한 weight만 골라 살릴 수 있는 자유도가 높은 대신, 메모리 접근이 불규칙해짐
- Coarse(layer) 쪽은 제거 단위가 커서 중요한 weight도 같이 날아가지만, 결과는 작아진 dense 행렬이므로 기존 GPU 연산 그대로 활용 가능

### 3.2 비교 표
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

| Granularity | 같은 accuracy에서 달성 sparsity | 실제 속도 향상 | 필요 HW/SW | 메모리 절감 |
|:---|:---:|:---:|:---|:---:|
| Element | ★★★★★ | ★☆☆☆☆ | cuSPARSE | ★★★★★ |
| N:M (2:4) | ★★★★☆ | ★★★☆☆ | Sparse Tensor Core | ★★★★☆ |
| Block/Tile | ★★★☆☆ | ★★★☆☆ | block-sparse kernel | ★★★☆☆ |
| Neuron/Head | ★★★☆☆ | ★★★☆☆ | 일반 dense 연산 | ★★★☆☆ |
| Width | ★★☆☆☆ | ★★★★☆ | 일반 dense 연산 | ★★★☆☆ |
| Layer | ★★☆☆☆ | ★★★★★ | 일반 dense 연산 | ★★☆☆☆ |
| Expert (MoE) | ★★★☆☆ | ★★★★☆ | 일반 dense 연산 | ★★★★☆ |

### 3.3 왜 이런 trade-off가 생기는가
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

- **Accuracy 측면**: fine할수록 중요한 weight만 선택적으로 남길 수 있음. coarse하면 그룹 안의 중요한 weight까지 함께 제거됨
- **속도 측면**: fine할수록 불규칙 메모리 접근, index 오버헤드, load imbalance가 발생함. coarse하면 작아진 dense GEMM을 그대로 사용 가능

### 3.4 FLOPs 감소 ≠ Latency 감소
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

- <span style="color:red">**Compute-bound**</span>: 연산량(FLOPs)이 병목 → FLOPs 줄이면 latency 감소
- <span style="color:red">**Memory-bound**</span>: weight 로딩이 병목 → FLOPs를 줄여도 latency 개선 미미
- LLM decode(batch=1)는 대표적인 **memory-bound** 케이스 → unstructured pruning도 weight 로딩량 자체가 줄어들어 latency 이득이 날 수 있음
- 즉, structured pruning이 항상 우월한 게 아니며 **타겟 연산 regime**에 따라 선택이 달라짐

### 3.5 Structured 내부 trade-off: Width vs Depth
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

- <span style="color:red">**Depth pruning**</span> (layer 제거): 순차 레이어 수가 줄어 small batch latency에 유리. 단, 같은 파라미터 제거량 대비 accuracy drop이 큰 편
- <span style="color:red">**Width pruning**</span> (d_model 축소): 병렬 연산 폭이 줄어듦. {% include memo.html word="Minitron" text="Compact Language Models via Pruning and Knowledge Distillation\n(Muralidharan et al., NVIDIA 2024)" %}은 retraining 후 width pruning이 depth pruning보다 accuracy가 높았다고 보고

### 3.6 선택 가이드
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

| 조건 | 권장 방향 |
|:---|:---|
| 목적이 **메모리 절감** | fine-grained (element, N:M) |
| 목적이 **추론 속도 향상** | structured (layer, width) |
| 타겟이 **Ampere 이상 GPU** | N:M (2:4 sparsity) |
| 타겟이 **범용 하드웨어** | structured pruning |
| **Fine-tuning 불가** (one-shot) | fine-grained 쪽이 복구 부담 작음 |
