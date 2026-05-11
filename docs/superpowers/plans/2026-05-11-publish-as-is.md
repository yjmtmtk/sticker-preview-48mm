# 48mm シールプレビュー公開 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 既存の単一HTML製ホログラムカードプレビューをライブラリ化せず GitHub に公開し、GitHub Pages で配信できる状態にする。

**Architecture:** ソース改変なし。ルートに `.gitignore`/`LICENSE`/書き換えた `README.md` を追加し、`git init` → `gh repo create` → push → GitHub Pages 有効化、の順で素直に進める。

**Tech Stack:** git, GitHub CLI (`gh`), GitHub Pages（`main` ブランチのルート公開）

---

## File Structure

- Create: `.gitignore` — `.DS_Store` のみ無視
- Create: `LICENSE` — MITライセンス、著者 `yjmtmtk`、年 `2026`
- Modify: `README.md` — 全面書き換え（既存のURLパラメータ表・機能説明は内容を保持しつつ、公開URLとAI組み込み利用の案内を追加）
- 触らない: `index.html`, `assets/*`（`assets/sample.zip` は DL機能に必要なので必ず含める）
- 触らない: `docs/superpowers/`（spec/planは含めて公開してOK）

---

## Task 1: `.gitignore` 作成

**Files:**
- Create: `/Users/tomotakayajima/Desktop/yjm/git/card/.gitignore`

- [ ] **Step 1: ファイルを作成**

内容:

```
.DS_Store
```

- [ ] **Step 2: 確認**

Run: `cat /Users/tomotakayajima/Desktop/yjm/git/card/.gitignore`
Expected: `.DS_Store` の1行のみ

---

## Task 2: `LICENSE` 作成（MIT）

**Files:**
- Create: `/Users/tomotakayajima/Desktop/yjm/git/card/LICENSE`

- [ ] **Step 1: ファイルを作成**

内容（MIT License、著者 yjmtmtk、年 2026）:

```
MIT License

Copyright (c) 2026 yjmtmtk

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

- [ ] **Step 2: 確認**

Run: `head -3 /Users/tomotakayajima/Desktop/yjm/git/card/LICENSE`
Expected: 1行目 `MIT License`、3行目 `Copyright (c) 2026 yjmtmtk`

---

## Task 3: `README.md` 全面書き換え

**Files:**
- Modify: `/Users/tomotakayajima/Desktop/yjm/git/card/README.md`（既存内容を全削除して以下に置換）

- [ ] **Step 1: 新内容で完全上書き**

新しい `README.md` の内容:

````markdown
# 48mm シールプレビュー (Holographic Card Previewer)

ビックリマンシール風のホログラフィックカードをブラウザ上でシミュレーション・プレビューするWebツールです。
Three.jsとシェーダーで、光の当たり方によるホログラムの変化をリアルタイムに表現します。

**🌐 公開URL: https://yjmtmtk.github.io/sticker-preview-48mm/**

ブラウザで上記URLを開くだけで動きます。インストールやビルドは不要です。

---

## 主な機能

- **ホログラムシミュレーション**: シェーダーによる虹色の干渉エフェクト
- **3Dプレビュー**: マウスドラッグで回転、ホイール／ピンチでズーム
- **画像アップロード**: 自分の画像を表面・背面・マスク・ホログラムパターンに即反映
- **サンプル素材ダウンロード**: 試作用のサンプル画像一式（zip）をワンクリックでDL
- **URLパラメータ制御**: GET引数で初期状態や画像URLを指定可能（埋め込み・共有向け）

---

## 使い方

### そのまま使う

[https://yjmtmtk.github.io/sticker-preview-48mm/](https://yjmtmtk.github.io/sticker-preview-48mm/) をブラウザで開いてください。
右上の設定パネル（lil-gui）から各種パラメータの調整や画像の読み込みができます。

### 自分のサイトに組み込みたい人へ

このリポジトリには「組み込み用ライブラリ」や「npmパッケージ」は用意していません。
代わりに、`index.html` をそのまま **AI（ChatGPT / Claude / Gemini など）に渡してください**。

```
このHTMLを参考に、私のサイトの〇〇ページに同じカードプレビューを埋め込んで。
カラー画像は××.png、背面画像は△△.png を使って。
```

といった指示で、AIが文脈に合わせて統合してくれます。
ソース全体が単一HTMLで完結しているため、AIによる読解・移植が容易です。

もしくは、`index.html` をダウンロードして自由に改変・流用してください（MITライセンス）。

### iframe で埋め込む

URLパラメータを使えば、iframeで簡単に埋め込めます:

```html
<iframe
  src="https://yjmtmtk.github.io/sticker-preview-48mm/?gui=0&colorMap=https://example.com/front.png&backMap=https://example.com/back.png"
  width="600" height="600" frameborder="0">
