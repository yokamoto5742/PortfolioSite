# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 概要

MkDocs + Material テーマで構築した静的ポートフォリオサイト。`main` ブランチへの push で GitHub Actions が自動的に GitHub Pages へデプロイする。

- 公開 URL: https://yokamoto5742.github.io/portfolio/
- パッケージ管理: uv (`pyproject.toml` / `uv.lock`)

## 開発コマンド

```bash
# 依存インストール（uv 使用）
uv sync

# ローカルプレビュー（ホットリロード付き）
uv run mkdocs serve

# 静的ビルド（site/ に出力）
uv run mkdocs build

# GitHub Pages への手動デプロイ
uv run mkdocs gh-deploy --force
```

## 構成

| パス | 役割 |
|---|---|
| `mkdocs.yml` | サイト設定・ナビゲーション・テーマ・拡張 |
| `docs/` | コンテンツ（Markdown） |
| `docs/assets/` | カスタム CSS・JS・画像 |
| `docs/projects/` | 各プロジェクト紹介ページ（サブディレクトリ構造） |
| `.github/workflows/deploy.yml` | CI/CD（push to main → `mkdocs gh-deploy`） |

## ページ追加手順

1. `docs/` 以下に Markdown ファイルを作成する。
2. `mkdocs.yml` の `nav:` セクションにエントリを追加する。

## コーディング規約（Python）

Python スクリプトを追加する場合は `.claude/rules/python-coding.md` に従う:

- PEP8・型ヒント必須
- 関数は 50 行以下を目標、単一責任
- コメントは分かりにくいロジックにのみ日本語で記述

## コミットメッセージ

`.claude/rules/commit.md` に従い、絵文字プレフィックス + 日本語で記述する:

```
✨ feat / 🐛 fix / 📝 docs / ♻️ refactor / ✅ test
```
