# Arcana Duel

[English](README.md), [Korean](README.ko.md), [Japanese](README.ja.md)

A multiplayer magic-combat game, built by one programmer and one graphics teammate.

Public portfolio: project overview, my contribution and development videos. The team's source repositories remain private.

| Project | Details |
|---|---|
| Period and team | Aug 2025 ~ Feb 2026, 2 members: 1 programmer, 1 graphics |
| My role | Programming: C++ server & Unity game integration |
| Stack | C++, IOCP, DB worker, Unity |

[Project page](https://ysl-ryudo.github.io/en/work/magicwar/)

[![Arcana Duel, Arcana Duel](https://i.ytimg.com/vi/MJo1y09kzd4/hqdefault.jpg)](https://www.youtube.com/watch?v=MJo1y09kzd4)

## My work

### magicwarserver

Implemented connection acceptance and IOCP workers with receive/send processing.

Worked on session/buffer/overlapped pools and a DB worker separated from network processing.

Implemented game services around magic, cooldowns, matches, player profiles and ranks.

### magicwar

Worked on the game/network integration and the client-side magic-combat systems.

The source includes damage/heal, blink, invisibility, barriers, walls and slow effects; the companion server handles the corresponding game services.

Original audio/UI contributions recorded in source history and third-party asset credits are retained.

## Implementation and design

Multiplayer magic combat combines receive/send handling, reusable connection resources and database-backed game services. Those responsibilities should be readable as separate parts of the server.

The server uses IOCP workers, session/buffer/overlapped pools and a separate DB worker. My work includes magic/cooldown services, matches, profiles and ranks, with Unity-side game/network integration.

| Area | Implementation |
|---|---|
| Network work | Accept connections; complete receives and sends through IOCP workers |
| Resource lifetime | Reusable session, buffer and overlapped-I/O pools |
| Database work | Database requests handled by a separate DB worker |
| Game integration | Magic, cooldowns, matches, profiles and ranks paired with Unity |

## Development videos

[Arcana Duel](https://www.youtube.com/watch?v=MJo1y09kzd4)

[Early development demo](https://www.youtube.com/watch?v=8sKpA2H9Ib4)

## Source access and credits

[magicwarserver](https://github.com/YSL-RyuDo/magicwarserver), Private, access required

[magicwar](https://github.com/YSL-RyuDo/magicwar), Private, access required

The following links identify the recorded source revisions. Access requires permission to the original private repositories.

| Repository | Implementation record |
|---|---|
| magicwarserver | [IOCP implementation](https://github.com/YSL-RyuDo/magicwarserver/blob/48daa832f20fab13309e17a4a7af5e283b52d355/projectMagicWar/IOCP_GameServer.cpp) |
| magicwarserver | [Development history](https://github.com/YSL-RyuDo/magicwarserver/commits/main/?author=YSL-RyuDo) |
| magicwar | [Development history](https://github.com/YSL-RyuDo/magicwar/commits/main/?author=YSL-RyuDo) |

The scope above describes my programming contribution. Art, audio, third-party assets and other team members' work retain their original credits. Recorded demos show the project at the time of recording; they do not establish current load-test results.
