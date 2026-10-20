# Arcana Duel

[English](README.md), [Korean](README.ko.md), [Japanese](README.ja.md)

プログラマー1名とグラフィック担当1名で制作した、マルチプレイヤー魔法バトルゲームです。

プロジェクト概要、担当した実装、開発デモをまとめた公開ポートフォリオです。チームのソースリポジトリは非公開のままです。

**2025.08 - 2026.02, 2名：プログラマー1名、グラフィック1名**

**プログラミング担当：C++サーバー・Unityゲーム連携**

C++, IOCP, DB worker, Unity

[プロジェクト詳細](https://ysl-ryudo.github.io/ja/work/magicwar/)

[![Arcana Duel — Arcana Duel](https://i.ytimg.com/vi/MJo1y09kzd4/hqdefault.jpg)](https://www.youtube.com/watch?v=MJo1y09kzd4)

## 担当した実装

IOCPワーカー、セッション・バッファー・Overlappedプール、独立したDBワーカーを構成しました。魔法・クールダウン、試合・プロフィール・ランク、Unityとの連携を担当しました。

| 領域 | 実装 |
|---|---|
| ネットワーク処理 | 接続受付・IOCPワーカーによる送受信完了処理 |
| リソース寿命 | セッション・バッファー・Overlapped I/Oのプール |
| DB処理 | 専用DBワーカーでデータベース要求を処理 |
| ゲーム連携 | 魔法・クールダウン・試合・プロフィール・ランクとUnityを連携 |

## 開発デモ動画

- [Arcana Duel](https://www.youtube.com/watch?v=MJo1y09kzd4)
- [Early development demo](https://www.youtube.com/watch?v=8sKpA2H9Ib4)

## ソースへのアクセスとクレジット

- [magicwarserver](https://github.com/YSL-RyuDo/magicwarserver) — 非公開・アクセス権が必要
- [magicwar](https://github.com/YSL-RyuDo/magicwar) — 非公開・アクセス権が必要

上記は私が担当したプログラミングの範囲です。グラフィック、音声、外部素材、他のメンバーの作業は元のクレジットを保持します。動画は撮影時点の記録であり、現行版の負荷試験結果を示すものではありません。
