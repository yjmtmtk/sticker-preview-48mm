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
