---
title: 서버 기초
date: 2025-10-09 20:00:00 +0900
categories: [Game, Server]
tags: [Programming, C++, Game, Server]     # TAG names should always be lowercase
---

## 서버란 무엇인가
특수한 목적으로 사용하는 컴퓨터 가벼운 OS 설치되어 있음. 계속 실행되는 프로그램이 설치되어 네트워크 통신하여 서비스를 제공함.

Binding : IP와 Port를 연결하는 작업   
Listen : 연결을 받을 준비

## 웹 서버
Request - Response의 일정한 통신 규약을 가지고 있음    

__무상태성__ : 고객에 대한 정보를 가지지 않는다.     
__비연결성__: 클라이언트 - 서버 사이 연결은 유지되지 않고 1회성 통신 이후 종료

게임에서의 사용 예시    
- Single Game 점수 Web Server 랭킹 시스템 등     
- 비동기 PVP 게임 TCG 등      

언어별로 Web Framework를 제공함
- Web Framework : 웹 서버를 구현하기 위해 필요한 기능을 모아놓은 기반

## 실시간 서버 (Stateful Server)
클라이언트 요청에 따라 수동적인 반응만이 아닌 능동적으로 서버에서 클라이언트로 통신도 가능함.

언제든 패킷을 보낼 수 있음 = >__실시간 통신__ 이 가능하다.    
__상태성__ : 클라이언트의 상태를 가지고 있음.    
__연결 지향성__

프레임워크를 만들기가 어려워 직접 구축하여 사용. 특수 목적을 가짐. 

## 패킷과 직렬화 (Packet Serialization)
패킷 : 데이터 전송 규약, 일반적으로 Header (Packet ID + Size) + Payload (Data) 구성    
MMORPG는 50~100개 정도의 Packet 사용

직렬화 : 파편화된 데이터를 모으는 작업, Save, Replay, Network Packet등에 사용
