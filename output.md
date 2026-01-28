# AI-DLC セットアップガイド for Claude Code

このドキュメントは、任意のリポジトリで AI-DLC (AI-Driven Development Life Cycle) を Claude Code で実践するためのセットアップガイドです。

**対応バージョン**: [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) main ブランチ (2025-01 時点)

## 目次

1. [AI-DLC とは](#ai-dlc-とは)
2. [前提条件](#前提条件)
3. [セットアップ手順](#セットアップ手順)
4. [ファイルテンプレート](#ファイルテンプレート)
5. [使用方法](#使用方法)
6. [ワークフロー図](#ワークフロー図)
7. [セットアップ検証](#セットアップ検証)

---

## AI-DLC とは

AI-DLC (AI-Driven Development Life Cycle) は、AWS が提唱する AI 中心の開発手法です。従来のアジャイル/スクラムに AI を「後付け」するのではなく、AI を前提としてゼロベースで設計された方法論です。

**核心思想**: AI が計画・提案し、人間が検証・承認する。

**参照リンク**:
- [aidlc-workflows リポジトリ](https://github.com/awslabs/aidlc-workflows)
- [AWS 公式ブログ](https://aws.amazon.com/jp/blogs/news/ai-driven-development-life-cycle/)
- [ホワイトペーパー](https://prod.d13rzhkk8cj2z0.amplifyapp.com/)

### 3 フェーズ構造

| Phase | 責務 | 問い |
|-------|------|------|
| **Inception** | What / Why | 何を作るか、なぜ作るか |
| **Construction** | How | どう作るか |
| **Operations** | Run | どう運用するか（将来拡張） |

---

## 前提条件

- Claude Code がインストールされていること
- Git リポジトリが初期化されていること

---

## セットアップ手順

### 方法 A: セットアップスクリプトを使用（推奨）

```bash
# 1. aidlc-workflows リポジトリをクローン（まだの場合）
git clone https://github.com/awslabs/aidlc-workflows.git

# 2. 対象プロジェクトのルートディレクトリで実行
cd /path/to/your-project

# 3. セットアップスクリプトを実行
bash ../aidlc-workflows/setup-aidlc.sh
```

**setup-aidlc.sh** の内容（aidlc-workflows リポジトリに配置するか、手動で作成）:

```bash
#!/bin/bash
set -e

echo "Setting up AI-DLC for Claude Code..."

# ルールファイルのコピー
mkdir -p aidlc-rules
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details aidlc-rules/

# Claude Code ディレクトリの作成
mkdir -p .claude/skills/{inception,construction,ai-dlc-status}
mkdir -p .claude/rules

# AI-DLC 成果物ディレクトリの作成
mkdir -p aidlc-docs/{inception/{requirements,user-stories,application-design,reverse-engineering,plans},construction/{build-and-test,plans},operations}

# 初期ファイルの作成
cat > aidlc-docs/aidlc-state.md << 'EOF'
# AI-DLC 状態

## 現在のステータス
- **フェーズ**: 未開始
- **ステージ**: なし
- **最終更新**: -

## 完了したステージ
（まだなし）

## スキップしたステージ
（まだなし）

## メモ
EOF

cat > aidlc-docs/audit.md << 'EOF'
# AI-DLC 監査ログ

このファイルは AI-DLC ワークフロー中のすべての決定とユーザーとのやり取りを記録します。

---
EOF

echo "AI-DLC setup complete!"
echo ""
echo "Next steps:"
echo "1. Create CLAUDE.md in your project root"
echo "2. Create skill files in .claude/skills/"
echo "3. Create rule files in .claude/rules/"
echo "4. Run 'claude /ai-dlc-status' to verify"
```

### 方法 B: 手動セットアップ

#### Step 1: aidlc-workflows リポジトリをクローン

```bash
# 作業ディレクトリの親フォルダで実行
git clone https://github.com/awslabs/aidlc-workflows.git
```

#### Step 2: ルールファイルをコピー

```bash
# 対象プロジェクトのルートディレクトリで実行
mkdir -p aidlc-rules
cp -R ../aidlc-workflows/aidlc-rules/aws-aidlc-rule-details aidlc-rules/
```

#### Step 3: Claude Code 設定ファイルを作成

以下のファイルを作成します：

1. `CLAUDE.md` - プロジェクトルート
2. `.claude/skills/inception/SKILL.md`
3. `.claude/skills/construction/SKILL.md`
4. `.claude/skills/ai-dlc-status/SKILL.md`
5. `.claude/rules/aidlc-docs.md`

#### Step 4: aidlc-docs ディレクトリを作成

```bash
mkdir -p aidlc-docs/{inception/{requirements,user-stories,application-design,reverse-engineering,plans},construction/{build-and-test,plans},operations}
```

#### Step 5: 初期ファイルを作成

`aidlc-docs/aidlc-state.md`:

```markdown
# AI-DLC 状態

## 現在のステータス
- **フェーズ**: 未開始
- **ステージ**: なし
- **最終更新**: -

## 完了したステージ
（まだなし）

## スキップしたステージ
（まだなし）

## メモ
```

`aidlc-docs/audit.md`:

```markdown
# AI-DLC 監査ログ

このファイルは AI-DLC ワークフロー中のすべての決定とユーザーとのやり取りを記録します。

---
```

---

## ファイルテンプレート

### CLAUDE.md

```markdown
# プロジェクト名

## AI-DLC Framework

このプロジェクトは AI-DLC (AI-Driven Development Life Cycle) に従って開発を進めます。

### 基本原則

1. **Human in the Loop**: すべての重要な決定は人間が承認する
2. **段階的承認**: 各フェーズの完了時にユーザーの承認を得る
3. **透明性**: 実行計画を事前に提示し、承認を得てから実行する
4. **監査証跡**: すべての決定を `aidlc-docs/audit.md` に記録する

### フェーズ

1. **Inception Phase** (`/inception`)
   - 目的: WHAT & WHY の決定
   - 成果物: `aidlc-docs/inception/`

2. **Construction Phase** (`/construction`)
   - 目的: HOW の実装
   - 成果物: `aidlc-docs/construction/`

### AI-DLC ルール参照

詳細なルールは以下を参照してください：

@./aidlc-rules/aws-aidlc-rule-details/common/process-overview.md
@./aidlc-rules/aws-aidlc-rule-details/inception/workflow-planning.md
@./aidlc-rules/aws-aidlc-rule-details/construction/code-generation.md

### 成果物ディレクトリ構造

\`\`\`
aidlc-docs/
├── inception/
│   ├── plans/
│   ├── reverse-engineering/    # ブラウンフィールドのみ
│   ├── requirements/
│   ├── user-stories/
│   └── application-design/
├── construction/
│   ├── plans/
│   ├── {unit-name}/
│   │   ├── functional-design/
│   │   ├── nfr-requirements/
│   │   ├── nfr-design/
│   │   ├── infrastructure-design/
│   │   └── code/
│   └── build-and-test/
├── operations/                 # 将来拡張
├── aidlc-state.md
└── audit.md
\`\`\`
```

---

### .claude/skills/inception/SKILL.md

```markdown
---
description: AI-DLC Inception Phase を実行する - WHAT & WHY を決定する
---

# AI-DLC Inception Phase

## 概要

このスキルは AI-DLC の Inception Phase を実行します。
ユーザーの Intent（達成目的）を明確にし、実行計画を策定します。

## 実行手順

### 1. Workspace Detection（常に実行）

まず、プロジェクトの状態を確認します：

- **グリーンフィールド**: 既存コードがない新規プロジェクト
- **ブラウンフィールド**: 既存コードがあるプロジェクト

結果を `aidlc-docs/aidlc-state.md` に記録してください。

### 2. Reverse Engineering（ブラウンフィールドのみ）

既存コードがある場合、以下を分析して成果物を生成します：

**生成する成果物** (`aidlc-docs/inception/reverse-engineering/`):
- `business-overview.md` - ビジネストランザクションの概要
- `architecture.md` - アーキテクチャ文書
- `code-structure.md` - コード構造の説明
- `component-inventory.md` - コンポーネント一覧
- `technology-stack.md` - 技術スタック
- `dependencies.md` - 依存関係
- `interaction-diagrams.md` - コンポーネント間のインタラクション図

**ユーザー承認を得てから次に進む**

### 3. Requirements Analysis（常に実行、深さは適応的）

ユーザーの要件を分析します：

1. Intent の明確化
2. 要件の収集
3. 不明点の質問

**深さレベル**:
- **Minimal**: 単純で明確なリクエスト
- **Standard**: 通常の複雑さ
- **Comprehensive**: 複雑・高リスク

質問ファイルを `aidlc-docs/inception/requirements/requirement-verification-questions.md` に保存してください。

### 4. User Stories（条件付き）

**実行する場合**:
- 新しいユーザー向け機能
- 複数のペルソナが関与
- 複雑なビジネス要件

**スキップする場合**:
- 純粋なリファクタリング
- インフラのみの変更
- 単純なバグ修正

成果物は `aidlc-docs/inception/user-stories/` に保存してください。

### 5. Workflow Planning（常に実行）

どのステージを実行するか計画を作成します：

- 実行するステージの一覧
- スキップするステージとその理由
- 各ステージの深さレベル

成果物は `aidlc-docs/inception/plans/workflow-plan.md` に保存してください。

**ユーザー承認を得てから次に進む**

### 6. Application Design（条件付き）

**実行する場合**:
- 新しいコンポーネントが必要
- サービスレイヤー設計が必要
- コンポーネント間の依存関係を明確にする必要がある

成果物は `aidlc-docs/inception/application-design/` に保存してください。

### 7. Units Generation（条件付き）

**実行する場合**:
- 複数の Unit（作業単位）への分割が必要
- 並行開発が予定されている

成果物:
- `aidlc-docs/inception/application-design/unit-of-work.md`
- `aidlc-docs/inception/application-design/unit-of-work-dependency.md`
- `aidlc-docs/inception/application-design/unit-of-work-story-map.md`

## 完了条件

Inception Phase の完了時に、ユーザーに以下を提示して承認を得てください：

1. 要件サマリー
2. 実行計画（どのステージを実行するか）
3. 次のフェーズ（Construction）への移行確認

## 参照ルール

詳細な手順は以下のルールファイルを参照してください：

@./aidlc-rules/aws-aidlc-rule-details/inception/workspace-detection.md
@./aidlc-rules/aws-aidlc-rule-details/inception/reverse-engineering.md
@./aidlc-rules/aws-aidlc-rule-details/inception/requirements-analysis.md
@./aidlc-rules/aws-aidlc-rule-details/inception/user-stories.md
@./aidlc-rules/aws-aidlc-rule-details/inception/workflow-planning.md
@./aidlc-rules/aws-aidlc-rule-details/inception/application-design.md
@./aidlc-rules/aws-aidlc-rule-details/inception/units-generation.md
```

---

### .claude/skills/construction/SKILL.md

```markdown
---
description: AI-DLC Construction Phase を実行する - HOW を実装する
---

# AI-DLC Construction Phase

## 概要

このスキルは AI-DLC の Construction Phase を実行します。
Inception Phase で策定した計画に基づいて、設計と実装を行います。

## 前提条件

- Inception Phase が完了していること
- `aidlc-docs/inception/` に要件と計画が存在すること

## 実行手順

### 1. コンテキストの読み込み

まず、Inception Phase の成果物を読み込みます：

- `aidlc-docs/inception/requirements/`
- `aidlc-docs/inception/application-design/`（存在する場合）
- `aidlc-docs/inception/plans/workflow-plan.md`

### 2. Unit ごとのループ

各 Unit（作業単位）に対して以下を実行します。
単一 Unit の場合も同じ手順で実行します。

#### 2.1 Functional Design（条件付き）

**実行する場合**:
- 新しいデータモデル/スキーマが必要
- 複雑なビジネスロジックがある
- ビジネスルールの詳細設計が必要

**成果物** (`aidlc-docs/construction/{unit-name}/functional-design/`):
- `business-logic-model.md`
- `business-rules.md`
- `domain-entities.md`

#### 2.2 NFR Requirements（条件付き）

**実行する場合**:
- パフォーマンス要件がある
- セキュリティ考慮が必要
- スケーラビリティ要件がある
- 技術スタックの選定が必要

**成果物** (`aidlc-docs/construction/{unit-name}/nfr-requirements/`):
- `nfr-assessment.md`
- `technology-selection.md`

#### 2.3 NFR Design（条件付き）

**実行する場合**:
- NFR Requirements が実行された
- NFR パターンの組み込みが必要

**成果物** (`aidlc-docs/construction/{unit-name}/nfr-design/`):
- `nfr-design.md`
- `logical-components.md`

#### 2.4 Infrastructure Design（条件付き）

**実行する場合**:
- インフラサービスのマッピングが必要
- デプロイアーキテクチャの設計が必要
- クラウドリソースの指定が必要

**成果物** (`aidlc-docs/construction/{unit-name}/infrastructure-design/`):
- `infrastructure-mapping.md`
- `deployment-architecture.md`

#### 2.5 Code Generation（常に実行）

コード生成は 2 部構成で行います：

**Part 1 - Planning**:
- 生成するファイルの一覧
- 各ファイルの責務
- チェックボックス `[ ]` 付きの計画

計画を `aidlc-docs/construction/plans/{unit-name}-code-generation-plan.md` に保存

**Part 2 - Generation**:
- 計画に基づいてコードを生成
- テストコードも含める
- 完了したら `[x]` でチェック

**ユーザー承認を得てから次の Unit に進む**

### 3. Build and Test（常に実行）

すべての Unit 完了後：

**成果物** (`aidlc-docs/construction/build-and-test/`):
- `build-instructions.md` - ビルド手順
- `unit-test-instructions.md` - ユニットテスト手順
- `integration-test-instructions.md` - 統合テスト手順
- `build-and-test-summary.md` - サマリー

## 成果物の配置

- **ドキュメント**: `aidlc-docs/construction/` に保存
- **コード**: プロジェクトルートに配置（`aidlc-docs/` 内ではない）

## 完了条件

各ステージ完了時に、ユーザーに以下の選択肢を提示してください：

1. **Request Changes**: 修正を依頼する
2. **Continue to Next Stage**: 次のステージに進む

## 参照ルール

詳細な手順は以下のルールファイルを参照してください：

@./aidlc-rules/aws-aidlc-rule-details/construction/functional-design.md
@./aidlc-rules/aws-aidlc-rule-details/construction/nfr-requirements.md
@./aidlc-rules/aws-aidlc-rule-details/construction/nfr-design.md
@./aidlc-rules/aws-aidlc-rule-details/construction/infrastructure-design.md
@./aidlc-rules/aws-aidlc-rule-details/construction/code-generation.md
@./aidlc-rules/aws-aidlc-rule-details/construction/build-and-test.md
```

---

### .claude/skills/ai-dlc-status/SKILL.md

```markdown
---
description: AI-DLC の現在の進捗状況を確認する
---

# AI-DLC ステータス

## 概要

このスキルは AI-DLC の現在の進捗状況を表示します。

## 実行内容

1. `aidlc-docs/aidlc-state.md` を読み込む
2. 現在のフェーズとステージを表示
3. 完了したステージと残りのステージを一覧表示
4. 次のアクションを提案

## 表示形式

\`\`\`
AI-DLC ステータス
================

現在のフェーズ: [INCEPTION / CONSTRUCTION / OPERATIONS]
現在のステージ: [ステージ名]

完了済み:
- [x] ワークスペース検出
- [x] 要件分析
- [ ] ユーザーストーリー (スキップ - 理由: ...)
- [x] ワークフロー計画

残り:
- [ ] 機能設計
- [ ] コード生成
- [ ] ビルド＆テスト

次のアクション: [推奨される次のアクション]
\`\`\`

## ステータス更新

進捗があった場合は `aidlc-docs/aidlc-state.md` を更新してください。
```

---

### .claude/rules/aidlc-docs.md

```yaml
---
paths:
  - aidlc-docs/**
---

# AI-DLC ドキュメント作成ルール

## 質問形式

ユーザーへの質問は `[回答]:` タグを使用してください。

### 形式

\`\`\`markdown
### 質問 1: 認証方式について

どの認証方式を使用しますか？

A. セッションベース認証
B. JWT 認証
C. OAuth 2.0 + OIDC
D. その他（具体的に記述してください）

[回答]:
\`\`\`

### 重要なルール

1. **「その他」オプションは必須**: 必ず最後のオプションとして「その他」を含める
2. **オプションはラベル付き**: A, B, C, D などでラベル付け
3. **相互排他的**: オプション同士が重複しないようにする
4. **回答の検証**: 曖昧な回答（「どちらでも」「状況による」など）には追加質問を行う

## チェックボックス

計画のトラッキングにはチェックボックスを使用してください：

\`\`\`markdown
## 実行計画

- [ ] ステップ 1: 要件分析
- [ ] ステップ 2: 設計
- [x] ステップ 3: 実装（完了）
\`\`\`

**重要**: 作業完了時は必ずチェックボックスを `[x]` に更新すること。

## 監査ログ

すべての決定は `audit.md` に記録してください。
**重要**: audit.md は追記のみ。内容を上書きしない。

### 形式

\`\`\`markdown
## [ステージ名]
**タイムスタンプ**: YYYY-MM-DDTHH:MM:SSZ
**ユーザー入力**: "[ユーザーの入力をそのまま記録 - 要約しない]"
**AI の応答**: "[AI の応答]"
**コンテキスト**: [ステージ、アクション、決定事項]

---
\`\`\`

## ファイル命名規則

- 小文字とハイフンを使用: `requirements-analysis.md`
- 日本語ファイル名は避ける
- ユニット名はディレクトリ名として使用: `construction/{unit-name}/`
```

---

## 使用方法

### Inception Phase の開始

```bash
claude /inception
```

または、Claude Code のチャットで：

```
Using AI-DLC, I want to build [あなたの Intent]
```

### Construction Phase の開始

```bash
claude /construction
```

### 進捗確認

```bash
claude /ai-dlc-status
```

### 典型的なワークフロー

1. `/inception` で要件を明確化
2. 生成された質問ファイルに回答（`[回答]:` の後に回答を記入）
3. 実行計画を確認・承認
4. `/construction` で実装
5. 各ステージで成果物を確認・承認
6. Build and Test で完了

---

## ワークフロー図

```
                         User Request
                              |
                              v
        ╔═══════════════════════════════════════╗
        ║     INCEPTION PHASE                   ║
        ║     Planning & Application Design     ║
        ╠═══════════════════════════════════════╣
        ║ • Workspace Detection (ALWAYS)        ║
        ║ • Reverse Engineering (CONDITIONAL)   ║
        ║ • Requirements Analysis (ALWAYS)      ║
        ║ • User Stories (CONDITIONAL)          ║
        ║ • Workflow Planning (ALWAYS)          ║
        ║ • Application Design (CONDITIONAL)    ║
        ║ • Units Generation (CONDITIONAL)      ║
        ╚═══════════════════════════════════════╝
                              |
                              v
        ╔═══════════════════════════════════════╗
        ║     CONSTRUCTION PHASE                ║
        ║     Design, Implementation & Test     ║
        ╠═══════════════════════════════════════╣
        ║ • Per-Unit Loop (for each unit):      ║
        ║   - Functional Design (CONDITIONAL)   ║
        ║   - NFR Requirements (CONDITIONAL)    ║
        ║   - NFR Design (CONDITIONAL)          ║
        ║   - Infrastructure Design (COND.)     ║
        ║   - Code Generation (ALWAYS)          ║
        ║ • Build and Test (ALWAYS)             ║
        ╚═══════════════════════════════════════╝
                              |
                              v
                          Complete
```

### Claude Code 機能との対応

| AI-DLC 概念 | Claude Code 機能 | 説明 |
|------------|-----------------|------|
| Phase 実行 | Skills (`/inception`, `/construction`) | ユーザーが明示的に起動 |
| 既存コード分析 | Explore Subagent | 自動で起動される |
| 実装計画 | Plan Mode | 安全に分析・計画 |
| コードレビュー | code-reviewer Subagent | 生成コードの検証 |
| ルール参照 | CLAUDE.md + import | DRY 原則でルール管理 |

---

## セットアップ検証

セットアップ完了後、以下のチェックリストで動作確認してください：

### 基本確認

- [ ] `.claude/skills/` ディレクトリが存在する
- [ ] `aidlc-rules/aws-aidlc-rule-details/` ディレクトリが存在する
- [ ] `aidlc-docs/` ディレクトリ構造が正しい
- [ ] `CLAUDE.md` がプロジェクトルートに存在する

### 動作確認

- [ ] `claude /ai-dlc-status` が正常に実行される
- [ ] `claude /inception` でワークスペース検出が動作する
- [ ] 質問ファイルが `aidlc-docs/inception/requirements/` に生成される

### 確認コマンド

```bash
# ディレクトリ構造の確認
tree -L 3 .claude/ aidlc-rules/ aidlc-docs/

# Skills の確認
ls -la .claude/skills/*/SKILL.md

# CLAUDE.md の確認
head -20 CLAUDE.md
```

---

## トラブルシューティング

### Skills が認識されない

`.claude/skills/` ディレクトリが正しく作成されているか確認してください：

```bash
ls -la .claude/skills/
```

ファイル名が `SKILL.md`（大文字）であることを確認してください。

### import が機能しない

`aidlc-rules/` ディレクトリがプロジェクトルートに存在するか確認してください：

```bash
ls -la aidlc-rules/aws-aidlc-rule-details/
```

CLAUDE.md の import パスが `@./aidlc-rules/...` の形式になっているか確認してください。

### aidlc-docs が生成されない

手動でディレクトリを作成してください：

```bash
mkdir -p aidlc-docs/{inception/{requirements,user-stories,application-design,reverse-engineering,plans},construction/{build-and-test,plans},operations}
```

### 監査ログが上書きされる

Claude に「audit.md は追記のみで、内容を上書きしないでください」と伝えてください。
`.claude/rules/aidlc-docs.md` にもこのルールが記載されています。

---

## カスタマイズ例

### プロジェクト固有のルール追加

CLAUDE.md にプロジェクト固有のルールを追加できます：

```markdown
## プロジェクト固有ルール

### コーディング規約
- TypeScript を使用
- ESLint + Prettier でフォーマット
- テストは Jest を使用

### アーキテクチャ方針
- Clean Architecture を採用
- 依存性注入を使用
```

### 特定ステージのスキップ

Inception Phase の Workflow Planning で、特定のステージをスキップするよう指示できます：

```
NFR Requirements と NFR Design はスキップしてください。
このプロジェクトでは非機能要件の詳細設計は不要です。
```

---

## 参考リンク

- [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) - 公式リポジトリ
- [AWS 公式ブログ: AI-DLC](https://aws.amazon.com/jp/blogs/news/ai-driven-development-life-cycle/)
- [ホワイトペーパー](https://prod.d13rzhkk8cj2z0.amplifyapp.com/)
- [Claude Code Skills ドキュメント](https://docs.anthropic.com/en/docs/claude-code/skills)
