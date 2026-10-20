# Arcana Duel

[English](README.md), [Korean](README.ko.md), [Japanese](README.ja.md)

A multiplayer magic-combat game, built by one programmer and one graphics teammate.

Public portfolio: project overview, my contribution and development videos. The team's source repositories remain private.

**Aug 2025 - Feb 2026, 2 members: 1 programmer, 1 graphics**

**Programming: C++ server & Unity game integration**

C++, IOCP, DB worker, Unity

[Project page](https://ysl-ryudo.github.io/en/work/magicwar/)

[![Arcana Duel — Arcana Duel](https://i.ytimg.com/vi/MJo1y09kzd4/hqdefault.jpg)](https://www.youtube.com/watch?v=MJo1y09kzd4)

## My work

The server uses IOCP workers, session/buffer/overlapped pools and a separate DB worker. My work includes magic/cooldown services, matches, profiles and ranks, with Unity-side game/network integration.

| Area | Implementation |
|---|---|
| Network work | Accept connections; complete receives and sends through IOCP workers |
| Resource lifetime | Reusable session, buffer and overlapped-I/O pools |
| Database work | Database requests handled by a separate DB worker |
| Game integration | Magic, cooldowns, matches, profiles and ranks paired with Unity |

## Development videos

- [Arcana Duel](https://www.youtube.com/watch?v=MJo1y09kzd4)
- [Early development demo](https://www.youtube.com/watch?v=8sKpA2H9Ib4)

## Source access and credits

- [magicwarserver](https://github.com/YSL-RyuDo/magicwarserver) — Private, access required
- [magicwar](https://github.com/YSL-RyuDo/magicwar) — Private, access required

The scope above describes my programming contribution. Art, audio, third-party assets and other team members' work retain their original credits. Recorded demos show the project at the time of recording; they do not establish current load-test results.
