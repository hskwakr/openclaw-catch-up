# 事前調査フェーズ（2026-03-21〜03-24）

実機セットアップの前に行った調査ノート一式。OpenClawの理解 → 構成選定 → セットアップ準備の流れで進めた。

## 結論

- **フレームワーク**: ZeroClaw（deny-by-default、Rust製、軽量）
- **メインLLM**: Codex OAuth（ChatGPT Plus $20/月で定額運用）
- **チャネル**: Discord
- **インストール**: Homebrew管理
- **Docker**: 使わない（16GBではメモリがタイト）
- **Ollama**: まずCodexだけで始める

詳細はルートの [PROGRESS.md](../../PROGRESS.md) の「決まっていること」を参照。

## ノート一覧

### OpenClawの理解

| # | トピック | 要点 |
|---|---------|------|
| 01 | [概要](01-overview/) | OpenClawとは何か、規模感、作者の動向 |
| 02 | [コミュニティ](02-community/) | ガバナンス、エコシステム、論争（ClawHavoc等） |
| 03 | [アーキテクチャ](03-architecture/) | 5コンポーネント構成、スキル/ツール制御とセキュリティ |

### 構成選定

| # | トピック | 要点 |
|---|---------|------|
| 04 | [フォーク比較](04-forks-comparison/) | OpenClaw/NanoClaw/ZeroClaw/PicoClaw/Nanobot → ZeroClawが最有力 |
| 05 | [ローカルLLMとコスト](05-local-llm-cost/) | M4 16GBでは7Bがスイートスポット、Codex定額が有利 |
| 06 | [Docker運用](06-docker-operation/) | 16GBではメモリがタイト → Docker不使用に決定 |
| 07 | [Codexハイブリッド構成](07-codex-hybrid/) | ZeroClaw + Codex OAuth発見、Claude APIは設計レビュー限定 |

### セットアップ準備

| # | トピック | 要点 |
|---|---------|------|
| 08 | [セキュリティ対照比較](08-security-comparison/) | OpenClawの既知リスクの大部分をZeroClawは構造的に回避 |
| 09 | [Discord統合](09-discord-integration/) | 既知バグはすべて修正済み。Message Content Intentの有効化が必須 |
| 10 | [アップデート管理](10-update-management/) | Homebrewで統一管理が無難 |
| 11 | [セットアップ手順](11-setup-guide/) | Phase 1〜3の段階的セットアップ手順 |
