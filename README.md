# Zepto Price Tracker

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://brightdata.jp)
[![Zepto Price Tracker](https://img.shields.io/badge/Zepto%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://brightdata.jp/products/insights/price-tracker/zepto)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://brightdata.jp/products/insights/price-tracker/zepto)

Zepto のリアルタイム価格追跡 - インドの即時食料品配達プラットフォーム。開始方法は 2 通りあります: **フルマネージド**のインテリジェンスプラットフォーム、または Bright Data の AI Scraper Builder で構築する**カスタム scraper**です。

---

## Option 1: Bright Insights - AI 搭載の価格追跡（推奨）

**[Bright Insights](https://brightdata.jp/products/insights/price-tracker/zepto)** は、Bright Data のフルマネージド小売インテリジェンスプラットフォームです。scraper の構築も、インフラの保守も不要で、構造化された分析対応の価格データをダッシュボード、data feed、または BI ツールにそのまま配信できます。

**チームが Bright Insights を選ぶ理由:**
- 🚀 **セットアップ不要** - すぐに使えるダッシュボードと data feed で数分で本番稼働
- 🤖 **AI による推奨** - 対話型 AI アシスタントが数百万のデータポイントを即座に実用的なインサイトに変換
- ⚡ **リアルタイム監視** - 1 時間ごとから日次までの更新頻度と即時アラート（email、Slack、webhook）
- 🌍 **無制限のスケール** - あらゆる Web サイト、あらゆる地域、あらゆる更新頻度に対応
- 🔗 **プラグアンドプレイ統合** - AWS、GCP、Databricks、Snowflake など
- 🛡️ **フルマネージド** - スキーマ変更、サイト更新、データ品質を Bright Data が自動で処理

**主なユースケース:**
- ✅ Zepto のカテゴリ横断で**食料品価格インフレを追跡**
- ✅ **プロモーションを監視**し、週次セールを自動追跡
- ✅ バスケット分析のための**価格履歴データベースを構築**
- ✅ MAP ポリシー準拠を監視し、価格違反を検出
- ✅ 競合のプロモーションと販促動向を追跡
- ✅ クリーンで正規化されたデータを動的価格設定アルゴリズムや AI モデルに直接投入

> **月額 $250 から - [お見積もりはこちら →](https://brightdata.jp/products/insights/price-tracker/zepto)**

---

## Option 2: 独自の Zepto Scraper を構築する

事前構築済みの Zepto scraper API がない？問題ありません。Bright Data の **AI Scraper Builder** なら、数クリックでカスタム Zepto scraper を生成できます — コーディングは不要です。

### 数分で Zepto scraper を構築

**[Zepto AI Scraper Builder を開く →](https://brightdata.jp/products/web-scraper/zepto)**

ドメインを選択し、必要なデータ要件を記述するだけで、AI scraper builder が自動的に API を作成します。

1. **必要なデータを平易な英語で記述**
2. **AI が即座に scraper API を生成**
3. **API リクエストを実行してすぐに結果を取得**
4. 必要に応じて**組み込み IDE でコードを編集**

構築が完了すると、scraper には **Web Scraper ID**（`gd_xxxxxxxxxxxx`）が付与されます — 以下の Setup 手順で使うためにコピーしてください。

### 前提条件

- Python 3.9 以上
- [Bright Data account](https://brightdata.jp)（無料トライアルあり）
- Bright Data の **API token**（[取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)）
- Zepto 用の **Web Scraper ID**（上記の構築手順で取得）

### Setup

1. **この repository を clone**

   ```bash
   git clone https://github.com/bright-jp/zepto-price-tracker.git
   cd zepto-price-tracker
   ```

2. **依存関係をインストール**

   ```bash
   pip install -r requirements.txt
   ```

3. **認証情報を設定**

   `.env.example` を `.env` にコピーし、値を入力します:

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **あなたの Web Scraper ID**
   > [AI Scraper Builder dashboard](https://brightdata.jp/products/web-scraper/zepto) から Web Scraper ID をコピーし、
   > `BRIGHTDATA_DATASET_ID` に貼り付けてください（形式: `gd_xxxxxxxxxxxx`）。

---

## 使用方法

Zepto scraper の構築が完了し、Web Scraper ID を `.env` に設定すると、Python interface は同じ方法で動作します。

### 1. URL で特定の商品を追跡

Zepto の商品 URL のリストを渡して、構造化された価格データを取得します:

```python
from price_tracker import track_prices

urls = [
    "https://www.zeptonow.com/products/sample-product",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

または直接実行:

```bash
python price_tracker.py
```

### 2. キーワードで商品を検索

キーワード検索に一致する商品を見つけます:

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. カテゴリ URL で商品を閲覧

Zepto のカテゴリページからすべての商品を収集します:

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://zeptonow.com/category/example",
    limit=100,
)
```

---

## 出力フィールド

各結果レコードには次のフィールドが含まれます:

| Field | Description |
|-------|-------------|
| `url` | 商品ページ URL |
| `name` | 商品名 |
| `brand` | ブランド |
| `price` | 現在価格 |
| `currency` | 通貨コード |
| `unit_price` | 単位/重量あたりの価格 |
| `in_stock` | 在庫状況 |
| `category` | 商品カテゴリ |
| `sku` | SKU / バーコード |
| `images` | 商品画像 URL |
| `description` | 商品説明 |
| `timestamp` | 収集タイムスタンプ |

### 出力例

```json
[
  {
    "url": "https://www.zeptonow.com/products/sample-product",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://zeptonow.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## 高度なオプション

`trigger_collection()` 関数は、データ収集を制御するためのオプションパラメータを受け付けます:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 返されるレコードの最大数 |
| `include_errors` | boolean | `true` | 結果に error report を含める |
| `notify` | string (URL) | - | snapshot の準備完了時に呼び出す webhook URL |
| `format` | string | `json` | 出力形式: `json`、`csv`、または `ndjson` |

オプション付きの例:

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://www.zeptonow.com/products/sample-product"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## リソース

- 🌟 [Zepto Price Tracker - Bright Insights (Managed)](https://brightdata.jp/products/insights/price-tracker/zepto)
- 🏗️ [Zepto Scraper を構築](https://brightdata.jp/products/web-scraper/zepto)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.brightdata.jp/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://brightdata.jp/cp/scrapers)
- 🔑 [API token の取得方法](https://docs.brightdata.jp/general/account/account-settings#api-token)
- 🌐 [Bright Data ホームページ](https://brightdata.jp)

---

*業界をリードする Web データプラットフォーム [Bright Data](https://brightdata.jp) で構築。*