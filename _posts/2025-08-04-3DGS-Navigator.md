---
title: 3DGS Navigator
# description: Examples of text, typography, math equations, diagrams, flowcharts, pictures, videos, and more.
author: young0tete
date: 2024-03-20
categories: [Paper Review, 3DGS]
tags: [Navigator, 3DGS]
pin: false
math: true
mermaid: true
# image:
#   path: /commons/devices-mockup.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices. #caption
---
<!-- 
{% include wiki-infobox.html 
    title="지킬(Jekyll)" 
    label1="개발 언어" data1="Ruby"
    label2="용도" data2="정적 사이트 생성기" 
%} -->

## 0. Foundation Model
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

- **3DGS** [SIGGRAPH 2023 \| [review]( {% link _posts/2024-03-23-3DGS.md %}) \| [project page](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/)]

## 1. Dynamic Scene Reconstruction
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

### 1.1 Offline per-scene optimization
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

#### 1) Deformation Network based
{: style="background-color:#87CEEB; color:#000000; padding:4px 8px; border-radius:6px;" }

- **D3DGS** [CVPR 2024 \| review \| project page]
- **4DGS** [CVPR 2024 \| review \| project page]

#### 2) Temporal Function based
{: style="background-color:#87CEEB; color:#000000; padding:4px 8px; border-radius:6px;" }

- **Spacetime Gaussian** [CVPR 2024 \| review \| project page]
- **Taylor Gaussian** [CVPR 2025 \| review \| project page]

### 1.2 Online Streaming
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

- **3DGStream** [CVPR 2024 \| review \| project page]
- **Instant Gaussian Stream** [CVPR 2025 \| review \| project page]
- **4DGC** [CVPR 2025 \| review \| [project page](https://mediax-sjtu.github.io/4dgc/)]

## 2. Feed-Forward Gaussian Splatting (Generalizable)
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

### 2.1 pixel aligned Gaussian
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

- **pixelSplat** [CVPR 2024 \| review \| [project page](https://davidcharatan.com/pixelsplat/)]
- **MVSplat** [ECCV 2024 (Oral) \| review \| project page]
- **DepthSplat** [CVPR 2025 \| review \| project page]

### 2.2 Large Reconstruction Model (LRM)
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

- **GS-LRM** [ECCV 2024 \| review \| project page]
- **Long-LRM** [ICCV 2025 \| review \| project page]
- **LVT** [SIGGRAPH Asia 2025 \| review \| project page]

### 2.3 Others
{: style="background-color:#bbbbbb; color:#000000; padding:4px 8px; border-radius:6px;" }

- **GPS-Gaussian** [review \| project page]

## 3. Scene Editing
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

## 4. Avatar Reconstruction
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }


<!-- [review \| project page] -->
<!-- [review \| paper] -->