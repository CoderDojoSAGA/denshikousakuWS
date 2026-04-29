# CoderDojo Saga 公式ウェブサイト プロジェクト要件・運用ルール

本ドキュメントは、CoderDojo Saga（CoderDojo さが）の紹介ウェブサイトを実装・運用するためのAIエージェントおよび開発者向けのガイドラインです。

## 1. プロジェクト概要
- **目的**: CoderDojo Sagaの紹介ウェブサイト（公式サイト）の作成
- **対象リポジトリ**: `denshikousakuWS` (公開対象: `coderdojo-saga.github.io`)

## 2. 実装要件

### 2.1 ページ構成
現在はすべての情報を1ページに集約した**シングルページ（ランディングページ / LP）構成**となっています。
各コンテンツは同一ページ内のセクションとして実装し、ナビゲーションからアンカーリンク（`#xxx`）で遷移させます。

**主なセクション構成（`index.html` 内）**
1. **Hero**: メインビジュアル
2. **QUEST** (`#quest`): ワークショップの内容（AM/PM）
3. **MAP** (`#day`): 1日のタイムテーブル
4. **INFO** (`#details`): 開催概要、対象、参加費など
5. **VENUE** (`#venue`): 会場アクセス情報
6. **VOICE** (`#voice`): 過去の活動・レポート
7. **FAQ** (`#faq`): よくある質問
8. **SPONSOR / CONTACT** (`#sponsor`, `#contact`): スポンサー募集および申し込み案内

### 2.2 技術スタック
- **基本技術**: HTML と Tailwind CSS (CDN経由) を標準とする。(`index.html` 内での `<script src="https://cdn.tailwindcss.com"></script>` および設定スクリプトによる運用)
  - **【備考】CSSの非分離について**: Tailwind CSSをCDN経由で利用する制約上、`@apply`等を用いたカスタムCSSは外部ファイル（`style.css`等）に分離せず、必ず `index.html` 内の `<style type="text/tailwindcss">` に記述すること（外部ファイル化するとCDNスクリプトがパースできずスタイルが適用されません）。
- **JavaScriptの制限**: Reactなどのフレームワークや大規模ライブラリは極力使用せず、機能の実現は必要最低限のバニラJS（IntersectionObserverや簡単なトグル処理など）のみを使用すること。

### 2.3 機能要件（今後の拡張予定）
現在の `index.html` では未実装ですが、今後以下の機能を必要に応じて追加していく想定です。

- **言語切替機能（日英）**:
  - 英語と日本語の表示切替を実装する。
  - URLパラメータやLocalStorageに依存せず、HTML/CSSの `<input type="radio">` と `display: none` を用いた手法などを検討する。
- **テーマ切替機能（カラーモード）**:
  - ライトモード / ダークモード / システム（デバイス設定追従）の切替機能。
  - 現在はTailwindの設定スクリプトにより独自テーマで固定しているが、今後切り替えを実装する場合は `prefers-color-scheme` や CSS変数を活用する。

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

### 4.1 イベントページの運用・管理
本サイトはシングルページ（LP）構成となっているため、次回以降のイベント開催時には以下の運用を行います。

1. **メインLP（`index.html`）の更新**:
   新しいイベントが決定した際は、現在の `index.html` の内容（Hero、QUEST、MAP、VENUE、INFO等）を新しいイベント情報に上書きして運用します。
2. **過去のイベントのアーカイブ**:
   過去のイベント情報をそのまま残したい場合は、上書きする前の `index.html` を `events/YYYYMMDD.html` のような別ファイルとして退避させます。
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

### 4.3 開発・PR作成ルール
本リポジトリに対してPull Request (PR) を作成する際は、タイトルおよび説明文を含め、**必ず日本語で記述**してください。
