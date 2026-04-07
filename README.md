# Project Template

This directory is a reusable base template for new AI-driven React Native projects.

### 日本語説明
このディレクトリは、AI 駆動で開発する React Native プロジェクト向けの再利用テンプレートです。

## Template assumptions

- React Native
- Expo managed workflow
- JavaScript
- Node 22
- Expo SDK 54

### 日本語説明
- React Native を使う前提です。
- Expo managed workflow を採用します。
- 実装言語は JavaScript です。
- Node 22 と Expo SDK 54 を前提にします。

## Included files

- `AGENTS.md`
- `PROJECT_CONTEXT.md`
- `docs/ideas/`
- `.steering/.gitkeep`
- `.agents/`
- `.gitignore`
- `.nvmrc`
- `.devcontainer/` (optional)

`.agents/` contains reusable commands, skills, settings, and review agents copied from the current project where they are generic enough for new React Native + Expo + JavaScript projects.

`docs/ideas/` is reserved for specs only. It contains the bootstrap project spec `initial-requirements.md` and any later feature specs created by `define-feature`.

The template intentionally does not include application source code, `node_modules`, or `package-lock.json`.

### 日本語説明
- `AGENTS.md` は AI エージェント向けの運用ルールです。
- `PROJECT_CONTEXT.md` は技術前提と環境制約をまとめた文書です。
- `docs/ideas/` は仕様専用ディレクトリです。
- `docs/ideas/initial-requirements.md` はプロジェクト全体の初期要件に使います。
- 追加機能の仕様は `define-feature` で `docs/ideas/YYYYMMDD-[feature-name].md` として作成・更新します。
- `.steering/.gitkeep` はタスク単位の計画ディレクトリを保持するためのプレースホルダーです。
- `.agents/` には再利用可能な commands、skills、settings、review agents が入っています。
- `.gitignore` と `.nvmrc` は共通開発環境の前提を揃えるために含めています。
- `.devcontainer/` は将来利用できる任意の開発環境設定です。

このテンプレートにはアプリケーションのソースコード、`node_modules`、`package-lock.json` は含まれていません。

## How to start a new project from this template

1. Create a new repository for the project.
2. Copy the contents of `project-template/` into the new repository root.
3. Read `AGENTS.md`, `PROJECT_CONTEXT.md`, and `.agents/README.md`.
4. Run `init-project` to create the Expo managed workflow baseline.
5. Keep the environment pinned to Node 22 and Expo SDK 54.
6. Run `define-feature` to create `docs/ideas/initial-requirements.md` if it does not exist yet.
7. Run `setup-project` to create the six durable docs from `docs/ideas/initial-requirements.md`.
8. Run `define-feature` again to create or update a feature spec in `docs/ideas/YYYYMMDD-[feature-name].md`.
9. Run `plan-feature` with that `docs/ideas/...md` file to create `.steering/[YYYYMMDD]-[task]/`.
10. Run `implement-feature` for the target `.steering/...` directory.
11. Run `validate-implementation` for the same `.steering/...` directory.

### 日本語説明
1. 新しいプロジェクト用のリポジトリを作成します。
2. `project-template/` の中身を新しいリポジトリのルートへコピーします。
3. `AGENTS.md`、`PROJECT_CONTEXT.md`、`.agents/README.md` を読みます。
4. `init-project` を実行して Expo managed workflow の土台を作成します。
5. 開発環境は Node 22 と Expo SDK 54 に固定します。
6. `define-feature` を実行して、未作成なら `docs/ideas/initial-requirements.md` を作成します。
7. `setup-project` を実行して `docs/ideas/initial-requirements.md` から 6 つの永続ドキュメントを作成します。
8. 追加機能に着手する前に、`define-feature` を実行して `docs/ideas/YYYYMMDD-[feature-name].md` を作成または更新します。
9. `plan-feature` に対象の `docs/ideas/...md` を渡して `.steering/[YYYYMMDD]-[task]/` を作成します。
10. `implement-feature` で対象 `.steering/...` に従って実装します。
11. `validate-implementation` で同じ `.steering/...` を検証します。

## Development commands

- `npx expo start`
- `npx expo start --tunnel`

### 日本語説明
- `npx expo start` は通常の開発サーバー起動に使います。
- `npx expo start --tunnel` はリモート端末から接続したい場合に使います。

## Workflow note

Run `init-project` before `setup-project`.

`init-project` prepares the Expo app baseline and shared environment files.

`define-feature` is the entry point for creating and updating specs in `docs/ideas/`.

`setup-project` prepares the six durable documents from `docs/ideas/initial-requirements.md`.

`plan-feature` only accepts a feature spec under `docs/ideas/`. It must not be run with `docs/ideas/initial-requirements.md`; use `setup-project` first.

`validate-implementation` is the dedicated validation step after `implement-feature`.

`.devcontainer/` is kept in the template as an optional future-ready setup. It does not mean Docker must be used immediately.

### 日本語説明
`init-project` は `setup-project` より先に実行します。  
`init-project` は Expo アプリの土台と共通環境ファイルを整えます。  
`define-feature` は `docs/ideas/` の仕様作成・更新の入口です。  
`setup-project` は `docs/ideas/initial-requirements.md` をもとに 6 つの永続ドキュメントを作ります。  
`plan-feature` は `docs/ideas/` 配下の追加仕様ファイルを入力に使い、`initial-requirements.md` は受け付けません。  
`validate-implementation` は `implement-feature` の後に使う実装検証専用コマンドです。  
`.devcontainer/` は将来用にテンプレートへ残しますが、現時点で Docker 利用は必須ではありません。

## Dependency policy

- Use `npx expo install` for Expo-related dependencies
- Do not upgrade Expo SDK automatically
- Do not change Node version automatically

### 日本語説明
- Expo 関連の依存追加や更新では `npx expo install` を使います。
- Expo SDK は明示依頼がない限り自動アップグレードしません。
- Node のバージョンも自動では変更しません。
