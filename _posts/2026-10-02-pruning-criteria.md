---
title: "Pruning Criteria"
author: young0tete
date: 2026-10-02
categories: [Efficient AI, Pruning]
tags: [Efficient AI, Pruning, Model Compression]
pin: false
math: true
mermaid: false
---

{% include navbox.html id="pruning" %}

## 1. What is Pruning Criterion?
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

- <span style="color:red">**Pruning Criterion**</span> (= <span style="color:red">**saliency**</span>): 어떤 weight를 제거할지 결정하는 **중요도 점수**
  - 목표: 제거했을 때 loss 증가가 가장 작은 weight를 고르는 것
- weight 제거로 인한 loss 변화 $\Delta L$을 Taylor 전개하면:

  $$\Delta L = g^\top \Delta w + \frac{1}{2}\,\Delta w^\top H \Delta w + O(\|\Delta w\|^3)$$

  - $g = \partial L / \partial w$ (gradient, 1차), $H = \partial^2 L / \partial w^2$ (Hessian, 2차)
- 1차항만 보면 gradient가 작은 weight를 지워야 loss 증가가 작음
- 그런데 학습이 수렴한 모델은 $g \approx 0$ → gradient로는 어떤 weight를 지울지 구분할 수 없음
- 그래서 2차항의 Hessian을 봄 (Hessian = 해당 지점에서 loss 곡면의 곡률)
- weight $w_i$ 하나만 0으로 만들면 2차항은:

  $$\Delta L \approx \frac{1}{2} H_{ii}\, w_i^2$$

- 따라서 $H_{ii}\, w_i^2$가 작은 weight를 찾아 제거하는 것이 목표
  - Hessian만 작다고 되는 게 아니라, weight 크기와의 **곱**이 작아야 함
- 이것이 Hessian 기반 criterion({% include memo.html word="OBD, OBS" text="Optimal Brain Damage (LeCun et al., NeurIPS 1989)\nOptimal Brain Surgeon (Hassibi & Stork, NeurIPS 1993)" %})의 출발점
- 문제: LLM에서 $H$는 $d \times d$ 크기 ($d$ = 파라미터 수) → 직접 계산·역행렬 모두 불가
  - **어떻게 근사하느냐**에 따라 criterion이 갈림 → Section 3, 4
- LLM에서 criterion이 특히 중요해진 이유:
  - 모델이 커서 **retraining이 사실상 불가능** → one-shot pruning이 기본
  - fine-tuning으로 복구할 수 없으니 처음부터 정확히 골라야 함

## 2. Magnitude (baseline)
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

## 3. Hessian-based
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

### 3.1 OBD
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

### 3.2 OBS
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

### 3.3 SparseGPT
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

## 4. Data-aware
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

### 4.1 Reconstruction Error
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

### 4.2 Wanda
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

## 5. Comparison
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }
