# Advocacy Education Survey Analysis

（日本の小児科専門研修プログラムにおけるアドボカシー教育調査）

## 概要（Overview）

本リポジトリは、日本全国の小児科専門研修プログラムを対象に実施した
Health Advocacy（HA）教育に関する全国アンケート調査の解析を、
再現可能な形で実施するための解析コード一式をまとめたものです。

本解析は、以下の研究課題（RQ）に基づいて設計されています。

- 日本の小児科専門研修プログラムにおいて、HA教育はどの程度実施されているか
- HA教育の内容および教育形式（opportunistic / formal、micro/meso/macro）はどのような特徴を持つか
- HA教育の実施を妨げている障壁は何か

本プロジェクトは、**AMEE（Theme: Designing and Planning Learning）** での発表を起点とし、
将来的な学術論文化を見据えた解析構成となっています。

## ディレクトリ構成

```
advocacy-survey/
├── data/
│   └── survey.xlsx          # 生データ（固定ファイル名推奨）
├── src/
│   ├── 01_build_dictionary.py   # データ辞書の構築
│   ├── 02_cleaning.py           # データクリーニング
│   ├── 03_descriptive.py        # 記述統計（RQ1–RQ3）
│   ├── 04_regression.py         # 回帰分析
│   ├── 05_causal.py             # 効果推定・因果推論
│   └── run_analysis.py          # 全解析の実行スクリプト
├── output/
│   ├── clean.csv
│   ├── clean.parquet
│   ├── summary_RQ1-3.md
│   ├── tables/
│   ├── figures/
│   └── logs/
├── requirements.txt
└── README.md
```

## 使用データ（Data）

- ファイル名：`data/survey.xlsx`
- 研究デザイン：全国横断アンケート調査
- 解析単位：1専門研修プログラム＝1回答
- 使用シート：
  - `1;id～q21`：アンケート回答ローデータ
  - `ﾗﾍﾞﾙ対応表`：設問文・選択肢・値ラベルを含むデータ辞書

> ⚠️ 生データは一切上書き・変更しません。

## 解析フロー（Analysis Workflow）

解析は以下の順で自動的に実行されます。

1. **データ辞書の構築**
   - 設問文、選択肢、単一選択／複数選択の判定
   - 数値コード → ラベル変換

2. **データクリーニング**
   - 欠損値の統一（NaN）
   - 複数選択設問のダミー変数化

3. **記述統計解析**
   - HA教育実施率
   - 教育形式（opportunistic / formal）
   - 教育内容（micro / meso / macro）
   - 障壁要因の分布

4. **回帰分析**
   - HA教育実施の関連要因分析（ロジスティック回帰）

5. **効果推定・因果解析（探索的）**
   - 傾向スコア法（IPW）
   - 回帰標準化（G-computation）
   - 感度分析（未測定交絡）

## 実行方法（How to Run）

### 1. 環境構築

```bash
pip install -r requirements.txt
```

### 2. 解析の実行

```bash
python src/run_analysis.py
```

すべての解析結果は `output/` 以下に保存されます。

## 出力物（Outputs）

- **`output/summary_RQ1-3.md`**
  - AMEE抄録・本文に対応した解析サマリー
- **`output/tables/`**
  - 記述統計・回帰分析・効果推定結果
- **`output/figures/`**
  - 発表・論文用図表
- **`output/logs/`**
  - 実行ログ（再現性担保）

## 因果解釈に関する注意

本研究は横断研究であるため、
因果推論は **仮説生成的（exploratory）** に位置づけられます。
未測定交絡や時間順序の不確実性を考慮し、結果の解釈は慎重に行います。

## 再現性と拡張性

- 解析はすべてスクリプト化されており、再実行可能です
- 学会発表後の
  - 追加変数投入
  - モデル拡張
  - 論文化向け解析

  に容易に対応できます

## 連絡先・メモ

- 本解析は研究目的に限定して使用してください
- 解析仕様書は別紙 `analysis_spec.md` を参照（必要に応じ作成）
