# Autonomous n8n Content Automation Agent Blueprint

## 1) End-to-end workflow architecture (node-by-node)

### A. Orchestration & Scheduling
1. **Cron: Daily Research Trigger** (06:00)
2. **Cron: Daily Content Trigger** (09:00)
3. **Cron: Daily Reporting Trigger** (20:00)
4. **Cron: Weekly Insights Trigger** (Sunday 21:00)
5. **Webhook: Manual Override / Re-run**
6. **Merge: Trigger Router** (normalizes trigger payload)

### B. Automated Market Research (R&D)
7. **HTTP Request: YouTube Trending Shorts API**
8. **HTTP Request: TikTok Trend Discovery API**
9. **HTTP Request: Instagram Reels Discovery API**
10. **IF: Category Filter Enabled?**
11. **Code: Category Mapping + Keyword Filter**
12. **Code: Normalize Metrics** (views, likes, comments, publish age, creator size)
13. **Code: Competition Score**
14. **Code: Opportunity Score** (high views + low competition + growth velocity)
15. **Code: Duplicate Detection Hash** (title + topic + script similarity)
16. **IF: Duplicate/Low-quality Data?**
17. **Airtable/Google Sheets/DB: Upsert Research Records**
18. **OpenAI/LLM Agent: Niche Insights Summary**
19. **Airtable/Sheets: Save Daily R&D Insight**

### C. 30-Day Growth Plan Generator
20. **Read DB: Last 30 Days Research + Performance Data**
21. **LLM Agent: Niche Selection** (profitability + trend sustainability)
22. **LLM Agent: Content Pillars Builder** (3–5 pillars)
23. **LLM Agent: 30-Day Topic Calendar**
24. **LLM Agent: Hook/Title/Keyword Matrix**
25. **Code: Daily Posting Schedule Compiler**
26. **Code: Growth Tactics Injector** (SEO, hashtag clusters, cross-post plan)
27. **DB Upsert: Growth Plan Table**
28. **Slack/Email: Plan Approval (Optional)**
29. **IF: Approved?** (continue or queue for revision)

### D. Daily Reel/Short Creation
30. **Read DB: Today’s Planned Topic**
31. **LLM Agent: Script Writer** (15–45s optimized short)
32. **LLM Agent: Hook + CTA Optimizer**
33. **LLM Agent: Title/Description/Tag Generator**
34. **Code: Content Policy Pre-check** (ban words, claims, risky terms)
35. **IF: Policy Flagged?**
36. **AI Video Node/API: Generate Video** (Runway/Pika/CapCut template)
37. **Subtitle Node/API: Auto Captions + Burn-in**
38. **Code: Thumbnail Frame Picker + Overlay Text**
39. **Cloud Storage: Save Final Assets**
40. **Manual Review Node (Optional)**
41. **IF: Approved for Publishing?**

### E. Uploading & Distribution
42. **HTTP Request: YouTube Upload (Shorts)**
43. **HTTP Request: Instagram Reels Publish**
44. **HTTP Request: TikTok Upload**
45. **Code: Retry Strategy Controller** (exponential backoff + max attempts)
46. **IF: Upload Failed?**
47. **Error Workflow Trigger: Upload Failure Handler**
48. **DB Upsert: Published URLs + Platform IDs + Status**
49. **Slack/Telegram: Publish Confirmation**

### F. Performance Monitoring & Optimization
50. **Cron: Pull 24h Metrics**
51. **HTTP Request: YouTube Analytics**
52. **HTTP Request: Instagram Insights**
53. **HTTP Request: TikTok Analytics**
54. **Code: Engagement Metrics Calculator** (ER, watch-time %, retention, velocity)
55. **Code: Prediction vs Actual Comparator**
56. **LLM Agent: Winning Format Detector**
57. **LLM Agent: Low-performer Diagnosis + Fix**
58. **IF: Performance Drop Threshold Reached?**
59. **LLM Agent: Auto-pivot Niche Proposal**
60. **DB Upsert: Metrics & Insights**
61. **Google Sheets/Notion/Airtable: Dashboard Update**
62. **Email/Slack: Daily Summary Report**
63. **Email/Slack: Weekly Deep-Dive Insights**

