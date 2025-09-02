---
title: GPU On WDDM
date: 2025-09-02 11:48:00 +0900
categories: [Hardware, GPU]
tags: [Hardware, Graphics Processing Unit]     # TAG names should always be lowercase
---

DirectX12 에서 GPU Engine의 정확한 정의를 알기 위한 포스팅

## WDDM (Window Display Driver Model)   
실행중인 모든 프로세스 간 GPU 추상화, 관리 및 공유하는 역할을 하는 그래픽 커널
  - GPU Scheduler (VidSch : Video Scheduler로 추측됨)
    GPU의 다양한 엔진을 사용하려는 프로세스 스케쥴링, 엔진 간 액세스 중재 및 우선순위 지정
  - Video Memory Manager(VidMm) - GPU Memory Manager     
    VRAM, Main DRAM 페이지를 포함한 GPU가 사용하는 모든 메모리 관리

## GPU Engine (엔진 간 병렬 처리 가능)
GPU에서 스케줄링이 가능하고 서로 병렬로 작동할 수 있는 독립적인 실리콘 단위 
  - Copy Engine : 데이터 전송
  - 3D Engine : 3D Rendering
  - Compute Engine : General Perpose 연산 (확인 필요)

일부 GPU의 경우 CPU Hyper Threading과 비슷한 개념으로 기본 코어를 공유하여 여러 엔진을 매핑함.
  - 코어는 실행 시 엔진 간 공간적, 시간적으로 분할

## GPU Core
GPU Engine을 구성하는 요소 그룹화된 엔진 단위로 스케줄링

윈도우 작업 관리자의 GPU 메모리 표현 방법은 레퍼런스 참고
## Reference
 - [Microsoft dev blog gpu-in-the-task-manager](https://devblogs.microsoft.com/directx/gpus-in-the-task-manager/)
