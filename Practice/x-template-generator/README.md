# X Template Generator Workflow

フォームから入力されたX（旧Twitter）の投稿やアイデアを解析し、再利用可能な「テンプレート」と「キャッチフレーズ」を抽出してGoogleスプレッドシートに保存するn8nワークフローです。

## ワークフローの目的
- 優れた投稿の構造を抽出し、テンプレート資産として蓄積する。
- 汎用性の高いキャッチフレーズをプレースホルダ化して抽出し、制作スピードを向上させる。

## システム構成
- **トリガー**: n8n Form Trigger
- **AI処理**: Google Gemini (1.5 Flash推奨)
- **保存先**: Google Sheets (テンプレート用とキャッチフレーズ用の2シート)
- **通知**: Slack (成功時およびエラー時)

## ワークフロー図 (Mermaid)

```mermaid
graph TD
    Trigger["On form submission (Form Trigger)"] --> Normalize["Normalize Input (Set)"]
    
    subgraph "AI Processing"
        Normalize --> TemplateGen["Basic LLM Chain (ChainLLM)"]
        Normalize --> PhraseGen["CatchphraseExtractor (InformationExtractor)"]
        
        Gemini1["Google Gemini Chat Model"] -.->|ai_languageModel| TemplateGen
        Parser["Structured Output Parser"] -.->|ai_outputParser| TemplateGen
        Gemini2["Google Gemini Chat Model1"] -.->|ai_languageModel| PhraseGen
        
        TemplateGen --> AddKey1["add combine_key (Set)"]
        PhraseGen --> AddKey2["add combine_key1 (Set)"]
    end
    
    AddKey1 -->|input 0| Merge["Merge: Template and Phrase (Merge)"]
    AddKey2 -->|input 1| Merge
    
    Merge --> Unify["Set:UnifyPayload (Set)"]
    Unify --> NormalizeJS["Code:NormalizePlaceholders (Code)"]
    
    subgraph "Storage"
        NormalizeJS --> Split["Split Out (Split Out)"]
        NormalizeJS --> SaveTemplate["Append row in sheet1 (Google Sheets)"]
        
        Split --> MapCatch["Set:MapCatchRow (Set)"]
        MapCatch --> SaveCatch["Append row in sheet (Google Sheets)"]
    end
    
    SaveTemplate --> SlackSuccess["Slack:ErrorNotify1 (Success Notification)"]
    
    subgraph "Error Handling"
        ErrorTrigger["ErrorTrigger:Global"] --> SlackError["Slack:ErrorNotify"]
    end
```

## 評価結果 (Quality Score)
- **総合点: 4.1 / 5.0 (PASS)**
- **構造 (4.5)**: 並列処理とマージの設計が非常に効率的。
- **再利用性 (4.5)**: JSノードによるプレースホルダの正規化が優秀。

## 改善のためのTodo
- [ ] AIノードのモデル名を `gemini-1.5-flash` に修正
- [ ] Slack通知メッセージに `{{ $execution.url }}` を追加
- [ ] Google Sheetsノードにリトライ設定 (`Retry On Fail`) を追加
