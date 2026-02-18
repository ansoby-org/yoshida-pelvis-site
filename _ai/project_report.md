# 吉田式 骨盤体操教室 サイト移行プロジェクト完了報告

本プロジェクトにおける全工程（Step 1〜5）の実施内容と結果を報告します。

---

## Step 1: Tailwind ビルド基盤の構築

Tailwind CSS (v4) を用いたビルド環境を構築しました。CDNを使用せず、Node.jsベースのモダンな開発体制を整えています。

### 実施内容
- **基盤セットアップ**: `package.json` を作成し、`tailwindcss`、`postcss`、`autoprefixer` を導入。
- **設定ファイル**: `tailwind.config.js`、`postcss.config.js` を作成。
- **CSS構成**: `assets/css/input.css` を作成し、Hugoのパイプラインで `main.css` を生成・圧縮するスクリプト (`npm run tw:build`) を実装。
- **Hugoテンプレート**: `layouts/partials/head.html` にCSS読み込み処理を追加。

---

## Step 2: LPコンテンツの移植 (トップページ)

既存のHTML LPをHugoのテンプレート構造に合わせて移植し、運用しやすい形に再構築しました。

### 実施内容
- **テンプレート分割**: `header.html`、`footer.html` に共通パーツを切り出し。
- **メインコンテンツ**: `layouts/index.html` にHero、Benefits、Teacher、Mechanism、FAQセクションを実装。
- **問い合わせ導線の刷新**: 静的フォームを廃止し、**LINE公式アカウントへの誘導ボタン**を主要CTAとして配置。予備としてGoogleフォームへのリンクを設置。
- **デザイン移行**: カスタムCSSを `assets/css/input.css` に統合し、Tailwindビルドで一元管理。
- **パラメータ管理**: `hugo.toml` にサイト情報やURL設定を集約。

---

## Step 3: ブログ機能の実装

お知らせやコラムを配信するためのブログ機能を実装しました。

### 実施内容
- **記事管理**: `content/blog/` 配下にMarkdownファイルで記事を作成可能に。サンプル記事を作成済み。
- **一覧・詳細ページ**: `layouts/blog/list.html` (一覧) と `layouts/blog/single.html` (詳細) を作成。
- **タイポグラフィ**: `@tailwindcss/typography` を導入し、Markdown本文が自動的に美しくスタイルされるよう設定。

---

## Step 4: お問い合わせページの実装

高齢男性ユーザーにも分かりやすい、LINE中心のお問い合わせ動線を構築しました。

### 実施内容
- **専用ページ**: `/contact/` (`content/contact/_index.md`) を作成。
- **LINE連携**:
    - 「友だち追加」→「テンプレートコピー」→「送信」の3ステップで案内。
    - 予約・相談用のメッセージテンプレート（氏名、希望日時など）を掲載。
    - `{{< linebutton >}}` ショートコードを作成し、どこでもLINEボタンを呼び出し可能に。
- **QRコード**: `static/img/line-qr.png` (プレースホルダ) を配置。運用時は実画像に差し替え推奨。

---

## Step 5: GitHub Pages 自動デプロイ

GitHub Actions を利用した自動デプロイパイプラインを構築しました。

### 実施内容
- **Workflow作成**: `.github/workflows/pages.yml` を実装。`main` ブランチへのpush契機でビルド＆デプロイが走ります。
- **設定**: `hugo.toml` の `baseURL` を GitHub Project Pages 用に設定（要ユーザー名書き換え）。

### デプロイ手順（初回）
1. GitHubにリポジトリを作成。
2. ローカルコードをpush。
3. GitHubリポジトリ設定の **Pages > Source** を **GitHub Actions** に変更。

---


---

## トラブルシューティング

### デザインが崩れる（スタイルが当たらない）場合
Tailwind CSS v4 (`@tailwindcss/cli`) を使用しているため、クラスの検知設定は `assets/css/input.css` 内の `@source` ディレクティブで管理されています。
もし新しいディレクトリを追加した場合は、`input.css` に `@source` を追記してください。

```css
@source "../../layouts";
@source "../../content";
/* 新しいパスがあれば追記 */
```
また、`tailwind.config.js` は v4 では必須ではありません（本プロジェクトでは互換性のため残していますが、実質的な設定は `input.css` が優先されます）。
