# China BlackCat Complaints Scraper API (新浪黑猫投诉 — 100% No Cookie / No Login)

> 🚀 **Run Directly on Apify Cloud (Zero Setup, No Cookies Required):**
> 👉 **[Launch BlackCat Consumer Complaints Scraper on Apify Store](https://apify.com/axon-studio/blackcat-complaints-scraper)**

Extract real-time Chinese consumer complaints, refund disputes, product defect reports, and brand crisis signals from **Sina BlackCat (新浪黑猫投诉 / tousu.sina.com.cn)** — China's largest public consumer-rights and dispute-resolution platform with **38M+ verified complaints**.

Built for **Wall Street alt-data equity research, multinational brand PR & legal teams, supply-chain due diligence, and AI/LLM risk-monitoring pipelines** (n8n, Make, Dify, Claude MCP). **100% no login, no cookies, and no VPN required.** Users on the **Apify Free plan ($5 monthly credit) can extract up to 1,111 BlackCat complaint records per month for free**.

---

## What Can This BlackCat Scraper Do? (Features)

| BlackCat Scraper Capability | Built-In Advantage |
| :--- | :--- |
| **Brand & Keyword Search (`searchQuery`)** | Search consumer complaints about any domestic or international brand via our built-in managed session pool and signed API (`tousu.sina.com.cn/api/index/s`). |
| **Nationwide Live Complaint Feed** | Leave `searchQuery` empty to stream the latest consumer disputes across all industries in China (`tousu.sina.com.cn/api/index/feed`). |
| **Auto EN ↔ ZH Brand Localization** | Type an English brand name (e.g., `Tesla`, `Apple`, `Luckin`, `Starbucks`, `Nike`) and the scraper automatically translates and queries its official Chinese name (`特斯拉`, `苹果`, `瑞幸咖啡`, `星巴克`, `耐克`). |
| **Standardized English Status (`statusEn`)** | Automatically maps Chinese dispute stages into standardized English statuses (`pending_assignment`, `in_progress`, `replied`, `completed`, `closed`). |
| **Built-In Complaint Sentiment Scoring** | Zero-RAM Chinese/English sentiment engine with negation reversal tags every complaint with `sentiment` (`negative` / `neutral` / `positive`) and `sentimentScore` (`-1.0` to `+1.0`). |
| **256 MB RAM Real-Time Streaming** | Streams complaint records to your Apify Dataset in real-time batches of 10 rows on just **256 MB RAM**, supporting scheduled monitoring (`deltaMode`), REST API, and one-click **JSON, CSV, Excel, or Parquet** exports. |

---

## How to Scrape BlackCat (黑猫投诉) in 3 Easy Steps

1. **Enter a Brand or Keyword (`searchQuery`)** — Type any company, brand, or product in **English or Chinese** (e.g., `Tesla`, `Luckin`, `瑞幸咖啡`, `携程`), or leave it empty to capture the nationwide live complaint feed.
2. **Set `maxResults`** — Choose how many BlackCat complaints to extract (from `1` to `5,000`).
3. **Click "Start"** — Download your structured consumer complaint dataset in **JSON, CSV, or Excel**, or connect directly to n8n, Make, Google Sheets, or Claude MCP.

---

## BlackCat Scraper Use Cases

| Buyer Profile | BlackCat Consumer Intelligence Use Case |
| :--- | :--- |
| **Hedge Funds & Equity Research Desks** | Track complaint velocity, refund demand spikes (`退款`), and resolution rates (`statusEn`) as an early warning alt-data signal on Chinese consumer & EV tickers (BYD, Tesla, NIO, Luckin, PDD, Alibaba, Meituan) |
| **Global Brand PR & Crisis Teams** | Detect product safety issues, counterfeit reports, and customer service failures in China before they escalate to Weibo Hot Search |
| **Legal, Compliance & ESG Analysts** | Audit corporate consumer-rights responsiveness and dispute patterns across Chinese subsidiaries and e-commerce storefronts |
| **AI / LLM Risk & Financial Agents** | Ground autonomous research agents in live Chinese consumer dispute records via REST API or Claude MCP |

---

## Input

All parameters are available in the **Input** tab. Below are the primary configuration fields and JSON input examples:

| Input Field | Type | Description |
| :--- | :--- | :--- |
| **`searchQuery`** | `string` | Brand, company, or product keyword in English or Chinese (e.g., `"Tesla"`, `"瑞幸咖啡"`). Leave empty `""` for the nationwide live complaint feed. |
| **`autoLocalize`** | `boolean` | Automatically translates English brand names into Simplified Chinese (default: `true`). |
| **`searchAliases`** | `array` | Additional sub-brands or product keywords to merge and deduplicate into the same run. |
| **`maxResults`** | `integer` | Maximum number of BlackCat complaints to return (`1` to `5,000`, default: `50`). |
| **`sentimentAnalysis`** | `boolean` | Attaches `sentiment` polarity and `sentimentScore` (`-1.0` to `+1.0`) to every complaint (default: `true`). |
| **`deltaMode`** | `boolean` | Remembers scraped complaint `sn` IDs across scheduled runs so you only pay for newly filed complaints (default: `false`). |

### 1. Brand-Specific BlackCat Complaint Search (JSON)
```json
{
  "mode": "complaints",
  "searchQuery": "Tesla",
  "autoLocalize": true,
  "searchAliases": ["Model Y", "Model 3"],
  "maxResults": 100,
  "sentimentAnalysis": true,
  "deltaMode": false
}
```

### 2. Nationwide Live Consumer Complaint Feed (JSON)
```json
{
  "mode": "complaints",
  "searchQuery": "",
  "maxResults": 200,
  "sentimentAnalysis": true
}
```

---

## Output

Each scraped complaint is stored in your Apify **Dataset** with the following structured fields (matching the `"Tesla"` search input above):

```json
{
  "mode": "complaints",
  "sn": "17402449965",
  "company": "特斯拉客户支持",
  "title": "特斯拉门店充电桩计费异常且客服未按承诺退还差价",
  "appeal": "退款,改善服务,作出解释",
  "text": "本人于2026年10月22日在超级充电站充电，账单多扣取超时占用费，联系特斯拉客服核实后至今未退回...",
  "summary": "本人于2026年10月22日在超级充电站充电，账单多扣取超时占用费，联系特斯拉客服核实后至今未退回...",
  "sentiment": "negative",
  "sentimentScore": -0.78,
  "statusEn": "in_progress",
  "scrapedAt": "2026-10-24 16:32:10 UTC"
}
```

### Output Field Definitions
* **`sn`**: Unique BlackCat complaint serial number (used for deduplication in `deltaMode`).
* **`company`**: Official name of the complained-against company or brand (`投诉对象`).
* **`title`**: Headline of the consumer complaint.
* **`appeal`**: Specific consumer demands (`投诉要求`, e.g., `退款` refund, `赔偿` compensation, `道歉` apology).
* **`text` / `summary`**: Clean HTML-stripped complaint summary text.
* **`statusEn`**: Standardized dispute progress (`pending_assignment`, `in_progress`, `replied`, `completed`, `closed`).
* **`sentiment` / `sentimentScore`**: Polarity label and normalized severity score (`-1.0` to `+1.0`).

---

## Automated Daily Brand Crisis Monitoring (Schedules & Delta Mode)

Set up a **24/7 automated brand complaint monitor** in 2 minutes:
1. In **Apify Console → Schedules → Create new schedule**, select **`axon-studio/blackcat-scraper`** and set your cron frequency (e.g., hourly `0 * * * *` or daily `0 8 * * *`).
2. Set **`deltaMode: true`** in your input JSON. The Actor stores up to 10,000 scraped `sn` IDs in your Key-Value Store (`DELTA_SEEN_IDS`) and skips previously delivered complaints on future runs—so you only pay for brand-new disputes.

### Python, JavaScript & One-Line REST API Integration

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_API_TOKEN")
run = client.actor("axon-studio/blackcat-scraper").call(run_input={
    "mode": "complaints",
    "searchQuery": "Tesla",
    "autoLocalize": True,
    "maxResults": 100,
    "sentimentAnalysis": True,
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item["sn"], item["company"], item["statusEn"], item["title"])
```

```bash
curl -X POST "https://api.apify.com/v2/acts/axon-studio~blackcat-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"mode":"complaints","searchQuery":"Tesla","maxResults":50,"sentimentAnalysis":true}'
```

---

## How Much Does It Cost to Scrape BlackCat? (Pricing)

**$4.50 per 1,000 complaints (`$0.0045` per item) + `$0.02` per Actor start (Pay-Per-Event)**

* **Free Plan Credits**: Every Apify account includes **$5.00 in free monthly credits**, covering up to **1,111 BlackCat complaint records every month for free**.
* **Volume Discounts**: Automatic lower unit prices for Apify **Bronze / Starter**, **Silver / Scale**, and **Gold / Business** plan subscribers.
* **No Server Compute Fees**: Memory and CPU usage are fully included in the Pay-Per-Event rate.
* **Typical Run Costs**: 50 brand complaints = **`$0.245`** · 200 brand complaints = **`$0.92`** · 1,000 complaints = **`$4.52`**.

---

## FAQ & Legal Compliance

### Do I need a Sina Weibo or BlackCat login cookie?
**No.** This BlackCat Scraper uses a built-in managed session pool and SHA-256 request signing automatically.

### Is it legal to scrape BlackCat consumer complaints?
This Actor extracts publicly visible consumer dispute summaries from `tousu.sina.com.cn` and does not access private user accounts. Under GDPR and PIPL, ensure you have a lawful basis for market research or compliance analytics. *Not affiliated with Sina Corporation.*

---

## More Chinese Social Intelligence Scrapers by Axon Studio

* **[Weibo Scraper — All-in-One Search, Hot Trends, Comments & BlackCat](https://apify.com/axon-studio/weibo-scraper)** — 9-mode comprehensive Sina Weibo & BlackCat intelligence suite.
* **[Weibo User Posts & Profile Scraper (No Cookie)](https://apify.com/axon-studio/weibo-user-posts-scraper)** — Extract creator timelines (`user_posts`) and KOL follower metrics without login.
* **[Weibo Comments & Province Geolocation Scraper](https://apify.com/axon-studio/weibo-comments-scraper)** — Harvest reader comment threads and English IP province locations (`regionEn`).
