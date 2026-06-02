---
title: "[Benchmark] Tanks and Temples"
# description: Examples of text, typography, math equations, diagrams, flowcharts, pictures, videos, and more.
author: young0tete
date: 2025-10-10
categories: [Benchmark, 3Drecon]
tags: [Benchmark, 3Drecon]
pin: false
math: true
mermaid: true
# image:
#   path: /commons/devices-mockup.png
#   lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#   alt: Responsive rendering of Chirpy theme on multiple devices. #caption
---

> Tanks and temples: benchmarking large-scale scene reconstruction <br>
> Intel Labs <br>
> ACM ToG 2017 <br>
> [ [Download](https://www.tanksandtemples.org/download/) | [Paper](https://dl.acm.org/doi/10.1145/3072959.3073599)]

8개의 intermediate scene, 6개의 advanced scene, 7개의 추가 데이터(training data)로 구성됨.

| | | |
| Family | Francis | Horse |
| | | |
| Lighthouse | M60 | Panther |
| | <img src="/assets/images/benchmark/T&T_train.jpg" alt="" style="width:100%; display:block; margin:auto;" /> | |
| Playground | Train | Auditorium |
| | | |
| Ballroom | Courtroom | Museum |
| | | |
| Palace | Temple | Barn |
| | | |
| Caterpillar | Church | Courthouse |
| | | <img src="/assets/images/benchmark/T&T_truck.jpg" alt="" style="width:100%; display:block; margin:auto;" /> |
| Ignatius | Meeting room | Truck |



## Intermediate Scenes
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

### Family
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Francis
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Horse
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Lighthouse
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### M60
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Panther
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Playground
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Train
{: color:#000000; padding:4px 8px; border-radius:6px;" }

- 원본 해상도 : 1920x1080
- 3DGS 해상도 : 980x545

| Method | PSNR   | SSIM  | LPIPS | Training Time | GPU     | Storage(MB) | Source | 
| 3DGS   | 21.097 | 0.802 | 0.218 | -             | A6000   | -           | 3DGS   |
| 3DGS   | 22.073 | 0.821 | 0.196 | 14m 05s       | RTX3090 | 259         | 직접실험 |


## Advanced Scenes
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

### Auditorium
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Ballroom
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Courtroom
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Museum
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Palace
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Temple
{: color:#000000; padding:4px 8px; border-radius:6px;" }

## Training Scenes
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

### Barn
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Caterpillar
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Church
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Courthouse
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Ignatius
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Meeting room
{: color:#000000; padding:4px 8px; border-radius:6px;" }

### Truck
{: color:#000000; padding:4px 8px; border-radius:6px;" }

- 원본 해상도 : 1920x1080
- 3DGS 해상도 : 979x546

| Method | PSNR   | SSIM  | LPIPS | Training Time | GPU     | Storage(MB) | Source | 
| 3DGS   | 25.187 | 0.879 | 0.148 | -             | A6000   | -           | 3DGS   |
| 3DGS   | 25.479 | 0.885 | 0.142 | 16m 53s       | RTX3090 | 484         | 직접실험 |


## Methods
{: style="background-color:#ffeaa7; color:#000000; padding:4px 8px; border-radius:6px;" }

- [3DGS](../3DGS): Train, Truck scene에 대해 수행