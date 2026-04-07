---
description: Deprecated. Use define-feature -> setup-project -> plan-feature -> implement-feature -> validate-implementation.
---

# add-feature (Deprecated)

このコマンドは新規テンプレートでは正式フローとして使用しません。  
後方互換のために残していますが、以下の順序を使ってください。

1. `define-feature`
2. `setup-project` （初期要件から永続ドキュメントを作る段階で必要）
3. `plan-feature <docs/ideas/...md>`
4. `implement-feature <.steering/...>`
5. `validate-implementation <.steering/...>`

## 理由

- 設計と実装と検証を明確に分離するため
- `.steering/[YYYYMMDD]-[task]/` の更新責務を分かりやすくするため
- AI エージェントの役割を一貫させるため

## 置き換え手順

- 仕様を作る・更新する: `define-feature`
- 初期要件から永続ドキュメントを作る: `setup-project`
- 仕様から実装計画を作る: `plan-feature`
- tasklist に従って実装する: `implement-feature`
- 実装を厳しめに検証する: `validate-implementation`

## 注意

- 新しいプロジェクトでは `add-feature` を主要ワークフローとして案内しない
- `AGENTS.md` と `README.md` の公式フローを優先する
