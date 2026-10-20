# Arcana Duel

[English](README.md), [Korean](README.ko.md), [Japanese](README.ja.md)

プログラマー1名とグラフィック担当1名で制作した、マルチプレイヤー魔法バトルゲームです。

プロジェクト概要、担当した実装、開発デモをまとめた公開ポートフォリオです。チームのソースリポジトリは非公開のままです。

| プロジェクト情報 | 内容 |
|---|---|
| 期間とチーム構成 | 2025.08 ~ 2026.02, 2名：プログラマー1名、グラフィック1名 |
| 担当 | プログラミング担当：C++サーバー・Unityゲーム連携 |
| 使用技術 | C++, IOCP, DB worker, Unity |

[プロジェクト詳細](https://ysl-ryudo.github.io/ja/work/magicwar/)

[![Arcana Duel, Arcana Duel](https://i.ytimg.com/vi/MJo1y09kzd4/hqdefault.jpg)](https://www.youtube.com/watch?v=MJo1y09kzd4)

## 担当した実装

### magicwarserver

接続受付、IOCPワーカー、送受信処理を実装しました。

セッション・バッファー・overlappedのプールと、ネットワーク処理から分離したDBワーカーを担当しました。

魔法、クールダウン、試合、プロフィール、ランクに関するゲームサービスを実装しました。

### magicwar

ゲーム・ネットワーク連携とクライアント側の魔法戦闘システムを担当しました。

ダメージ・回復、瞬間移動、透明化、バリア、壁、スロー効果が実装され、対応するサービスは別サーバーで処理します。

ソース履歴に記録されたオーディオ・UIの貢献と外部素材のクレジットを維持しています。

## 実装と設計

マルチプレイヤー魔法戦闘では送受信、再利用する接続リソース、DBを使うゲームサービスが共存します。サーバー内の責務の分け方を示す事例です。

IOCPワーカー、セッション・バッファー・Overlappedプール、独立したDBワーカーを構成しました。魔法・クールダウン、試合・プロフィール・ランク、Unityとの連携を担当しました。

| 領域 | 実装 |
|---|---|
| ネットワーク処理 | 接続受付・IOCPワーカーによる送受信完了処理 |
| リソース寿命 | セッション・バッファー・Overlapped I/Oのプール |
| DB処理 | 専用DBワーカーでデータベース要求を処理 |
| ゲーム連携 | 魔法・クールダウン・試合・プロフィール・ランクとUnityを連携 |

## 開発デモ動画

[Arcana Duel](https://www.youtube.com/watch?v=MJo1y09kzd4)

[Early development demo](https://www.youtube.com/watch?v=8sKpA2H9Ib4)

## ソースへのアクセスとクレジット

[magicwarserver](https://github.com/YSL-RyuDo/magicwarserver), 非公開・アクセス権が必要

[magicwar](https://github.com/YSL-RyuDo/magicwar), 非公開・アクセス権が必要

以下は説明の根拠となるコード版と変更記録です。元の非公開リポジトリの閲覧権限が必要です。

| リポジトリ | 実装記録 |
|---|---|
| magicwarserver | [IOCP implementation](https://github.com/YSL-RyuDo/magicwarserver/blob/48daa832f20fab13309e17a4a7af5e283b52d355/projectMagicWar/IOCP_GameServer.cpp) |
| magicwarserver | [開発履歴](https://github.com/YSL-RyuDo/magicwarserver/commits/main/?author=YSL-RyuDo) |
| magicwar | [開発履歴](https://github.com/YSL-RyuDo/magicwar/commits/main/?author=YSL-RyuDo) |

上記は私が担当したプログラミングの範囲です。グラフィック、音声、外部素材、他のメンバーの作業は元のクレジットを保持します。動画は撮影時点の記録であり、現行版の負荷試験結果を示すものではありません。
