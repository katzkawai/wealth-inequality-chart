# 富の増大 — アメリカの富の格差（上位1% vs 下位50%）

NBC News のグラフ *Rising wealth* を日本語化し、FRB「分配金融勘定（Distributional Financial Accounts）」の
最新データ（2026年第2四半期）まで拡張して再現した静的ページです。

## 公開方法（GitHub Pages）

```bash
git init
git add .
git commit -m "Add wealth inequality chart page"
git branch -M main
git remote add origin https://github.com/<ユーザー名>/<リポジトリ名>.git
git push -u origin main
```

その後、GitHub リポジトリの **Settings → Pages → Source: Deploy from a branch → main / (root)** を選択すると、
`https://<ユーザー名>.github.io/<リポジトリ名>/` で公開されます。

## ファイル構成

- `index.html` — グラフ・ポイント解説・データテーブルを含むページ
- `assets/rising_wealth_ja.png` — 日本語版グラフ
- `assets/dfa_top1_bottom50_ja.csv` / `dfa_top1_bottom50_en.csv` — 四半期データ（1989Q3–2026Q2）

## データ出典

Board of Governors of the Federal Reserve System — Distributional Financial Accounts
https://www.federalreserve.gov/releases/z1/dataviz/dfa/distribute/table/

上位1% = 上位0.1% + 99–99.9パーセンタイルの合計。単位は兆ドル。
注：FRBは過去値を改訂することがあるため、NBCの元グラフ（2025年3月時点）とはわずかに異なる場合があります。
