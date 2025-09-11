---
title: DirectX12 자산 초기화
date: 2025-09-11 21:00:00 +0900
categories: [Graphics, DirectX12]
tags: [Graphics, DirectX12]     # TAG names should always be lowercase
---

MSDN Sample에서 분류하는 방법처럼 우선 그래픽스 파이프라인과 Asset 관련 함수를 분리한데에 맞춰서 포스팅도 기본적으로 분리하고 시작한다.    

우선은 현재 지식이 얕아 판단이 불가능한 일부 영역에 대해서는 실제 코드를 구현한 순서로 나열하고, 정확하고 실무적인 지식이 더 쌓인 후에 포스팅을 수정하는 것으로 한다.    

초기화 라인에서 순서를 정하는 것이 의미가 있는가가 판단이 서지 않는다.     
    
Command Allocator - Command List를 한 쌍으로 본다고 하면 PSO를 제외하고, 개별적인 느낌이 강하기 때문이다. 여러 강의 또는 사람들의 의견을 참고해서 정리해보겠다.

## 1. Command Allocator 생성
제출하고자 하는 Command Queue의 타입과 추후 명령을 제출할 Command List의 타입에 맞춰 Device를 이용해 Command Allocator를 생성한다. 

## 2. Root Signature 생성
Descriptor Table, Descriptor, 루트 상수를 바인딩하여 Signature 설명자를 초기화하고, 생성하는 순서로 동작한다.

## 3. Shader 생성
1. IDxcLibrary 생성 (GUID : CLSID_DxcLibrary) 
2. IDxcBlobEncoding을 이용하여 Shader File에 대한 Blob 데이터 생성 
3. IDxcIncludeHandler 생성 
4. IDxcCompiller 생성 (GUID : CLSID_DxcCompiler)
5. IDxcCompiller->Compile을 이용하여 셰이더 파일 컴파일 IDxcOperationResult에 결과 반환
6. IDxcOperationResult를 이용하여 상태 확인 후 결과를 ID3DBlob에 반환

블로그나 다양한 DirectX12 관련 내용을 수집하다 보면 D3DCompiler를 이용한 셰이더 컴파일이 대중화 되어 있다. 하지만 fxc.exe를 사용하는 버전의 라이브러리라서 셰이더 모델 6.0부터는 지원이 되지 않는 것으로 확인했다.      

#### 정확한 정보를 찾아보지 못해서 그럴지도 모른다.     

레이트레이싱 기능도 추가해보고 싶어서 DXC를 누겟 패키지를 이용하여 설치하였다.      
DXC(DirectX Shader Compiler) 누겟 패키지를 설치하고, 처음 맞이한 문제는 기존에 사용하던 함수인 D3DCompiler 기반 D3DCompileFromFile 함수를 대체해야 한다는 사실이었다.    

어려운 내용은 아니라서 Microsoft 샘플과 위키를 찾아보고 동일한 매개변수로 컴파일 가능하도록 위에서 설명한 순서에 맞춰 함수를 추가하였고, 솔루션 탐색기에서 프로젝트에 셰이더 파일을 추가한 경우에도 정상적인 컴파일 완료되는 것까지 확인하였다.      
   
참조 : [MSDN DirectXShaderCompiler Wiki](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll)

## 4. PSO(Pipeline State Object) 생성 (개념 구축 진행 중)
#### 커맨드 리스트를 만드는데 들어가지만 루트 서명을 nullptr로 설정하고 나중에 설정이 가능하기도 하고 그러면 어디서 만드는 게 옳은 건가??

## 5. 업로드 힙 생성 및 리소스 제출


## 6. 펜스 및 이벤트 생성
렌더링 및 리소스 업로드 상황에서 CPU & GPU 동기화를 위한 객체를 생성한다.
HANDLE, ID3D12Fence 타입의 객체