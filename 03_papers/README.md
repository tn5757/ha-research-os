# 03_papers

論文をセクション単位で管理する場所。

## 構成

- `references/` ― 引用文献プール（全論文で共有する書誌情報）
- `paper-slug/` ― 個別の論文プロジェクト（`_sample-paper/` を参考に作成）

## 新しい論文の始め方

1. `_sample-paper/` をコピーして論文スラッグ名でリネーム
2. `_meta.md` にタイトル・投稿先を記入
3. 各セクションを順次執筆

## 論文のライフサイクル

論文はフォルダを移動しない。`_meta.md` の `status` だけが進む。

```
drafting → submitted → in_review → revision → accepted → published
```
