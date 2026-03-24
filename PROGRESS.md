# OpenClaw キャッチアップ

## 決まっていること

- **環境**: M4 Mac Mini 16GB
- **チャネル**: Discord を通してAIエージェントとやり取りする
- **フレームワーク**: ZeroClaw を第一候補とする（セキュリティ対照比較の結果、既知リスクの大部分が構造的に回避されているため。ただし第三者監査未実施は留意。サンドボックスはPR #3989で統合済み）
- **メインLLM**: Codex OAuth（ChatGPT Plusサブスク $20/月）で定額運用
- **コスト方針**: Claude APIの従量課金は避ける。使うとしても設計・レビューに限定し、月間上限を設定する
- **Claude API**: 使うならSonnet、設計・レビュー専用のサブエージェントとして設定。Anthropicコンソールで月間上限$15-20を設定（ノート07）。ただし「使わない」選択肢も残っており、最終判断は実機検証後
- **セキュリティ**: ZeroClawの deny-by-default アクセス制御をベースに
- **Ollama**: まずCodexだけで始める。必要を感じたら追加する（Codex OAuthが定額で使えるため、ローカルLLMの実用的メリットは限定的）
- **Docker**: 使わない（16GBではメモリがタイト。ZeroClawのdeny-by-default + サンドボックス統合で対応）
- **インストール・更新**: Homebrewで管理（`brew install zeroclaw` → `brew upgrade zeroclaw`）。セルフアップデートコマンドとの併用は避ける（ノート10）

## 決まっていないこと

- **具体的なモデル**: gpt-5.4がメイン候補だが、gpt-5.4-miniが使えるかは実機検証で確認予定（ノート11 Phase 1）

## 調査で判明した重要事項

- Claudeサブスクの自動利用は規約違反（2026/2 Anthropic明確化）→ Claudeを使うならAPI従量課金一択
- ZeroClaw Discord統合の既知バグはすべて修正済み。セットアップ時はMessage Content Intentの有効化が必須（ノート09）
- **セキュリティ**: 第三者監査は両プロジェクトとも未実施。ZeroClawのCVE 0件は「まだ十分に調べられていない」可能性あり（ノート08）

## 次にやること

1. **実機検証** — ノート11のPhase 1〜3に沿ってセットアップを実行し、以下を確認する:
   - Codex OAuthで利用可能なモデルとレート制限
   - Discord経由でのquery_classification動作
   - Issue #2537（model_routesチャネル反映）の影響有無

## まだ気になっていること

- ZeroClawのWebSocket origin検証の実装状況（localhostバインドでもCSWSHが理論上可能 — ノート08）
- Claude APIを使う場合の上限設定手順（Claudeの使い方が決まってから調査）

## 調査済み（解決済み）

過去の項目は [meta/progress/2026-03.md](meta/progress/2026-03.md) に退避済み。

## ノート

- [01-overview/](notes/01-overview/) — 概要（2026-03-21）
- [02-community/](notes/02-community/) — ガバナンス、エコシステム、論争（2026-03-21）
- [03-architecture/](notes/03-architecture/) — アーキテクチャ概要、スキル・ツール制御とセキュリティ（2026-03-22）
- [04-forks-comparison/](notes/04-forks-comparison/) — 軽量フォーク比較（2026-03-23）
- [05-local-llm-cost/](notes/05-local-llm-cost/) — ローカルLLMとコスト検討（2026-03-23、03-24修正）
- [06-docker-operation/](notes/06-docker-operation/) — Docker運用の調査と懸念事項（2026-03-24）
- [07-codex-hybrid/](notes/07-codex-hybrid/) — Codexハイブリッド構成とコスト管理（2026-03-24）
- [08-security-comparison/](notes/08-security-comparison/) — セキュリティ対照比較: OpenClaw vs ZeroClaw（2026-03-24）
- [09-discord-integration/](notes/09-discord-integration/) — Discord統合の安定性確認（2026-03-24）
- [10-update-management/](notes/10-update-management/) — アップデートとバージョン管理の比較（2026-03-24）
- [11-setup-guide/](notes/11-setup-guide/) — セットアップ手順（ZeroClaw + Codex OAuth + Discord）（2026-03-24）

## 運用ルール

- [フォルダ構成ルール](meta/001-folder-structure.md)
- [ファクトチェック用プロンプト](meta/prompts/fact-check.md)
- [PROGRESS見直し用プロンプト](meta/prompts/progress-review.md)
- 過去ログ → [meta/progress/](meta/progress/)

---
*最終更新: 2026-03-25（PROGRESS見直し: Claude APIの方針を「決まっていること」に移動、「決まっていないこと」を整理）*
