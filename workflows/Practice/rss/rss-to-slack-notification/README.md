# Practice0117 Rss feed to Slack

このワークフローは、RSSフィードから最新の記事を取得し、Slackの指定したチャンネルへ自動通知するための練習用テンプレートです。

## ワークフローの概要

1.  **手動トリガー**: 「Execute Workflow」をクリックすることで実行されます。
2.  **設定 (Workflow Configuration)**: 取得先のRSS URLと通知先のSlackチャンネル（例: `#ニュース通知`）を定義します。
3.  **RSS読み取り**: 指定されたURL（デフォルトでは note の特定フィード）から最新情報を取得します。
4.  **件数制限**: 最新の5件のみに絞り込みます。
5.  **データ整形**: Slack通知用にタイトル、概要、リンクなどのフィールドを整理します。
6.  **Slack通知**: 整形されたメッセージをSlackへ送信します。

## 設定方法

1.  **Workflow Configurationノード**: 
    - `noteRssUrl` を購読したいRSS URLに変更してください。
    - `slackChannel` を通知先のチャンネル名に合わせて変更してください。
2.  **Slackノード**:
    - 自身のSlack認証情報（Credentials）を選択して連携させてください。

## 使用しているノード
- Manual Trigger
- Set (Configuration & Data Edit)
- RSS Feed Read
- Limit
- Slack