</iframe>
```

---

## URLパラメータ（GET引数）

URLの末尾にクエリパラメータを付与することで、初期状態やGUIの表示を制御できます。
例: `index.html?gui=0&autoRotate=0&colorMap=assets/sticker-front.png`

### 表示制御

| パラメータ | 説明 | 値 | デフォルト |
| :--- | :--- | :--- | :--- |
| `gui` | 設定パネルの表示 | `1` (表示) / `0` (非表示) | `1` |

### アニメーション・描画設定

| パラメータ | 説明 | 値 | デフォルト |
| :--- | :--- | :--- | :--- |
| `autoRotate` | カードの自動回転 | `1` (ON) / `0` (OFF) | `1` |
| `lighting` | ライティング（影・反射） | `1` (ON) / `0` (OFF) | `1` |
| `hologram` | ホログラム効果の有効化 | `1` (ON) / `0` (OFF) | `1` |
| `pattern` | ホログラムパターンの種類 | `放射状`, `ノイズ`, `カスタム` | `放射状` |

### 画像URL指定

以下のパラメータで、初期読み込み時の画像をURLで指定できます。
※外部URLの場合、サーバー側でCORSが許可されている必要があります。

| パラメータ | 説明 |
| :--- | :--- |
| `colorMap` | 表面のカラー画像URL |
| `monoMap` | 表面のマスク画像URL（透明部分の制御用） |
| `patternMap` | ホログラムパターン画像のURL |
| `backMap` | 背面画像のURL |

---

## サンプル素材

`assets/sample.zip` にサンプル画像一式が入っています。
GUIの「💾 sampleダウンロード」ボタン、または以下のURLから直接DLできます:

[https://yjmtmtk.github.io/sticker-preview-48mm/assets/sample.zip](https://yjmtmtk.github.io/sticker-preview-48mm/assets/sample.zip)

含まれるもの:
- `sticker-front.png`: 表面サンプル
- `sticker-back.png`: 背面サンプル
- `sticker-map.png`: マスクサンプル（透明部分の指定用）
- `prizm-pattern.png`: ホログラムパターンサンプル

---

## 技術スタック

- [Three.js](https://threejs.org/) r128 (WebGL 3D Rendering)
- [lil-gui](https://lil-gui.georgealways.com/) 0.19 (Control Interface)
- Vanilla HTML/CSS/JavaScript（ビルドツール不使用）

外部ライブラリは CDN 参照、依存ゼロでファイルを開けば動きます。

---

## ライセンス

[MIT License](LICENSE) © 2026 yjmtmtk

商用・改変・再配布いずれも自由です。
````

- [ ] **Step 2: 確認**

Run: `head -5 /Users/tomotakayajima/Desktop/yjm/git/card/README.md`
Expected: 1行目が `# 48mm シールプレビュー (Holographic Card Previewer)`、4行目に公開URLが含まれる

---

## Task 4: Git 初期化と初回コミット

**Files:**
- Create: `.git/` ディレクトリ（自動）

- [ ] **Step 1: 既に git 初期化されていないか確認**

Run: `ls -la /Users/tomotakayajima/Desktop/yjm/git/card/.git 2>&1 | head -2`
Expected: `ls: ...No such file or directory` （まだgit化されていないことを確認）

もし既に `.git/` がある場合は次のステップに進む前にユーザーに確認すること。

- [ ] **Step 2: git init（main ブランチで初期化）**

Run: `cd /Users/tomotakayajima/Desktop/yjm/git/card && git init -b main`
Expected: `Initialized empty Git repository in /Users/tomotakayajima/Desktop/yjm/git/card/.git/`

- [ ] **Step 3: コミット対象を確認**

