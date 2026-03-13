# ActivityWatch × AI Skills

[ActivityWatch](https://activitywatch.net/) のデータを [Claude Code](https://docs.anthropic.com/en/docs/claude-code) に読み込ませて、日報生成・生産性分析・週次振り返りを自動で行うカスタムスキル集です。

## できること

| スキル | コマンド | 内容 |
|---|---|---|
| **Daily Report** | `/daily-report` | 今日の作業内容を自動で日報にまとめる |
| **Productivity Check** | `/productivity-check` | 集中スコア（0〜100点）の算出と改善提案 |
| **Weekly Review** | `/weekly-review` | 今週の振り返りレポート（先週比較付き） |

### 特徴

- **職種を問わない** — エンジニア、PM、デザイナー、ビジネス職など、どの職種でも正確に分析できます
- **マシン非依存** — ActivityWatch のバケット名を API から動的に取得するため、どの環境でもそのまま使えます
- **AI による改善提案付き** — 単なるログのまとめではなく、コンテキストスイッチの頻度や反復作業の検出に基づいた具体的な効率化アドバイスを提供します

## 前提条件

### 1. ActivityWatch

[ActivityWatch](https://activitywatch.net/) は、PC上でどのアプリ・ウィンドウをどれだけ使っていたかを自動記録する OSS のタイムトラッカーです。

- データはすべてローカルに保存され、外部に送信されません
- macOS / Windows / Linux に対応

**インストール:**

```bash
# macOS (Homebrew)
brew install --cask activitywatch

# その他の OS
# https://activitywatch.net/downloads/ からダウンロード
```

**起動:**

インストール後、ActivityWatch を起動してください。メニューバー（macOS）またはシステムトレイ（Windows/Linux）にアイコンが表示されます。

ActivityWatch が起動している状態で、ブラウザから http://localhost:5600 にアクセスすると Web UI が開きます。

> **重要:** スキルの実行時には ActivityWatch が起動している必要があります（API サーバーが `localhost:5600` でリッスンしている状態）。

### 2. Claude Code

[Claude Code](https://docs.anthropic.com/en/docs/claude-code) は Anthropic が提供する CLI ツールです。

**インストール:**

```bash
npm install -g @anthropic-ai/claude-code
```

## セットアップ

```bash
git clone https://github.com/keyiiiii/activewatch-ai-skills.git
cd activewatch-ai-skills
```

## 使い方

1. ActivityWatch が起動していることを確認する
2. このリポジトリのディレクトリで Claude Code を起動する

```bash
cd activewatch-ai-skills
claude
```

3. スラッシュコマンドを実行する

```
> /daily-report
> /productivity-check
> /weekly-review
```

## スキル詳細

### `/daily-report`

今日の ActivityWatch データから日報を自動生成します。

出力例:
- アクティブ時間
- 作業サマリー（ウィンドウタイトルから作業内容を推測）
- アプリ別使用時間
- AI からの一言コメント

### `/productivity-check`

今日の生産性をスコアリングし、改善提案を行います。

分析内容:
- **集中スコア（0〜100点）** — ディープワーク比率、コンテキストスイッチ頻度、割り込み回数などから算出
- **時間配分** — 開発 / ドキュメント・分析 / デザイン・企画 / コミュニケーション / AI ツール 等のカテゴリ別集計
- **改善サジェスト** — データに基づいた具体的な効率化提案
- **効率化ヒント** — 反復的な作業パターンの検出と自動化の提案

### `/weekly-review`

今週（月曜〜今日）の振り返りレポートを生成します。先週のデータとの比較も行います。

出力例:
- 日別アクティブ時間のグラフ
- 今週やったことのプロジェクト別まとめ
- カテゴリ別時間（先週比付き）
- 来週への改善提案

## 仕組み

```
ActivityWatch (localhost:5600)
    │
    │  REST API でデータ取得
    │
    ▼
Claude Code (スキル実行)
    │
    │  ウィンドウタイトル・アプリ名から
    │  作業内容を推測・分類・分析
    │
    ▼
レポート出力（日報 / スコア / 週次振り返り）
```

1. ActivityWatch API (`http://localhost:5600/api/0/`) からバケット一覧を取得
2. ウィンドウアクティビティと AFK（離席）データを取得
3. アプリ名・ウィンドウタイトルから作業内容を推測し、カテゴリ分類
4. 集計・分析結果をレポートとして出力

## ライセンス

MIT
