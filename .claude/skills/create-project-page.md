# create-project-page

GitHub リポジトリの README.md を読み込み、ポートフォリオ用の `docs/projects/{ProjectName}/index.md` を生成し、`mkdocs.yml` の nav にも追記するスキル。

## 呼び出し方

```
/create-project-page https://github.com/{owner}/{repo}
```

## 実行手順

### 1. URL からプロジェクト情報を取得

引数として受け取った GitHub URL を解析する。

- `owner` = URL の3番目のセグメント
- `repo` = URL の4番目のセグメント（= プロジェクトディレクトリ名）

例: `https://github.com/yokamoto5742/VoicePhrase` → owner=`yokamoto5742`, repo=`VoicePhrase`

### 2. README.md を取得

WebFetch ツールを使い、以下の順で試みる（200 が返った方を使用）:

1. `https://raw.githubusercontent.com/{owner}/{repo}/main/README.md`
2. `https://raw.githubusercontent.com/{owner}/{repo}/master/README.md`

取得できない場合はその旨をユーザーに伝えて終了する。

### 3. index.md を生成

README の内容を分析し、**以下のテンプレート書式**に従って日本語で index.md を生成する。
README が英語の場合は日本語に翻訳・要約する。

```markdown
# {リポジトリ名}

**{一言でアプリを表すキャッチコピー}**

## 概要

{アプリが何をするかを 1〜3 文で。}

## 想定ユーザーと使用シーン

{対象ユーザー像と主な使用シーンを箇条書きで。}

**想定ワークフロー:** {典型的な使い方を 1 文で。}

## 特徴

1. **{特徴1のタイトル}** — {説明}
2. **{特徴2のタイトル}** — {説明}
3. **{特徴3のタイトル}** — {説明}
（README に記載がある分だけ列挙する）

---

[:fontawesome-brands-github: GitHub リポジトリ]({元の GitHub URL}){ .md-button }
```

生成ルール:
- キャッチコピーは太字 `**...**` で、動詞を使わず名詞句で表現する
- 特徴は README の Features / 機能 セクションから抽出する。記載がなければ概要から推測する
- 想定ユーザーが README に書かれていない場合は、アプリの性質から合理的に推測する
- コードブロック・バッジ・インストール手順は含めない

### 4. ファイルを書き込む

出力先: `docs/projects/{repo}/index.md`

- ディレクトリが存在しない場合は `Bash(mkdir -p docs/projects/{repo})` で作成する
- ファイルを Write ツールで書き込む

### 5. mkdocs.yml を更新する

`mkdocs.yml` の `nav:` → `プロジェクト:` セクションに以下の行を追加する:

```yaml
    - {プロジェクト表示名}: projects/{repo}/index.md
```

- **プロジェクト表示名** は README のタイトルや説明から日本語で決める（英語リポジトリ名をそのまま使わない）
- 既に同じパスのエントリがある場合は追加しない

### 6. 完了メッセージ

以下を出力して終了:

```
作成しました: docs/projects/{repo}/index.md
mkdocs.yml の nav に "{プロジェクト表示名}" を追加しました。
```
