# CoderDojo Saga 公式ウェブサイト プロジェクト要件・運用ルール

本ドキュメントは、CoderDojo Saga（CoderDojo さが）の紹介ウェブサイトを実装・運用するためのAIエージェントおよび開発者向けのガイドラインです。

## 1. プロジェクト概要
- **目的**: CoderDojo Sagaの紹介ウェブサイト（公式サイト）の作成
- **対象リポジトリ**: `denshikousakuWS` (公開対象: `coderdojo-saga.github.io`)

## 2. 実装要件

### 2.1 ページ構成
サイトは以下の構成（複数のHTMLファイル、またはサブディレクトリ内の各子ページ）とします。

1. **トップページ** (`index.html`)
2. **About Us (私たちについて)** (`about.html` または `about/index.html`)
3. **Social Media** (`social.html` または `social/index.html`) ※各SNSへのリンクまたは埋め込み
4. **Contact (お問い合わせ)** (`contact.html` または `contact/index.html`)

### 2.2 技術スタック
- **基本技術**: HTML と Tailwind CSS (CDN経由) を標準とする。(`index.html` 内での `<script src="https://cdn.tailwindcss.com"></script>` および設定スクリプトによる運用)
- **JavaScriptの制限**: Reactなどのフレームワークや大規模ライブラリは極力使用せず、機能の実現は必要最低限のバニラJS（IntersectionObserverや簡単なトグル処理など）のみを使用すること。

### 2.3 機能要件
- **言語切替機能（日英）**:
  - 英語と日本語の表示切替を実装すること。
  - URLパラメータやLocalStorageに依存せず、HTML/CSSの `<input type="radio">` と `display: none` を用いた手法、または言語ごとの別リンクを設置する手法で実装する。
- **テーマ切替機能（カラーモード）**:
  - ライトモード / ダークモード / システム（デバイス設定追従）の切替機能を実装すること。
  - `prefers-color-scheme` と CSSカスタムプロパティ（変数）を軸とする。
  - 手動切替は `<input type="radio">` （チェックボックスハック等）を利用し、`body`タグ等のテーマ属性を切り替える形で実装する（JS非依存を推奨）。

## 3. 掲載リソース（各種リンク先情報）
サイト内に以下の情報を適切に配置・リンクしてください。

- **佐賀県産業スマート化センター** (コミュニティ紹介、イベント通知)
  - URL: https://www.saga-smart.jp/community/coderdojosaga/
- **Facebook** (イベント通知、イベント報告)
  - URL: https://www.facebook.com/CoderDojoSAGA/
- **Peatix** (イベント情報・募集)
  - URL: https://peatix.com/group/10488940
- **過去の活動記録**
  - 佐賀新聞: https://www.saga-s.co.jp/subcategory/%EF%BC%A3%EF%BD%8F%EF%BD%84%EF%BD%85%EF%BD%92%EF%BC%A4%EF%BD%8F%EF%BD%8A%EF%BD%8F%E3%81%95%E3%81%8C
  - 佐賀大学: https://www.saga-u.ac.jp/koho/education/2025072938104

## 4. 運用ルール

### 4.1 イベントページの作成・管理
今後新しいイベントページを作成する場合は、以下のルールを厳守すること：

1. **個別ページの作成**:
   各イベントの詳細は、開催日（年月日）をファイル名として作成する。
   - 例: `html/events/YYYYMMDD.html`
2. **一覧へのリンク追加**:
   作成した個別イベントページへのリンクを、必ずイベント一覧ページ（`html/events.html`）に追加すること。
3. **イベント用素材ファイルの格納**:
   企画書PDFや元画像などのイベントに関する素材ファイルは、個別のフォルダを作成して格納する。
   - 格納先: `assets/files/<イベント名>/`
   - **重要**: `assets/files/` ディレクトリは容量圧迫を防ぐため、`.gitignore` で一括して管理外（無視）設定とする。
4. **Web用公開画像の設定**:
   ロゴなどのサイト共通アセットや、Web上で公開すべき画像は適宜リサイズやフォーマット変換を行い、以下のディレクトリに配置して使用する (`index.html` の相対パス設定等に合わせる)。
   - 基本格納先: `assets/images/`
   - イベント固有の画像が増える場合は `assets/images/events/` 等に分けて管理する。

### 4.2 画像アセットの更新手続き（Favicon / OGP）
`favicon.ico` および `ogp.png` は、元画像（`assets/images/logo.png`）から `ImageMagick` を用いて生成されています。
※元画像 `logo.png` は生成用のソースファイルであるため `.gitignore` によりリポジトリ管理外としています。

画像の再生成や更新が必要な場合は、ターミナルで以下のコマンドを実行してください。

```bash
# favicon.ico の再生成
magick assets/images/logo.png -background transparent -define icon:auto-resize=64,48,32,16 favicon.ico

# OGP画像 (ogp.png) の再生成
magick assets/images/logo.png -resize 1200x630 -background white -gravity center -extent 1200x630 ogp.png
```