### G. Reliability & Edge-case Layer
64. **Global Error Trigger Workflow**
65. **Rate Limit Queue Node** (token bucket)
66. **Dead Letter Queue Node**
67. **Missing Data Imputer Code Node**
68. **Duplicate Publish Guard Node**
69. **Policy Violation Escalation Node**
70. **Fallback Content Generator Node**

---

## 2) AI agent logic (autonomous decision loop)

### Agent roles
- **Research Agent**: trend scraping, niche scoring, saturation detection.
- **Strategy Agent**: 30-day plan generation and content calendar optimization.
- **Creative Agent**: scripts, hooks, CTA, metadata.
- **Publishing Agent**: API dispatch + retries + status logging.
- **Analytics Agent**: KPI monitoring, winner extraction, pivot recommendation.

### Decision loop
1. **Observe**: ingest platform trend + own channel metrics.
2. **Orient**: compute opportunity score and risk score.
3. **Decide**:
   - continue niche,
   - adjust topic mix,
   - pivot niche if rolling 7-day KPI drop > threshold.
4. **Act**: generate/publish content and update plans.
5. **Learn**: compare forecast vs actual, feed model memory.

### Core scoring formulas
- `engagement_rate = (likes + comments + shares) / views`
- `growth_velocity = (views_24h - views_1h) / max(1, views_1h)`
- `competition_score = avg(top_creator_followers, post_volume_7d)`
- `opportunity_score = (normalized_views * 0.35) + (engagement_rate * 0.25) + (growth_velocity * 0.25) - (competition_score * 0.15)`

### Auto-pivot rule
- Trigger pivot when all are true for 2 weeks:
  - median ER < 60% of baseline,
  - CTR and retention both down >25%,
  - no breakout content in last 10 posts.

---

## 3) 30-day growth strategy blueprint

### Week 1: Positioning + Validation
- Publish 1 short/day across 2 content pillars.
- Test 3 hook styles:
  - shock/fact,
  - myth-busting,
  - fast tutorial.
- Objective: identify first winning format.

### Week 2: Expand winners
- Double down on top 20% topics by watch-time and ER.
- Introduce series format (Part 1/2/3).
- Start cross-posting within 15 minutes of YouTube publish.

### Week 3: Scaling + SEO hardening
- Build keyword clusters and hashtag packs by pillar.
- Improve intros: strongest 1.5 seconds, shorter setup.
- Add mid-video curiosity loop.

### Week 4: Optimization + pivot readiness
- Reduce low performers.
- Increase posting frequency to 1.5x if production permits.
- Apply auto-pivot if KPI floor breached.

### Content system defaults
- **Pillars**: Education, Controversy/Hot Takes, Case Studies, Tool/Template demos.
- **Cadence**: 1 short/day, 7 days/week.
- **Creative spec**: 9:16, 1080x1920, 15–45s, dynamic captions.
- **CTA rotation**: subscribe, comment keyword, save/share.

---

## 4) Execution plan (build sequence)

1. Set credentials in n8n (YouTube, Meta, TikTok, OpenAI, DB).
2. Import `workflows/n8n_content_agent.json`.
3. Configure environment variables:
   - `OPENAI_API_KEY`
   - `YOUTUBE_CHANNEL_ID`
   - `INSTAGRAM_BUSINESS_ID`
   - `TIKTOK_ADVERTISER_OR_CREATOR_ID`
   - `AIRTABLE_BASE_ID` (or sheet IDs)
4. Map category taxonomy and allowed keyword sets.
5. Enable optional manual approval branch.
6. Run dry test with “research only” path.
7. Run full pipeline with one test asset.
8. Enable production schedules.
9. Monitor first 7 days and calibrate thresholds.

---

## 5) Recommended data schema

### `research_trends`
- `date`
- `platform`
- `topic`
- `category`
- `views`
- `likes`
- `comments`
- `engagement_rate`
- `competition_score`
- `opportunity_score`
- `source_url`

### `content_plan`
- `publish_date`
- `pillar`
- `topic`
- `hook`
- `title`
- `keywords`
- `hashtags`
- `status`

### `published_content`
- `content_id`
- `platform`
- `post_url`
- `publish_ts`
- `upload_status`
- `retry_count`

### `performance_metrics`
- `content_id`
- `views_1h`
- `views_24h`
- `likes`
- `comments`
- `watch_time`
- `retention`
- `engagement_rate`
- `predicted_score`
- `actual_score`
- `winner_flag`
