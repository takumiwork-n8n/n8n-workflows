---
description: >
  n8nワークフローの全ライフサイクル（要件定義・設計・デプロイ・評価・保存）を管理するエキスパートスキル。
  n8n MCPツール（n8n-full / n8n-mcp）を駆使し、実機環境へのデプロイ、バリデーション、テスト実行までを直接操作する。
  発火キーワード: 「n8n 要件定義」, 「n8n 評価」, 「n8n 保存」, 「n8n expert」, 「n8n ワークフロー」, 「n8n デプロイ」
---

# n8n-expert

## Purpose / Outcome
n8nワークフローの導入検討から、詳細設計、実機へのデプロイ・検証、品質評価、そしてGitHub/Obsidianへの資産保存までを一気通貫で実行し、ユーザーの自動化プロセスを最高品質で維持・管理することを目的とする。

## Best Practices
- **Japanese First**: GitHub・Obsidianに出力するドキュメント（README等）は、常に日本語で生成する。
- **1 Node 1 Responsibility**: ノードの責務を分離し、保守性を高める。

## When to use
- **Design**: 新しい自動化フローを作りたいとき。
- **Deploy/Validate**: 作成したワークフローをn8nインスタンスに反映し、動作検証を行いたいとき。
- **Analyze**: JSONの品質をチェックし、改善案を提示したいとき。
- **Store**: 完成・検証済みのワークフローをGitHub（Practice等）とObsidianに保存したいとき。

## Capabilities & Process

### 4. Store (保存 & 同期)
GitHubとObsidianへ、新規作成・上書き更新を自動判断して同期する。
- **GitHub保存 (Smart Overwrite)**:
  - フォルダパス: `/Practice/[folder-name]/`
  - 既存チェック: 対象フォルダや同名JSONが既にあるか `ls` で確認。
  - 新規の場合: 新しいフォルダを作成。コミットメッセージは `feat: Add [workflow-name]`。
  - 更新の場合: 既存ファイルを上書き。**READMEは必ず日本語で生成・再生成する**。コミットメッセージは `update: Sync [workflow-name]`。
- **Git同期 (Safe Push)**:
  - `git add .`, `git commit`, `git push origin main` を実行。

## Quality Checklist
- [ ] 評価レポートにMermaid図が含まれているか
- [ ] GitHub保存時に既にフォルダがある場合は、新規作成せず「上書き更新」を選択しているか
- [ ] コミットメッセージが「Add（新規）」か「Update（更新）」か適切に選定されているか
