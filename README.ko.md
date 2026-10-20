# Arcana Duel

[English](README.md), [Korean](README.ko.md), [Japanese](README.ja.md)

프로그래머 1명과 그래픽 1명이 함께 만든 멀티플레이 마법 전투 게임입니다.

프로젝트 소개, 담당 작업과 개발 시연을 정리한 공개 포트폴리오입니다. 팀의 원본 코드 저장소는 비공개로 유지합니다.

| 프로젝트 정보 | 내용 |
|---|---|
| 기간과 팀 구성 | 2025.08 ~ 2026.02, 2명, 프로그래머 1명, 그래픽 1명 |
| 내 역할 | 프로그래밍 담당, C++ 서버, Unity 게임 연동 |
| 사용 기술 | C++, IOCP, DB worker, Unity |

[프로젝트 상세](https://ysl-ryudo.github.io/ko/work/magicwar/)

[![Arcana Duel, Arcana Duel](https://i.ytimg.com/vi/MJo1y09kzd4/hqdefault.jpg)](https://www.youtube.com/watch?v=MJo1y09kzd4)

## 담당 작업

### magicwarserver

연결 수락, IOCP 워커와 수신, 송신 처리를 구현했습니다.

세션, 버퍼, overlapped 풀과 네트워크 처리에서 분리한 DB 워커를 작업했습니다.

마법, 쿨다운, 매치, 플레이어 프로필, 랭크 관련 게임 서비스를 구현했습니다.

### magicwar

게임, 네트워크 연동과 클라이언트 마법 전투 시스템을 작업했습니다.

피해, 회복, 순간이동, 투명화, 보호막, 벽과 둔화 효과가 소스에 있으며, 대응하는 게임 서비스는 별도 서버에서 처리합니다.

소스 이력에 남아 있는 오디오, UI 기여와 외부 에셋의 원래 크레딧을 유지합니다.

## 구현과 설계

멀티플레이 마법 전투에서는 송수신 처리, 재사용할 연결 자원과 DB를 사용하는 게임 서비스가 함께 동작합니다. 서버에서 각 책임이 어떻게 나뉘는지 보여주는 사례입니다.

IOCP 워커, 세션, 버퍼, Overlapped 풀과 별도 DB 워커를 구성했습니다. 마법, 쿨다운 서비스, 경기, 프로필, 랭크와 Unity 게임, 네트워크 연동을 작업했습니다.

| 영역 | 구현 |
|---|---|
| 네트워크 작업 | 연결 수락, IOCP 워커의 송수신 완료 처리 |
| 자원 수명 | 세션, 버퍼, Overlapped I/O 자원 풀 |
| DB 작업 | 별도 DB 워커에서 데이터베이스 요청 처리 |
| 게임 연동 | 마법, 쿨다운, 경기, 프로필, 랭크와 Unity 연동 |

## 개발 시연 영상

[Arcana Duel](https://www.youtube.com/watch?v=MJo1y09kzd4)

[Early development demo](https://www.youtube.com/watch?v=8sKpA2H9Ib4)

## 소스 접근과 크레딧

[magicwarserver](https://github.com/YSL-RyuDo/magicwarserver), 비공개, 접근 권한 필요

[magicwar](https://github.com/YSL-RyuDo/magicwar), 비공개, 접근 권한 필요

아래 링크는 설명의 근거가 되는 코드 버전과 변경 기록입니다. 원본 비공개 저장소의 접근 권한이 필요합니다.

| 저장소 | 구현 기록 |
|---|---|
| magicwarserver | [IOCP implementation](https://github.com/YSL-RyuDo/magicwarserver/blob/48daa832f20fab13309e17a4a7af5e283b52d355/projectMagicWar/IOCP_GameServer.cpp) |
| magicwarserver | [개발 이력](https://github.com/YSL-RyuDo/magicwarserver/commits/main/?author=YSL-RyuDo) |
| magicwar | [개발 이력](https://github.com/YSL-RyuDo/magicwar/commits/main/?author=YSL-RyuDo) |

위 내용은 제가 담당한 프로그래밍 범위입니다. 그래픽, 음향, 외부 에셋과 다른 팀원의 작업은 원래 크레딧을 유지합니다. 영상은 촬영 당시의 개발 기록이며 현재 버전의 부하 시험 결과를 의미하지 않습니다.
