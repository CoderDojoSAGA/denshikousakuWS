# CoderDojo Saga 公式ウェブサイト / KidsVenture ワークショップ LP

佐賀で開催される小・中学生のための「電子工作＆プログラミング」ワークショップ（KidsVenture in 佐賀）および CoderDojo Saga の公式ウェブサイトリポジトリです。

## 概要

本リポジトリは、はんだごてを使った IchigoJam の組み立てから、BASIC言語を用いたプログラミングまでを1日で体験できるワークショップの紹介・案内ページを管理しています。

## ディレクトリ構成

- `index.html` : トップページ（ワークショップのランディングページ）
- `assets/` : サイト内で使用する静的ファイル群
  - `images/` : ロゴや写真などの画像アセット
- `AGENT_INSTRUCTIONS.md` : 開発者・AIエージェント向けの詳細な運用ルールやプロジェクト要件

## 技術スタック

- **HTML5**
- **Tailwind CSS** (CDN経由での読み込み)
- **Vanilla JavaScript** (トグル処理やスクロール連動アニメーション等、必要最低限の実装)
- **外部フォント**: Google Fonts (`DotGothic16`, `Zen Kaku Gothic New`, `Press Start 2P`)

※ Node.jsやビルドツール（Webpack, Vite等）、ReactなどのJSフレームワークは使用せず、ブラウザのみで動作するシンプルな構成を採用しています。

## 開発・プレビュー方法

特別なビルド手順は不要です。お使いのブラウザで直接 `index.html` を開くか、VS Codeの `Live Server` などのローカルサーバー機能を使用してプレビューしてください。

## 運用・開発ルール

本プロジェクトの機能追加やページ追加、画像アセットの管理ルールについては [AGENT_INSTRUCTIONS.md](./AGENT_INSTRUCTIONS.md) を必ずご確認ください。
