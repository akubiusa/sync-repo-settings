# GEMINI.md

## 目的

このファイルは Gemini CLI 向けのコンテキストと作業方針を定義します。

## 出力スタイル

- 言語: 日本語
- トーン: 技術的かつ簡潔
- 形式: Markdown

## 共通ルール

- 会話は日本語で行う
- PR とコミットは [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) に従う（`<description>` は日本語）
- 日本語と英数字の間には半角スペースを入れる

## プロジェクト概要

- 目的: 複数の GitHub リポジトリの設定を一括で同期・管理する
- 主な機能:
  - リポジトリ基本設定（マージ方法、自動削除など）の適用
  - ワークフロー権限の設定
  - Actions 変数（Copilot Firewall 等）の設定
  - ルールセットの作成・更新
  - Copilot Code Review の有効化
- 対象: tomacheese, book000 配下のリポジトリ

## コーディング規約

- **言語**: Bash
- **依存ツール**: `gh` CLI, `jq`
- **エラー処理**: `set -euo pipefail` を使用
- **変数**: ダブルクォートで囲む
- **コメント言語**: 日本語
- **エラーメッセージ言語**: 英語

## 開発コマンド

```bash
# ドライラン（実際には適用しない）
./apply-settings.sh --dry-run

# 全ターゲットに適用
./apply-settings.sh

# 特定オーナーのみ処理
./apply-settings.sh --target tomacheese

# 特定リポジトリのみ処理
./apply-settings.sh --repo tomacheese/my-repo

# Markdown 形式で出力
./apply-settings.sh --dry-run --markdown

# 詳細ログ
./apply-settings.sh --verbose
```

## 主要ファイル

| ファイル | 説明 |
|----------|------|
| `apply-settings.sh` | メイン適用スクリプト |
| `config.json` | 設定ファイル（ターゲット、デフォルト設定、フィルタルール） |
| `.github/workflows/sync.yml` | 定期実行ワークフロー |
| `.github/workflows/pr-preview.yml` | PR プレビューワークフロー |

## 注意事項

- `PERSONAL_ACCESS_TOKEN` は Repository Secrets で管理し、コミットしない
- トークンをログに出力しない
- 既存のコーディングルールやプロジェクト固有のルールを優先する

## リポジトリ固有

- `config.json` の `rules` でフィルタルールを定義する
- フォークリポジトリ、アーカイブ済みリポジトリは自動スキップ
- ステータスチェックは PR トリガーのワークフローから自動検出
- `finished-*` ジョブがある場合はそちらを優先