Run: `cd /Users/tomotakayajima/Desktop/yjm/git/card && git status`
Expected: 以下のファイルが untracked として表示される
- `.gitignore`
- `LICENSE`
- `README.md`
- `assets/` (5ファイル: prizm-pattern.png, sample.zip, sticker-back.png, sticker-front.png, sticker-map.png)
- `docs/`
- `index.html`

`.DS_Store` が表示されないこと（gitignoreされている）。

- [ ] **Step 4: 全ファイルを add（明示的に）**

Run: `cd /Users/tomotakayajima/Desktop/yjm/git/card && git add .gitignore LICENSE README.md index.html assets/ docs/`

- [ ] **Step 5: ステージ確認**

Run: `cd /Users/tomotakayajima/Desktop/yjm/git/card && git status --short`
Expected: 全て `A` (added) 状態。`.DS_Store` は含まれない。

- [ ] **Step 6: 初回コミット**

Run:
```bash
cd /Users/tomotakayajima/Desktop/yjm/git/card && git commit -m "$(cat <<'EOF'
Initial commit: 48mm holographic sticker previewer

Three.js + GLSL shader でホログラム干渉を再現する 48mm シール用
プレビューツール。単一 HTML で動作。

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```
Expected: コミットが作成される。出力に `[main (root-commit) ...]` が含まれる。

---

## Task 5: GitHub リポジトリ作成と push

**Files:**
- Create: GitHub上に `yjmtmtk/sticker-preview-48mm` リポジトリ

- [ ] **Step 1: 同名リポジトリが既存でないか確認**

Run: `gh repo view yjmtmtk/sticker-preview-48mm 2>&1 | head -3`
Expected: エラー（`Could not resolve to a Repository`）。既に存在する場合はユーザーに確認。

- [ ] **Step 2: GitHub に public リポジトリを作成して push**

Run:
```bash
cd /Users/tomotakayajima/Desktop/yjm/git/card && gh repo create sticker-preview-48mm \
  --public \
  --description "ビックリマンシール風 48mm ホログラムカードプレビュー (Three.js)" \
  --source=. \
  --remote=origin \
  --push
```
Expected: `https://github.com/yjmtmtk/sticker-preview-48mm` が表示され、push 成功。

- [ ] **Step 3: リポジトリが作成され push されたことを確認**

Run: `gh repo view yjmtmtk/sticker-preview-48mm --json url,visibility,defaultBranchRef`
Expected: JSONで `url`、`visibility: PUBLIC`、`defaultBranchRef.name: main` が確認できる。

---

## Task 6: GitHub Pages 有効化

**Files:**
- 変更: GitHub上のリポジトリ設定（ローカルファイルは触らない）

- [ ] **Step 1: GitHub Pages を main ブランチのルートで有効化**

Run:
```bash
gh api -X POST repos/yjmtmtk/sticker-preview-48mm/pages \
  -f "source[branch]=main" \
  -f "source[path]=/"
```
Expected: JSON応答で `"status": "queued"` または `"status": "building"`、`html_url` に `https://yjmtmtk.github.io/sticker-preview-48mm/` が含まれる。

エラー `409 Conflict`（既に有効）の場合は次へ。

- [ ] **Step 2: Pages の状態確認**

Run: `gh api repos/yjmtmtk/sticker-preview-48mm/pages | grep -E '"(status|html_url)"'`
Expected: `html_url` が `https://yjmtmtk.github.io/sticker-preview-48mm/`、`status` が `queued` / `building` / `built` のいずれか。

- [ ] **Step 3: ビルド完了を待ってから動作確認**

Pages の初回ビルドは 30秒〜数分かかる。下記をリトライ:

Run: `curl -sI https://yjmtmtk.github.io/sticker-preview-48mm/ | head -1`
Expected: `HTTP/2 200` （404 の場合はビルド未完。1〜2分待ってリトライ）

- [ ] **Step 4: ユーザーに公開URLを通知**

公開URL: `https://yjmtmtk.github.io/sticker-preview-48mm/`
リポジトリURL: `https://github.com/yjmtmtk/sticker-preview-48mm`

完了通知音を鳴らす:
Run: `~/.claude/bin/noti.sh done` (バックグラウンド実行)
