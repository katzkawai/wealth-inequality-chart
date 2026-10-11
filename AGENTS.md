# AGENTS.md — エージェント向けプロジェクト説明

## プロジェクト概要

NBC News のグラフ *Rising wealth*（アメリカの富の格差：上位1% vs 下位50%）を日本語化し、
FRB「分配金融勘定（Distributional Financial Accounts）」のデータ（1989Q3–2026Q2）で
再現・拡張した**静的 Web ページ**。GitHub Pages での公開を前提とする。

- 言語: プレーンな HTML / CSS / JavaScript（フレームワーク・ビルドツール・依存パッケージなし）
- ページ言語: 日本語（`index.html` は `<html lang="ja">`）
- リポジトリ: https://github.com/katzkawai/wealth-inequality-chart

## ファイル構成

- `index.html` — 唯一のページ。グラフ（SVG 画像）、ポイント解説、データテーブルを含む。
  スタイルは `<style>`、テーブル描画スクリプトは `<script>` としてすべてインライン。
- `assets/chart.svg` — 手書き座標の日本語版ラインチャート（上位1%: 金 `#c99a3f`、下位50%: 青 `#4a90c4`）。
  生成スクリプトはリポジトリに存在せず、SVG を直接編集して更新している（git 履歴参照）。
- `assets/dfa_top1_bottom50_ja.csv` — 四半期データ（日本語列名）。ヘッダー:
  `四半期,日付,上位1%_純資産_兆ドル,下位50%_純資産_兆ドル`
- `assets/dfa_top1_bottom50_en.csv` — 同上（英語列名）。ヘッダー:
  `quarter,date,top1_pct_net_worth_trillions,bottom50_pct_net_worth_trillions`
- `.nojekyll` — GitHub Pages で Jekyll 処理を無効化するための空ファイル。

## ビルド・テスト

ビルド工程・テスト・リンター・CI は**一切存在しない**。確認方法はローカルで HTTP サーバを立てて
ブラウザで開くことのみ（`file://` 直開きは `fetch()` が CORS で失敗しテーブルが空になる）:

```bash
python3 -m http.server 8000
# → http://localhost:8000/ を開く
```

## 動作の仕組み（重要）

- ページ下部のインラインスクリプトが `assets/dfa_top1_bottom50_ja.csv` を `fetch` し、
  **末尾16行**（最新16四半期）を `#tbl tbody` に描画する。列インデックス 0,2,3 を使用するため、
  **CSV の列順を変更すると壊れる**。
- テーブルの数値は `toFixed(2)` で表示される。
- `chart.svg` は CSV データを**自動では反映しない**静的画像。データ更新時は SVG の
  `<path d="...">` 座標と注記テキスト（四半期・基準日）も手動で整合させる必要がある。
- `index.html` 内のリード文・ポイント解説・キャプションにも四半期や数値（「約60兆ドル」等）が
  ハードコードされている。データ更新時はこれらも一緒に更新すること。

## データ出典・単位の規約

- 出典: FRB Distributional Financial Accounts
  https://www.federalreserve.gov/releases/z1/dataviz/dfa/distribute/table/
- 上位1% = 上位0.1% + 99–99.9パーセンタイルの合計。単位は兆ドル（現在価値）。
- 2つの CSV（ja/en）は同一データで列名のみ異なる。**片方だけ更新しない**こと。
  また `dfa_top1_bottom50_ja.csv` の最終行には末尾改行がない（既存状態）。

## デプロイ

GitHub Pages（Settings → Pages → Deploy from a branch → main / (root)）で公開。
`main` ブランチへ push すれば自動で反映される。`.nojekyll` は必須。

## 既知の注意点

- `README.md` のファイル構成節に記載の `assets/rising_wealth_ja.png` は存在しない
  （実際は `assets/chart.svg`）。README の記述が古い。
- ページ上の数値は FRB の過去値改訂により NBC 元グラフ（2025年3月時点）と
  わずかに異なる場合がある。
