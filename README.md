# manga-ai-pipeline
**AI漫画制作ワークフロー — アイデアからYAML・画像生成まで**

キャラ名だけでも、シナリオだけでも、ふんわりしたイメージだけでも受け取って、  
画像生成AIに投げられるYAMLまで育てるワークフローです。

---

## このリポジトリでできること

```
あなたのアイデア（どんな状態でも）
    ↓
01 設定の引き出し（intake）
    ↓
02 バイブル構築（キャラ・世界観・シリーズ構成）
    ↓
03 シナリオ・ネーム生成（ページ設計 → コマ台本）
    ↓
04 YAML出力（コマ仕様書）
    ↓
05 画像生成AIへ投げる（ChatGPT / Gemini）
```

---

## どのAIを使えばいい？

```
漫画を作りたい
    │
    ├─ 画像も生成したい
    │       │
    │       ├─ ChatGPT（GPT-5.5 / GPT Image 2）← メイン推奨
    │       │       → 03_scenario + 04_yaml + 05_image_generation を使う
    │       │
    │       └─ Gemini（Imagen 3）
    │               → 03_scenario + 04_yaml を使う
    │               → Geminiへの渡し方は各ファイルの「Gemini版」を参照
    │
    └─ シナリオ・バイブル管理だけしたい
            │
            └─ Claude
                    → 01_intake ～ 04_yaml を使う
                    → Claudeのプロジェクト機能にバイブルを登録して使う
                    → 画像生成はChatGPT / Geminiと組み合わせる
```

---

## ファイル構成

```
manga-ai-pipeline/
├── README.md                        # このファイル
│
├── 01_intake/
│   └── intake_prompt.md             # 設定を引き出すプロンプト（AI共通）
│
├── 02_bible_templates/
│   ├── CHARACTER_BIBLE.md           # キャラクター設定テンプレート
│   ├── WORLD_BIBLE.md               # 世界観設定テンプレート
│   └── SERIES_GUIDE.md             # シリーズ構成・話数管理テンプレート
│
├── 03_scenario/
│   └── scenario_prompt.md           # ネーム生成プロンプト（ChatGPTメイン・他AI注記付き）
│
├── 04_yaml/
│   └── episode_template.yaml        # コマ仕様書テンプレート
│
├── 05_image_generation/
│   └── gpts_system_prompt.md        # ChatGPT GPTs用 System Prompt（汎用版）
│
├── 06_knowledge/
│   └── MANGA_EFFECTS_KNOWLEDGE.md   # 漫画演出効果リファレンス
└── examples/
    └── aiyla/                       # 記入例：Aiyla & Mini Konshu
```

---

## クイックスタート

### パターンA：ChatGPTだけで完結する

1. `02_bible_templates/` の3ファイルを自分の作品で埋める
2. `03_scenario/scenario_prompt.md` をChatGPTに貼る
3. シナリオ・YAMLが出たら `05_image_generation/gpts_system_prompt.md` でGPTsを作る
4. YAMLを貼ると画像が出る

### パターンB：ClaudeでシナリオだけGPTで画像生成する

1. `02_bible_templates/` をClaudeのプロジェクトに登録する
2. `03_scenario/scenario_prompt.md` をClaudeに貼ってシナリオ・YAMLを生成する
3. 出力されたYAMLを `05_image_generation/gpts_system_prompt.md` のGPTsに貼る

### パターンC：Geminiで試す

1. `01_intake/intake_prompt.md` のURLをGeminiに渡す
2. `03_scenario/scenario_prompt.md` の「Gemini版」注記を参照する
3. YAMLはChatGPTのGPTsに渡して画像生成する

---

## 入力レベル別の入り口

| あなたの状態 | 最初に開くファイル |
|---|---|
| ふんわりしたイメージだけある | `01_intake/intake_prompt.md` |
| キャラ設定はある・ストーリー未定 | `02_bible_templates/CHARACTER_BIBLE.md` から |
| シナリオはある・キャラ詳細未定 | `02_bible_templates/WORLD_BIBLE.md` から |
| バイブルは揃っている | `03_scenario/scenario_prompt.md` から |
| YAMLはある・画像生成だけしたい | `05_image_generation/gpts_system_prompt.md` から |

---

## AI別の注意点

### ChatGPT（GPT-5.5 / GPT Image 2）
- **推奨：GPTsを作って使う**（System Promptでキャラ設定を固定できる）
- GPT Image 2は推論型のため、指示していない補完をしやすい
- YAMLを渡したら**生成前に必ずプロンプトをテキスト確認してからOKを出す**
- `05_image_generation/gpts_system_prompt.md` の手順を必ず守る

### Gemini
- GitHubのURLを直接渡してファイルを読み込める
- キャラの一貫性はChatGPTより弱め
- 複数話にまたがる設定管理はClaudeの方が安定する

### Claude
- 長文のバイブルを読み込む精度が高い
- プロジェクト機能にバイブルを登録すると会話ごとに設定が保持される
- 画像生成機能はないため、YAMLをChatGPT / Geminiに渡す運用が基本

---

## ライセンス

MIT License — 商用利用・改変・再配布可。  
使った場合はスターかフォークをもらえると嬉しいです。

---

## 作者

[@goroyattemiyo](https://github.com/goroyattemiyo)  
制作中の作品：*Aiyla & Mini Konshu*（`examples/aiyla/` に記入例として掲載）
