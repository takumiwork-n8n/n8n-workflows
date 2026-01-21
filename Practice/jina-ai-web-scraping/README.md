# jina-ai風scraping

## 概要
Google Gemini (Flash-lite) と Jina-reader (localhost経由) を活用して、Web記事の本文を抽出し、Markdown形式で要約および見出し案を生成するn8nワークフローです。

## 主な機能/ステップ
1. **Manual Trigger**: ワークフローの手動実行。
2. **HTTP Request**: `http://host.docker.internal:8787/fetch` を介して指定されたURLのコンテンツを取得。
3. **Clean HTML Noise (JavaScript)**: スクレイピングしたHTMLからscriptやstyleタグなどの不要な要素を削除し、トークンを節約。
4. **Basic LLM Chain (Google Gemini)**: 抽出されたHTMLから本文を抽出し、日本語Markdown形式で要約と見出し（5件）を作成。
5. **Set Node**: 最終的な出力を整理。

## 設定のポイント
- **Jina-readerのセットアップ**: ローカルの `8787` ポートで実行されているJina-readerサーバーを想定しています。
- **Google Gemini API**: 有効なAPIキーと、n8n上の `Google Gemini(PaLM) Api account` 資格情報が必要です。
- **評価レポート**: 同梱の `evaluation_report.md` には、設計上の課題と改善案が記載されています。

## 品質評価
- **総合スコア**: 2.3 / 5.0
- **課題**: URLがハードコードされている点、エラーハンドリング（リトライ処理など）が未実装である点。
- **改善案**: 入力変数の定義、エラー発生時の通知フローの追加。
