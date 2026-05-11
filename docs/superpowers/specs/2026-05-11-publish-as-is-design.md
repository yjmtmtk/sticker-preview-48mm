# 48mm シールプレビュー — そのまま公開する設計

## ゴール

現状の `index.html`（Three.js + シェーダーで動くホログラムカードプレビュー）と `assets/` 一式を、ライブラリ化せずそのまま GitHub に公開し、GitHub Pages でホストする。

## 方針（重要）

ライブラリ化（npm / Web Component / iframe 埋め込み等）はしない。理由は次の2点：

1. 単一HTMLで完結しているため、組み込みたい人は AI（ChatGPT/Claude等）にソースを渡せばすぐ自分のサイトに統合できる。API設計のオーバーヘッドが現代では割に合わない。
2. ターゲット層（HTMLが書けるデザイナー）にとって、抽象化されたAPIより「動く参照実装」の方が価値が高い。

## スコープ

### 作業内容

1. **`.gitignore` 作成**
   - `.DS_Store` を無視
   - `assets/sample.zip` は **除外しない**（DLボタンで配布する素材のため必要）

2. **`LICENSE` 追加（MIT）**
   - 著作者: tomotakayajima（GitHub: yjmtmtk）
   - 改変・組み込み自由を明示

3. **README 全面書き換え**
   - 公開URL（GitHub Pages）を冒頭に記載
   - 「組み込みたい人へ」セクション: 「`index.html` をAIに渡せば自分のサイトに統合できます」を明示
   - 既存の URLパラメータ表・機能説明・技術スタックは保持
   - サンプル素材ダウンロードへの言及を追加

4. **Git 初期化 → 初回コミット → GitHub に push**
   - リポジトリ名: `sticker-preview-48mm`
   - ユーザー: `yjmtmtk`（gh CLIで作成）
   - 公開リポジトリ（public）

5. **GitHub Pages 有効化**
   - `main` ブランチのルートを公開
   - `gh api` 経由で設定

### スコープ外

- ソースコード（index.html）の修正・リファクタ
- ライブラリ化、ビルドツール導入
- CI/CD、テスト
- カスタムドメイン

## 成果物

- 公開リポジトリ: `https://github.com/yjmtmtk/sticker-preview-48mm`
- 公開URL: `https://yjmtmtk.github.io/sticker-preview-48mm/`
- 利用者は URL を開くだけで動く / 組み込みたい人は AI に `index.html` を食わせる

## リスク・確認事項

- `assets/sample.zip` (3.4MB) を含めるためリポジトリが少し重くなるが、機能上必要なので許容
- 既存の `index.html` 内のCDN参照（three.js r128, lil-gui）はそのまま維持（GitHub Pages 上でも問題なく動く）
