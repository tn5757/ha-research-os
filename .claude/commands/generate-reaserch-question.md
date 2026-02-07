# Generate Research Question

01_insight-sources/ と 02_insights/concepts/ を読み込み、00_canon/ の問いに基づいてリサーチクエスチョンを生成し、02_insights/research-questions/ に記録する。

## 使用方法

引数: `$ARGUMENTS`

**形式**: `<ソースパス>`

- **ソースパス**（必須）: 以下のいずれかのパス
  - `01_insight-sources/` 配下 → 素材から直接RQを導出
  - `02_insights/concepts/` 配下 → 既存のインサイトからRQを導出
  - パスなし → 既存の全concepts + 全sourcesを対象にRQを提案

### 使用例

```
/generate-reaserch-question 02_insights/concepts/2026-02-06_learning-autonomy.md
```
→ 特定のconceptからRQを生成

```
/generate-reaserch-question 01_insight-sources/literature/
```
→ 文献メモ全体からRQを導出

```
/generate-reaserch-question
```
→ 全体を俯瞰してRQを提案

## 実行手順

### 1. canonの読み込み
00_canon/ の全ファイルを読み込み、研究プログラムの方向性を把握する。

### 2. 既存RQの確認
02_insights/research-questions/ を読み込み、すでにあるRQと重複しないことを確認する。

### 3. ソースの読み込み
指定されたパスのファイルを読み込む。

### 4. リサーチクエスチョンの生成

以下の基準でRQを生成する：

**良いRQの条件：**
1. **canonとの整合** ― core-questionの体系の一部として位置づけられる
2. **先行研究の空白に対応** ― positioningで示した空白を埋めるものである
3. **実証可能** ― データ収集と分析で答えを出せる形になっている
4. **焦点が明確** ― 一つの論文で取り組める範囲に絞られている

**RQの型：**
| 型 | 例 |
|---|---|
| 記述型 | 「〇〇の実態はどのようなものか」 |
| 関係型 | 「〇〇と△△にはどのような関係があるか」 |
| 因果型 | 「〇〇は△△にどのような影響を与えるか」 |
| プロセス型 | 「〇〇はどのようなプロセスで生じるか」 |

### 5. 02_insights/research-questions/ への記録

以下のフォーマットで `02_insights/research-questions/{YYYY-MM-DD}_{RQスラッグ}.md` に保存する：

```markdown
# RQ: {リサーチクエスチョン}

- **生成日**: {YYYY-MM-DD}
- **ソース**: {導出元のファイルパス}
- **RQの型**: 記述型 / 関係型 / 因果型 / プロセス型

## canonとの接続

{core-questionの体系のどこに位置づけられるか}

## 背景

{このRQが必要な理由。先行研究の空白との対応}

## 想定される研究デザイン

- **方法論**: {質的 / 量的 / 混合}
- **データ収集**: {インタビュー / アンケート / 観察 / etc.}
- **対象**: {誰を / 何を}

## 関連するinsights

- {関連するconceptファイルへのパス}

## ステータス

- [ ] 先行研究の追加レビュー
- [ ] 研究デザインの具体化
- [ ] 論文プロジェクトの作成（03_papers/）
```

### 6. 完了報告

以下を出力する：
- 生成したRQの数と一覧
- 各RQのcanonとの接続の概要
- 次のアクション（追加レビューすべき文献、03_papers/ に論文プロジェクトを作るべきか）
