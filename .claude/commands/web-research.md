# Web Research

00_canon/ の問いに基づき、Web上の学術情報・関連資料を探索し、01_insight-sources/web-research/ に記録する。

## 実行手順

### 1. 研究の方向を確認
00_canon/ を読み込み、検索の方向性を把握する。
- core-question.md → 何を探すか
- positioning.md → どの空白を埋める情報が必要か

### 2. 検索の実行
引数 `$ARGUMENTS` に応じて検索対象を決定：
- `literature` → 学術論文・書籍・学位論文
- `data` → 統計資料・調査報告・公的機関のレポート
- `trends` → 研究分野の最新動向・学会情報・ニュース
- 引数なし → ユーザーに検索目的を確認

検索キーワードは canon の問いから導出し、日本語・英語の両方で検索する。

### 3. 結果の記録
以下の形式で `01_insight-sources/web-research/{YYYY-MM-DD}_{HHmm}_{検索テーマ}.md` に保存する：

```markdown
# Web Research: {検索テーマ}

- **実行日時**: {YYYY-MM-DD HH:mm}
- **検索モード**: literature / data / trends
- **対応するcanonの問い**: {core-questionとの関連}

## 検索キーワード
- {keyword_1}
- {keyword_2}

## 発見した情報

### 1. {タイトル}
- **種別**: 学術論文 / 調査報告 / ニュース記事
- **著者/発行元**: {著者名 or 機関名}
- **年**: {YYYY}
- **URL**: {url}
- **概要**: {1-2文の要約}
- **研究との関連**: {canonの問いとどう繋がるか}

### 2. {タイトル}
- **種別**: ...
- **URL**: {url}
...

## 参照URL一覧
- [{タイトル1}]({url1})
- [{タイトル2}]({url2})
- [{タイトル3}]({url3})
```

### 4. 後続アクションの提案
結果の記録後、以下を出力する：
- 特に注目すべき発見（canon の問いに直結するもの）
- `01_insight-sources/literature/` に要約メモを作成すべき文献の提案
- `03_papers/references/` に追加すべき書誌情報の提案
- `02_insights/` への示唆（新しいRQの芽、既存conceptの補強等）
