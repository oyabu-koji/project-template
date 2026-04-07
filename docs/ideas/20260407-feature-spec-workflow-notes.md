# Feature Spec Workflow Notes

このメモは、`docs/ideas/` を起点にした新しい feature workflow の合意内容を整理し、
後続のコマンド改造とドキュメント更新の入力に使うための作業メモです。

## 目的

- 開発時のコンテキスト消費を抑える
- `.steering/` の前段に、機能仕様を整理する明確なステップを置く
- 仕様、永続ドキュメント、実装計画の役割を分離する

## 合意済みのワークフロー

### 全体像

1. `define-feature`
   - `docs/ideas/` の仕様を作成または更新する
   - `docs/ideas/initial-requirements.md` がなければ初期要件として作成する
   - `initial-requirements.md` が既にあれば、追加仕様ファイルを作成するか、既存仕様ファイルを更新する
2. `setup-project`
   - `docs/ideas/initial-requirements.md` を入力に、`docs/` の6つの永続ドキュメントを作成する
3. `plan-feature`
   - `docs/ideas/...md` を入力に `.steering/[YYYYMMDD]-[task]/` を作成する
4. `implement-feature`
   - `.steering/.../tasklist.md` に従って実装する
5. `validate-implementation`
   - `implementation-validator` を呼び出して実装を検証する

### コマンド責務の整理

| コマンド                  | 役割                         | 主入力                               | 主出力                                                        |
| ------------------------- | ---------------------------- | ------------------------------------ | ------------------------------------------------------------- |
| `define-feature`          | 仕様を作る・更新する         | アイデア、既存仕様                   | `docs/ideas/initial-requirements.md` または `docs/ideas/*.md` |
| `setup-project`           | 永続ドキュメントを初期化する | `docs/ideas/initial-requirements.md` | `docs/` の6文書                                               |
| `plan-feature`            | 実装計画を作る               | `docs/ideas/*.md`                    | `.steering/[YYYYMMDD]-[task]/`                                |
| `implement-feature`       | 計画に従って実装する         | `.steering/...`                      | 実装コード、更新済み `tasklist.md`                            |
| `validate-implementation` | 実装検証を行う               | 実装コード、関連 docs                | 検証結果                                                      |

## source of truth の整理

### `docs/ideas/`

- 仕様だけを置く
- `initial-requirements.md` と `define-feature` が作成・更新した仕様ファイルだけを置く
- 未確定の仕様や、実装前に整理したい仕様メモを扱う

### `docs/`

- 安定した要件、設計、構造、用語の正本を置く
- `setup-project` で初期化し、安定した判断が出たら更新する

### `.steering/`

- 実装セッション単位の短期計画と進捗管理を置く
- 仕様の正本ではなく、今回の実装で何をするかを管理する

## 各コマンドの合意内容

### `define-feature`

- `docs/ideas/` の仕様を作る・更新するコマンドにする
- `docs/ideas/initial-requirements.md` がなければ、それを作る
- `docs/ideas/initial-requirements.md` が既にあれば、別の仕様ファイルを作る
- 既存仕様の更新も許可する
- `setup-project` 前でも後でも使える
- 日本語で対話する
- 必要な確認は選択肢付きでユーザーに確認する
- Claude Code 系で運用する場合は `AskUserQuestion` ツールを使う前提をコマンド仕様に含める
- Codex 系など `AskUserQuestion` がない環境では、同等の選択肢提示を会話上で行う前提にする

#### 想定ユースケース

- 新規プロジェクト開始時に初期要件を作る
- `setup-project` 後に、追加機能の仕様を考える
- 既存の `docs/ideas/...md` を更新して仕様を詰め直す

#### 出力テンプレート案

追加仕様ファイルは、少なくとも以下の項目を持つ構成を推奨する。

```markdown
# Feature Spec

## Metadata

- Date: YYYY-MM-DD
- Feature name:
- Status: draft | confirmed
- Related files:

## Background

- なぜこの仕様が必要か
- どの課題を解決するか

## Target Users / Use Cases

- 誰が使うか
- どの場面で使うか

## Scope

- 今回含めること

## Out of Scope

- 今回含めないこと

## User Flow

- ユーザー操作の流れ

## Functional Requirements

- 要件1
- 要件2

## Non-Functional / Technical Notes

- React Native / Expo 前提の制約
- 端末機能、パフォーマンス、オフライン、権限など

## Acceptance Criteria

- 条件1
- 条件2

## Open Questions

- 未決事項1
- 未決事項2

## Durable Docs Impact

- 更新候補:
- 更新要否:
- 理由:
```

`initial-requirements.md` を新規作成する場合は既存テンプレートを使い、
追加仕様ファイルを作る場合は上記テンプレートをベースにする。

### `plan-feature`

- 正入力は `docs/ideas/...md`
- 会話中の短い補足コメントは許可するが、正本は `docs/ideas/...md` とする
- `docs/ideas/initial-requirements.md` を入力された場合は停止し、`setup-project` に誘導する
- 入力ファイルが指定されていない場合は自動推測しない
- `docs/ideas/` の候補ファイルを列挙し、対象ファイルを指定するよう案内して停止する

