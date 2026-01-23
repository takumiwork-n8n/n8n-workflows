# Product Fetch Loop (Pagination & Logging)

## 概要
`dummyjson.com` からページネーションを使用して製品データを取得し、サブワークフローで処理を行うn8nワークフローです。
レート制限を遵守しながらGoogleスプレッドシートに処理ステータスをログ出力し、Slackで完了通知を行います。

## 特徴
- **Do-Whileループ**: `if` ノードとループバック接続を使用した堅牢なループ処理。
- **レート制限対応**: `Wait` ノードによるAPI負荷制御。
- **構成管理**: `set-config` ノードによる設定値の一元管理。
- **エラーハンドリング**: 外部Global Error Handlerとの連携を想定。

## 構成
### 主なノード
- **Set Config**: 設定値（Spreadsheet ID, Slack Channel）の定義。
- **Fetch Products**: HTTP Requestによるデータ取得。
- **Save Products (Sub)**: データ保存処理（サブワークフロー `xhPCAUqTr1HSDa7D`）の呼び出し。
- **Extract Pagination Info**: レスポンスから `skip`, `limit`, `total` を抽出。
- **Log to Sheet**: 処理状況をGoogle Sheetsに追記。
- **Has Next?**: 次ページの有無を判定し、ループを制御。

### 必要なCredential
- **Slack API**: 通知用。
- **Google Sheets OAuth2 API**: ログ出力用。

## 使用方法
1. `set-config` ノードで `sheet_id` と `slack_channel_name` を自身の環境に合わせて設定します。
2. サブワークフロー（ID: `xhPCAUqTr1HSDa7D`）がデプロイされていることを確認します。
3. ワークフローを実行します。

## 評価結果 (2026-01-23)
- **Grade**: A (Production Ready)
- **Status**: Verified by n8n-expert skill.
