# OpenClaw キャッチアップ

## 決まっていること

- **環境**: M4 Mac Mini 16GB
- **フレームワーク**: ZeroClaw（deny-by-default、Rust製、軽量）
- **メインLLM**: Codex OAuth（ChatGPT Plus $20/月）で定額運用
- **チャネル**: Discord
- **インストール・更新**: Homebrewで管理（`brew install zeroclaw` → `brew upgrade zeroclaw`）
- **Docker**: 使わない（16GBではメモリがタイト）
- **Ollama**: まずCodexだけで始める。必要を感じたら追加
- **セキュリティ**: ZeroClawの deny-by-default アクセス制御をベースに
- **コスト方針**: Claude APIの従量課金は避ける。使うとしても設計・レビューに限定し、月間上限を設定
- **Claude API**: 使うならSonnet、Anthropicコンソールで月間上限$15-20。ただし「使わない」選択肢も残っており、最終判断は実機検証後
- **モデル**: onboardデフォルトの `gpt-5-codex` で動作確認済み（事前調査では `gpt-5.4` を想定していたが、実際の設定値は `gpt-5-codex`）

## 決まっていないこと

- （現時点ではなし）

## 次にやること

1. **Discord Bot設定の振り返り** — Developer Portal側の設定（Intent、権限など）を整理・記録する
2. **認証フォールバック手順の記録** — SSH経由でのdevice-codeフローの正確な手順をメモする
3. **Phase 3: model_routes + query_classification の設定** — セットアップ手順に沿って実行

## まだ気になっていること

- ZeroClawのWebSocket origin検証の実装状況（localhostバインドでもCSWSHが理論上可能）
- Claude APIを使う場合の上限設定手順（使い方が決まってから調査）

## ノート

- [00-pre-research/](notes/00-pre-research/) — 事前調査フェーズ（2026-03-21〜03-24、ノート01〜11）
- [01-hands-on/](notes/01-hands-on/) — 実機検証フェーズ（2026-03-28〜）

## 運用ルール

- [フォルダ構成ルール](meta/001-folder-structure.md)
- [プロンプト集](meta/prompts/)（セッション開始、ファクトチェック、PROGRESS見直し）
- 過去ログ → [meta/progress/](meta/progress/)

---
*最終更新: 2026-03-28（PROGRESS見直し: モデル確定を反映）*