#### 停止条件

- 引数なし
- 指定ファイルが `docs/ideas/` 配下にない
- 指定ファイルが `docs/ideas/initial-requirements.md`

#### 停止時メッセージの方針

- 引数なし:
  - `docs/ideas/` 配下の候補を表示し、対象ファイルを指定するよう案内する
- `initial-requirements.md` 指定時:
  - これはプロジェクト初期要件であり、`plan-feature` の入力ではないことを伝える
  - `setup-project` を先に実行するよう案内する

### `implement-feature`

- `.steering/...` に従って実装する
- `tasklist.md` を更新しながら進める
- 実装と進捗管理を同期させる

### `validate-implementation`

- 実装検証専用のコマンドとして新設する
- 中では `implementation-validator` を呼び出す
- `add-feature` の代替ではなく、検証ステップを明示化するための独立コマンドとする
- 実装済みコードを対象に実行する
- 入力は対象 `.steering/...` を明示で受ける形にする

#### 想定する入力契約

- 例: `validate-implementation .steering/20260407-feature-name/`
- 指定された `.steering/...` 配下の `requirements.md` / `design.md` / `tasklist.md` を読む
- 関連する `docs/` と実装コードを読み、`implementation-validator` に渡す
- 引数未指定時は停止し、対象 `.steering/...` を指定するよう案内する

## `add-feature` の扱い

- 現時点では必須ではない
- 一旦、主要ワークフローから外す方向で進める
- 代わりに `validate-implementation` を新設し、
  `define-feature -> setup-project -> plan-feature -> implement-feature -> validate-implementation`
  の分離された流れを公式フローにする

## 更新が必要な箇所

### ルートドキュメント

- `AGENTS.md`
- `README.md`

### コマンド定義

- `.agents/commands/init-project.md`
- `.agents/commands/setup-project.md`
- `.agents/commands/plan-feature.md`
- `.agents/commands/implement-feature.md`
- `.agents/commands/add-feature.md`
- `.agents/commands/validate-implementation.md` を新規作成するか検討
- `.agents/commands/define-feature.md` を新規作成する

### スキル・テンプレート

- `.agents/skills/steering/SKILL.md`
- `docs/ideas/initial-requirements.md`
- `docs/ideas/` 用の feature spec テンプレートを新規作成するか検討

## 残っている未決事項

### 1. `define-feature` の出力テンプレート

- 機能仕様ファイルに最低限どの項目を持たせるか
- 例:
  - 背景
  - 対象ユーザー
  - スコープ
  - 非スコープ
  - ユーザーフロー
  - 受け入れ条件
  - 未決事項

### 2. `docs/ideas/*.md` の命名規則

- 日付必須
- 形式は `YYYYMMDD-[feature-name].md`

### 3. `docs/ideas/` から `docs/` への昇格ルール

- `define-feature` 実行のたびに、`docs/` の更新要否を必ず検討する
- `docs/` が未作成なら更新検討だけ記録し、`setup-project` 後に反映する
- `docs/` が存在する場合、安定した仕様と判断できるものはその時点で更新候補にする

#### 「安定した仕様」とみなす提案条件

- ユーザーに確認済みで、方向性が固まっている
- `Open Questions` に残る未決事項が主要な振る舞いを変えない
- 今回限りのメモではなく、今後の実装判断で再参照される可能性が高い
- 複数機能にまたがる共通ルール、共通用語、共通構造に影響する
- 一時しのぎの回避策や実験案ではない

#### `docs/` 更新の判断基準案

- `docs/product-requirements.md`
  - ユーザー価値、対象ユーザー、スコープ、成功条件が変わるとき
- `docs/functional-design.md`
  - 画面、ユースケース、フロー、データ入出力が変わるとき
- `docs/architecture.md`
  - レイヤー構造、主要ライブラリ、状態管理、永続化、外部連携方針が変わるとき
- `docs/repository-structure.md`
  - ディレクトリ構成やファイル配置ルールが変わるとき
- `docs/development-guidelines.md`
  - 実装規約、テスト方針、レビュー方針、運用ルールが変わるとき
- `docs/glossary.md`
  - 継続的に使う新用語や意味変更が出たとき

### 4. `validate-implementation` の入力契約

- 対象 `.steering/...` を明示で受ける
- 実装したコードに対して行うスラッシュコマンドとして扱う
- 実装そのものは行わず、対象タスクの仕様・設計・コード・テスト状況を検証する

## 改造時の実装メモ

1. まず `define-feature` と `validate-implementation` の新規コマンド定義を作る
2. `define-feature` に日本語対話と選択肢付き確認の要件を入れる
3. 次に `plan-feature` を `docs/ideas/...md` 必須の入力契約へ変更する
4. `setup-project` は `initial-requirements.md` 入力を前提に明示する
5. `AGENTS.md` と `README.md` の公式フローを新ワークフローに合わせて更新する
6. 必要なら `add-feature` は deprecated のまま残すか、完全削除するかを最後に決める
