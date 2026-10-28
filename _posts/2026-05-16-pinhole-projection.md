---
title: "[Camera Models] Pinhole Projection"
author: young0tete
date: 2026-05-16
categories: [Computer Vision, Camera Models]
tags: [Computer Vision, 3D Vision, Camera, Pinhole, Homogeneous coordinate, Projection]
pin: false
math: true
mermaid: false
---

{% include navbox.html id="camera-models" %}

## 1. Pinhole Camera Model
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

- <span style="color:red">**Pinhole camera**</span>: 렌즈 없이 아주 작은 구멍(pinhole) 하나로 빛을 받아 상을 맺는 이상화된 카메라 모델
  - 물체의 한 점에서 나온 빛 중 구멍을 지나는 광선은 하나뿐 → 3D의 한 점이 이미지의 한 점에 대응 → 선명한 상
  - 실제 카메라는 렌즈로 빛을 모으지만, 초점이 맞은 상태에서는 기하적으로 pinhole과 똑같이 다룰 수 있음 (렌즈 왜곡은 Distortion 편에서)
- 구멍 뒤 거리 $f$에 있는 평면에는 상이 **상하좌우가 뒤집혀** 맺힘
- <span style="color:red">**Virtual image plane**</span>: 구멍 **앞쪽** 거리 $f$에 평면을 둔다고 가정하면, 크기는 같고 뒤집히지 않은 상을 얻음
  - 수식에서 부호가 깔끔해져서, 이후 모든 계산은 이 평면을 기준으로 함

{% include figures/pinhole-model.html %}

- 기본 용어
  - <span style="color:red">**Optical center**</span> (camera center): 구멍의 위치. 카메라 좌표계의 원점
  - <span style="color:red">**Optical axis**</span> (principal axis): 원점에서 이미지 평면에 수직으로 뻗은 축 = $Z$축
  - <span style="color:red">**Focal length**</span> $f$: optical center에서 이미지 평면까지의 거리
  - <span style="color:red">**Principal point**</span>: optical axis가 이미지 평면과 만나는 점 (Intrinsics 편에서 다시 다룸)


## 2. Perspective Projection
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

- 카메라 좌표계의 3D 점 $P(X, Y, Z)$와 optical center $O$를 잇는 광선이, 광축 방향으로 $f$만큼 떨어진 이미지 평면과 만나는 점 $p(x, y)$를 구하는 것

{% include figures/perspective-projection.html %}

- 닮은꼴 삼각형에서 $\dfrac{x}{f} = \dfrac{X}{Z}$, $\dfrac{y}{f} = \dfrac{Y}{Z}$ 이므로:

  $$x = f\,\frac{X}{Z}, \qquad y = f\,\frac{Y}{Z}$$

  - $Z$로 나누기 때문에 같은 크기의 물체라도 **멀리 있을수록 작게** 찍힘 → 원근감
  - **문제**: $P$마다 $Z$가 다르기 때문에 곱해지는 배율 $f/Z$가 점마다 다름 → 모든 점에 똑같이 곱할 **고정된 행렬 하나**로는 투영을 쓸 수 없음 → <span style="color:red">**비선형**</span>
  - **해결**: 나눗셈을 맨 마지막 단계로 미루면 행렬 하나로 쓸 수 있음 → Section 3 (Homogeneous Coordinates)

- 투영의 성질
  - 직선은 직선으로 투영됨
    - 직선과 $O$를 함께 지나는 평면이 이미지 평면과 만나는 곳에 상이 맺히는데, 두 평면이 만나는 곳은 항상 직선이기 때문
    - **의미**: 실제 사진에서 직선이 휘어 보이면 렌즈가 pinhole 모델에서 벗어났다는 뜻 (예: 광각 사진의 휜 문틀)
    - 그래서 "직선이 얼마나 휘었나"를 재서 렌즈 왜곡을 보정하기도 함 → <span style="color:red">**Distortion**</span>
  - 3D에서 평행한 직선들은 이미지에서 한 점으로 모임 → <span style="color:red">**소실점**</span> (vanishing point)
    - 소실점은 선의 위치가 아니라 **방향**만으로 정해짐 → **평행한 방향마다 소실점이 하나씩** 생김
    - 예: 건물 모퉁이를 비스듬히 찍으면 왼쪽 벽의 창문 줄과 오른쪽 벽의 창문 줄이 서로 다른 점으로 모임
    - 이미지 평면과 나란한 방향의 선들은 소실점 없이 이미지에서도 평행하게 남음
  - 길이, 각도, 길이의 비율은 보존되지 않음
  - **깊이 정보가 사라짐**: 같은 광선 위의 점 $(tX, tY, tZ)$는 $t$에 상관없이 모두 같은 $(x, y)$로 찍힘
    - → multi-view나 깊이 추정이 필요함

## 3. Homogeneous Coordinates
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

- 문제 (Section 2): $Z$로 나누는 연산 때문에 투영을 고정된 행렬 하나로 쓸 수 없음
  - 실제 투영은 월드 → 카메라 → 이미지 평면 → 픽셀처럼 여러 변환을 이어 붙여야 함
  - 각 단계가 행렬이면 곱해서 **행렬 하나로 합칠 수 있는데**, 투영 단계가 행렬이 아니라 이게 막힘
