# Advocacy Survey 解析パイプライン ステップ定義

## 現状（2025/02/07時点）

| 項目 | 状態 |
|------|------|
| フォルダ構成 (`data/`, `src/`, `output/`) | 完了 |
| `data/survey.xlsx` | 配置済み |
| `README.md` / `解析仕様書.md` | 作成済み |
| `src/` 内のスクリプト | **未着手** |
| `output/` の解析結果 | **未着手** |

---

## ステップ 1) フォルダ構成の作成 → 完了

```
advocacy-survey/
  data/survey.xlsx
  src/
  output/
  README.md
  解析仕様書.md
```

---

## ステップ 2) 解析の土台づくり（辞書対応とクリーニング） → 次にやる

### 前提：Xcode ライセンス同意 or Homebrew Python のセットアップが必要

### やること

Excel の3シートを確認：
- `ﾗﾍﾞﾙ対応表`（データ辞書：質問文、タイプ、カテゴリなど）
- `1;id～q21`（本データ：id〜q21、分岐質問、複数選択の列など）
- `ＧＴ表`（集計用/コード表の可能性）

### Claude Code への依頼文（コピペでOK）

```
あなたは統計解析の実装担当です。data/survey.xlsx を読み込み、以下を満たすPython解析パイプラインを src/ に作成してください。

要件:
1) シート「1;id～q21」を主データとして読み込む
2) シート「ﾗﾍﾞﾙ対応表」をデータ辞書として読み込み、
   - 各列の質問文、質問タイプ、カテゴリ数、カテゴリ（値ラベル）を抽出
   - SA(単一選択)やMA(複数選択)と思われる列を判定
3) 値が 1,2,3… のコードになっている列は、辞書を使って「ラベル付き列」を別名で作る（例: q1_label）
4) 欠損値（空欄、"NA"など）を統一し、クリーニング済みデータを output/clean.parquet と output/clean.csv に保存
5) 解析サマリーを output/summary.md に出力:
   - N、欠損率
   - 各設問の度数分布（SA）
   - 複数選択（qX-1, qX-2…形式）の選択率
6) 追加で、主要アウトカム候補を自動検出して（例: implementフラグ等があれば）、
   - 施設属性（役割など）とのクロス集計＋カイ二乗
   - 二値アウトカムならロジスティック回帰（statsmodels）も実装（多重共線性やカテゴリ参照レベルに配慮）
7) すべて再現可能にするため requirements.txt と 実行手順を README.md に書く

まずはデータの列一覧、推定タイプ、辞書の対応率を出すところまで実装し、実行してエラーがない状態にしてください。
```

### 成果物

- `src/01_build_dictionary.py`
- `src/02_cleaning.py`
- `output/clean.csv` / `output/clean.parquet`
- `output/data_dictionary.csv`

---

## ステップ 3) 記述統計（RQ1–RQ3）

- `src/03_descriptive.py` を作成
- 回答率、N
- RQ1：HA教育実施率（点推定 + 95%CI、Wilson）
- RQ2：形式（opportunistic/formal）、内容（micro/meso/macro）の割合 + 95%CI
- RQ3：障壁の割合 + 95%CI
- 出力：`output/summary_RQ1-3.md`, `output/table_RQ1_impl.csv` 等

---

## ステップ 4) 回帰分析

- `src/04_regression.py` を作成
- Y1（HA教育実装）を目的変数としたロジスティック回帰
- Y2（マクロ導入）/ Y3（フォーマル化）モデル
- 多重比較は FDR（Benjamini–Hochberg）
- 出力：`output/regression_Y1.csv`（OR, CI, p）

### Claude Code への段階的指示例

```
アウトカムを q?? に固定して、共変量は q1（役割）と施設規模…で
多重比較はFDR（Benjamini–Hochberg）で
表1（baseline table）を作って
結果表をCSVとWord貼り付け用（markdown）で
```

---

## ステップ 5) 効果推定・因果解析（探索的）

- `src/05_causal.py` を作成
- 傾向スコア法（IPW）、Doubly Robust（AIPW）、G-computation
- バランス評価（SMD < 0.1）
- 感度分析（E-value 等）
- 出力：`output/causal_effects.csv`（ATE/ATT, RD/RR/OR, CI）

---

## ステップ 6) 統合実行・最終確認

- `src/run_analysis.py`（全工程実行スクリプト）
- `requirements.txt` の確定
- `README.md` の実行手順を最終更新

---

## 注意点（失敗しやすいところ）

- **ハイフン付き列名**（`q4a-1` 等）：式処理では文字列として扱う
- **`q*_snt*_1` 列**：自由記述なので定量解析からは分離
- **`ans_flg` 列**：回答方法フラグ → 偏りチェックに使う
- **Python 環境**：Xcode ライセンス同意 or Homebrew Python が必要

## 必要パッケージ

```
pandas
openpyxl
numpy
scipy
statsmodels
matplotlib
```
