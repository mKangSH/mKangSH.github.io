---
title: 언리얼 네트워크
date: 2025-10-14 12:00:00 +0900
categories: [Game, Server]
tags: [Programming, C++, Game, Server, Unreal Engine]
---

## 언리얼 네트워크
단일 코드로 클라와 서버를 동시에 제작 가능하도록 하여 온라인 게임을 빠르게 개발 가능하도록 만들어 줌. 

### 핵심 기능
- Replication : Server의 액터를 Client에 전파하는 방식    
  Server -> Client 단방향 통신   
  Blueprint 기준으로 Actor의 Replicates 옵션 On   

  기본적으로는 생성 / 소멸 즉 클래스 디폴트 값으로 복제하여 싱크를 맞춤   
  - 프로퍼티 복제 : 액터의 멤버 변수의 값(데이터)을 동일하게 복제하는 방식     
    Replication 옵션을 RepNotify로 설정하면 OnPropertyChanged 이벤트 설정하여 값이 변경되는 상황 감지 가능
  - 컴포넌트 복제 : 이동 컴포넌트 등 액터에 붙어있는 요소와 함께 복제하는 방식

- RPC (Remote Procedure Call) : 원격 함수 호출

## 리슨 서버와 데디케이티드 서버
입력과 렌더링의 유무가 차이점

### 리슨 서버
서버의 호스팅 주체: 일반 유저 (Server가 됨)
협력 게임 경쟁이 필요 없으며, 방장이 방을 파고 사용자들이 접속해서 플레이할 때 사용
해킹에 취약하고, 일반 유저 컴퓨터 상태에 의존하게 됨.

### 데디케이티드 서버
공정성이 중요하며 경쟁이 필요한 게임에 주로 사용

## 언리얼 엔진 리슨 서버의 특징
서버가 되는 클라이언트는 다른 클라이언트와 달리 추가 액터를 가진다. 
- GameMode 
- 타인의 PlayerController 
- NetworkManager 
- GameSession 
- 타인의 PlayerCameraManager

## 언리얼 네트워크 실행 순서
리슨 서버는 로컬 플레이어가 서버 플레이어로 빙의 및 서버 + 클라이언트 역할
데디케이티드 서버는 아래 순서가 나눠져서 서버, 클라이언트 분리

### 서버 실행 순서
1. 월드 생성
2. 게임의 규칙과 진행을 맡는 게임모드 생성
3. 게임 스테이트, 플레이어 스테이트 생성
4. 플레이어 입장
5. 플레이어 컨트롤러 생성
6. 폰 생성
7. 빙의

### 클라이언트 (로컬 플레이어) 실행 순서
1. 로컬 플레이어의 플레이어 컨트롤러를 이용하여 서버의 폰 플레이어 컨트롤러에 입력 전달
2. 서버가 플레이어에 상태 복제

## 클라이언트 / Server 구분 
1. Enum ENetMode를 통해 현재 실행되는 프로그램의 NetMode 체크
    - NM_Standalone
    - NM_DedicatedServer
    - NM_ListenServer
    - NM_Client

2. 권한을 이용하여 체크 
    - HasAuthority() : 연출에만 사용하는 일부 Cosmetic Actor의 경우 클라이언트에서 권한을 가지고 있는 예외가 있음

3. NetRole
    - Server : Authority
    - Client : Autonomous Proxy(직접 제어), Simulated Proxy(제어하지 않는 액터)   

    GetLocalRole() : 현재 구동되는 프로그램에서의 액터 역할 확인      
    GetRemoteRole() : 서버에서 호출할 경우 클라이언트에서의 액터 역할 확인, 클라이언트에서 호출하는 경우 서버에서의 액터 역할 확인 