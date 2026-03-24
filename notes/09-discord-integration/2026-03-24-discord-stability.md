# ZeroClaw Discord統合の安定性確認

*作成日: 2026-03-24*

## 目的

ZeroClawのDiscordチャネル統合について、既知バグの現状と実用上のリスクを整理する。実機セットアップ前に「何が壊れていて何が直っているか」を把握しておく。

## 結論

Discord統合に関する主要なバグはすべて修正済み。セットアップをブロックする既知の問題は現時点で見当たらない。ただしセットアップ時の注意点がいくつかある。

## Issue #483: チャンネル返信が404エラー（解決済み）

- **報告日**: 2026-02-17 / **修正日**: 同日
- **重大度**: S1（ワークフローブロック）
- **原因**: `send()`に`channel_id`ではなく`msg.sender`（ユーザーID）を渡していた
- **修正**: PR #535 で`msg.channel`を使うよう修正。コミット `1908af3`
- **影響**: v0.1.7以降を使えば問題なし

## その他のDiscord関連バグ（すべて解決済み）

### Issue #574: 2000文字超でメッセージ送信失敗

- Discord APIの2000文字制限を超えるとBad Request
- `split_message_for_discord()`関数で改行→スペース境界での分割を実装（PR内、コミット`03c3ded`）
- Unicode・絵文字も正しく処理される

### Issue #2782: Discord経由でファイル操作ができない

- `auto_approve`に`file_write`/`file_edit`を設定しても「ツールがない」と返す
- PR #2836 で修正済み
- v0.1.7で発生 → 最新版で解消

### Issue #2686: Discord音声ファイルの文字起こしが動かない

- 重大度S2。Telegramでは動くがDiscordでは無音で失敗
- 原因: Discord添付ファイル処理が画像/テキストのみ対応で、音声をスキップしていた
- PR #2700 で音声検出・duration検証を追加して修正

### Issue #2365: ツール承認がテキストコマンドのみ

- `/approve-allow <id>`の手入力が必要で、IDのタイポで無音失敗する問題
- PR #2448 でDiscord Buttonコンポーネント（Approve/Deny）を実装
- テキストコマンドも引き続き利用可能

## PROGRESS.mdの「気になっていること」の解消状況

### Issue #2537: チャネル起動時にmodel_routesが反映されない → 解決済み

- `start_channels()`が`create_resilient_provider_nonblocking()`を呼ぶ際、`model_routes`を受け付けなかった
- agentコマンドでは動くがチャネルリスナーでは動かない不整合
- 修正済み: チャネル初期化をルーテッドプロバイダー構築に切り替え

### Issue #1646: フォールバック時のモデル名再マッピング → 解決済み

- プライマリプロバイダー障害時、フォールバック先に元のモデル名をそのまま送っていた
- 例: ZAIの`glm-5`がOpenRouterに送られ「無効なモデル」エラー
- PR #1652 でプロバイダー対応フォールバックモデルマッピングを実装

## セットアップ時の注意点

1. **Message Content Intent を必ず有効にする**: Discord Developer Portalで有効化しないと、接続→即切断の`Invalid Session (op 9)`エラーが繰り返される。最も頻度の高いセットアップ問題
2. **バージョン確認**: 上記バグの多くはv0.1.7〜v0.1.8で報告。最新リリースを使うこと
3. **ツール承認のUX**: Buttonコンポーネントが実装済みなので、テキスト手入力よりボタンを使うのが安全

## 残存リスク

- Discord統合のテストカバレッジが体系的に不足していた経緯がある（Issue #852で分析済み）。今後も新しいエッジケースが出る可能性はある
- WebSocket origin検証の実装状況は未確認のまま（ノート08で指摘済み、Discord統合固有ではなくZeroClaw全体のWebUI問題）

## ソース

- [Issue #483](https://github.com/zeroclaw-labs/zeroclaw/issues/483) — チャンネル返信404
- [Issue #574](https://github.com/zeroclaw-labs/zeroclaw/issues/574) — メッセージ分割
- [Issue #2782](https://github.com/zeroclaw-labs/zeroclaw/issues/2782) — ファイル操作不可
- [Issue #2686](https://github.com/zeroclaw-labs/zeroclaw/issues/2686) — 音声文字起こし
- [Issue #2365](https://github.com/zeroclaw-labs/zeroclaw/issues/2365) — 承認ボタン
- [Issue #2537](https://github.com/zeroclaw-labs/zeroclaw/issues/2537) — model_routesチャネル不整合
- [Issue #1646](https://github.com/zeroclaw-labs/zeroclaw/issues/1646) — フォールバックモデル名
- [Issue #852](https://github.com/zeroclaw-labs/zeroclaw/issues/852) — テストカバレッジ分析