- <span style="color:red">**Homogeneous coordinates**</span> (동차좌표): 좌표에 차원을 하나 추가해서 점을 표현하는 방식
  - 2D: $(x, y) \rightarrow (x, y, 1)$
  - 3D: $(X, Y, Z) \rightarrow (X, Y, Z, 1)$
- 핵심 규칙: **0이 아닌 상수배는 모두 같은 점**

  $$(\tilde{x}, \tilde{y}, \tilde{w}) \sim \left(\frac{\tilde{x}}{\tilde{w}}, \frac{\tilde{y}}{\tilde{w}}\right), \qquad \tilde{w} \neq 0$$

  - 예: $(2, 4, 2)$, $(1, 2, 1)$, $(3, 6, 3)$은 모두 2D 점 $(1, 2)$
- **왜 이게 투영 문제를 해결하는가**
  - 핵심: **추가한 차원에 분모($Z$)를 담아두고, 나눗셈은 맨 마지막에 한 번만 함**
  - ① 나누기 직전까지는 행렬 하나로 됨
    - 분자 $fX$, $fY$와 분모 $Z$는 각각 입력에 상수를 곱한 것뿐 → 모든 점에 **같은 행렬**을 쓸 수 있음

      $$\begin{bmatrix} f & 0 & 0 \\ 0 & f & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} X \\ Y \\ Z \end{bmatrix} = \begin{bmatrix} fX \\ fY \\ Z \end{bmatrix}$$

    - Section 2처럼 배율 $f/Z$를 곱하는 게 아니라, $Z$를 세 번째 칸에 넣어두기만 하므로 행렬이 점마다 바뀌지 않음
  - ② 나눗셈은 동차좌표 규칙이 대신 해줌
    - $(fX, fY, Z)$를 동차좌표로 읽으면 $(fX/Z,\ fY/Z)$와 같은 점 → Section 2의 결과와 같음

  - ③ 그래서 합성이 가능해짐
    - 월드 → 카메라 → 이미지 평면 → 픽셀의 중간 단계가 전부 행렬이 됨 → 곱해서 **행렬 하나로 합칠 수 있음**
    - 나눗셈은 끝에서 딱 한 번만 하면 됨
- 덤으로 얻는 것들
  - 평행이동(translation)도 행렬곱으로 표현 가능 → Extrinsics 편에서 $[R \mid t]$를 쓰는 이유
  - 여러 변환을 행렬곱으로 차례로 합성할 수 있음
  - $\tilde{w} = 0$인 점 = <span style="color:red">**무한원점**</span> (point at infinity), 즉 3D의 **방향**
    - 방향 $(d_X, d_Y, d_Z, 0)$을 투영하면 $(f d_X / d_Z,\ f d_Y / d_Z)$ → 이게 그 방향 직선들의 소실점

## 4. Projection as a Matrix
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

- 동차좌표로 쓰면 perspective projection이 3×4 행렬 하나가 됨:

  $$\begin{bmatrix} \tilde{x} \\ \tilde{y} \\ \tilde{w} \end{bmatrix} = \begin{bmatrix} f & 0 & 0 & 0 \\ 0 & f & 0 & 0 \\ 0 & 0 & 1 & 0 \end{bmatrix} \begin{bmatrix} X \\ Y \\ Z \\ 1 \end{bmatrix} = \begin{bmatrix} fX \\ fY \\ Z \end{bmatrix}$$

  - 정규화하면 $x = \tilde{x}/\tilde{w} = fX/Z$, $y = fY/Z$ → Section 2와 일치
- 이 행렬은 두 부분으로 쪼갤 수 있음:

  $$\begin{bmatrix} f & 0 & 0 & 0 \\ 0 & f & 0 & 0 \\ 0 & 0 & 1 & 0 \end{bmatrix} = \underbrace{\begin{bmatrix} f & 0 & 0 \\ 0 & f & 0 \\ 0 & 0 & 1 \end{bmatrix}}_{\text{초점거리}} \underbrace{\begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \end{bmatrix}}_{[I \mid 0]}$$

  - $[I \mid 0]$: 3D 점을 그대로 두고 동차좌표의 마지막 1만 버림 → 점이 **이미 카메라 좌표계에 있다고 가정**한 것
  - $\mathrm{diag}(f, f, 1)$: 초점거리만큼 스케일
- 아직 남은 한계 두 가지
  - 결과 $(x, y)$는 이미지 평면 위의 **물리 좌표**(mm, principal point가 원점)이지 **픽셀 좌표가 아님** → $\mathrm{diag}(f, f, 1)$이 픽셀 변환까지 포함한 $K$로 확장됨 → **Intrinsics 편**
  - 점이 **카메라 좌표계** 기준이어야 함 → 월드 좌표에서 카메라 좌표로 옮기는 $[R \mid t]$가 $[I \mid 0]$ 자리에 들어감 → **Extrinsics 편**
