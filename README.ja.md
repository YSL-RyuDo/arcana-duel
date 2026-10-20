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

## コードの動作と実装構造の理由

確認した実装が扱う制約と利点・代償を説明します。設計効果の説明はコード分析であり、比較する全代案を当時試作したという意味ではありません。担当リンクは共同制作内の変更を示し、ソースの閲覧権限は維持します。

### DB待機とネットワーク完了処理を分ける理由

**実装の流れ**

DBWorkerはミューテックスで保護したキューにタスクを受け取り、条件変数で専用スレッドを起こします。取り出し時だけロックし、解除してからDB処理を実行します。試合結果の保存ではアカウント識別子をタスクに渡します。

**この構造が必要な理由**

DBはソケットの状態と無関係にストレージを待つことがあります。分離によりネットワーク完了スレッドをDB呼出し内で待たせる結合を減らせます。実行前にキューのロックを解除すれば、その処理中も次の要求を投入できます。識別子の受渡しは、遅れて実行される検索の元セッションへの依存も減らします。

**代案と現在の実装の折合い**

DBの直接呼出しはキュー不要ですが保存待ちと通信処理を結びます。複数ワーカーは並列度を上げる代わりに順序と接続の管理を複雑にします。専用ワーカー一つはここで使う単純な分離であり、キューの継続的増加は再検討の根拠になります。

**利点・代償と検証範囲**

単一DBワーカーは順次実行を簡単にしますが、要求がたまるとボトルネックになります。キュー上限、取消、終了時の残タスク完了は別の課題であり、確認したStop実装は全処理完了の保証ではありません。この分離だけでは実測スループットや複数DB更新の原子性も示せません。

[magicwarserver/DBWorker.cpp](https://github.com/YSL-RyuDo/magicwarserver/blob/8089751ae5136876d6fe133fdb980ef24c97684c/projectMagicWar/DBWorker.cpp), [magicwarserver/MatchManager.cpp](https://github.com/YSL-RyuDo/magicwarserver/blob/8089751ae5136876d6fe133fdb980ef24c97684c/projectMagicWar/MatchManager.cpp)

### 1回の受信を1メッセージと見なさない理由

**実装の流れ**

受信バイトをセッションバッファに蓄積し、PacketParserはヘッダーと指定された本文がそろうまで待ちます。完成したメッセージだけを振り分け、残りは次回に保持します。IOCP_Recvは受信バッファとoverlappedオブジェクトをプールへ戻し、次の受信を登録します。

**この構造が必要な理由**

TCPは順序のあるバイトストリームなので、送信と受信の境界は一致しません。通信とゲーム処理の間でメッセージ境界を復元する層が必要です。プールは受信経路の反復割当てを扱い、セッションごとの蓄積は未完のデータを正しい接続に対応させます。

**代案と現在の実装の折合い**

区切り文字方式は読みやすい反面、本文内の区切りのエスケープが必要です。固定長は解析が単純ですが無駄な領域や拡張制限が生じます。現在のヘッダー・本文長方式は可変データを扱う代わりに長さと未完バッファの検証が必要です。

**利点・代償と検証範囲**

層を増やす分、長さ、容量、オブジェクト寿命の管理が必要です。プール容量は割当て設定であり、実証された同時接続人数ではありません。ヘッダー分割、本文分割、複数メッセージの同時受信、I/O待機中の切断が検証対象になります。

[magicwarserver/PacketParser.cpp](https://github.com/YSL-RyuDo/magicwarserver/blob/8089751ae5136876d6fe133fdb980ef24c97684c/projectMagicWar/PacketParser.cpp), [magicwarserver/IOCP_Recv.cpp](https://github.com/YSL-RyuDo/magicwarserver/blob/8089751ae5136876d6fe133fdb980ef24c97684c/projectMagicWar/IOCP_Recv.cpp)

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
