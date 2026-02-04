# 📊 Trump Social Media Intelligence Analytics — Power BI Dashboard

![Power BI](https://img.shields.io/badge/Tool-Power%20BI-yellow)
![Status](https://img.shields.io/badge/Status-Completed-success)
![License](https://img.shields.io/badge/License-MIT-blue)

> **End-to-end Social Media Analytics Dashboard built entirely using Power BI.**
> This project analyzes Donald Trump’s social media activity across **Twitter and Truth Social**, focusing on **engagement, content performance, virality, and posting behavior trends**.

---

## 📊 Key Performance Indicators (KPIs)

This dashboard is powered by a comprehensive set of **custom DAX measures** built entirely in **Power BI**. These KPIs enable deep analysis across engagement, content behavior, platform comparison, and posting trends.

---

### 🔢 Engagement & Volume Metrics

**Total Post**

```DAX
Total Post = COUNT(trump_truth_social_posts[id])
```

**Total Likes**

```DAX
Total Likes = SUM(trump_truth_social_posts[favorite_count])
```

**Total Repost**

```DAX
Total Repost = SUM(trump_truth_social_posts[repost_count])
```

**Total Engagement**

```DAX
Total Engagement = SUM(trump_truth_social_posts[engagement])
```

**Avg Engagement / Avg Engagement per Post / Avg Engagement by Platform**

```DAX
Avg Engagement = DIVIDE([Total Engagement], [Total Post], 0)
```

**Avg Likes per Post**

```DAX
Avg Likes per Post = AVERAGE('trump_truth_social_posts'[favorite_count])
```

**Avg Reposts per Post**

```DAX
Avg Reposts per Post = AVERAGE('trump_truth_social_posts'[repost_count])
```

---

### 📈 Content Performance Metrics

**Top Performing Post**

```DAX
Top Performing Post = MAX('trump_truth_social_posts'[engagement])
```

**Engagement with Media**

```DAX
Engagement with Media = 
CALCULATE([Total Engagement], 'trump_truth_social_posts'[media_count] > 0)
```

**Engagement without Media**

```DAX
Engagement without Media = 
CALCULATE([Total Engagement], 'trump_truth_social_posts'[media_count] = 0)
```

---

### 🧠 Content Behavior Analysis

**Original Posts**

```DAX
Original Posts = 
CALCULATE(COUNT('trump_truth_social_posts'[id]), 'trump_truth_social_posts'[post_type] = "Original")
```

**Repost Posts**

```DAX
Repost Posts = 
CALCULATE(COUNT('trump_truth_social_posts'[id]), 'trump_truth_social_posts'[post_type] = "Repost")
```

**% Original Posts**

```DAX
% Original Posts = 
DIVIDE(CALCULATE([Total Post], 'trump_truth_social_posts'[post_type] = "Original"), [Total Post])
```

**% Reposts**

```DAX
% Reposts = 
DIVIDE(CALCULATE([Total Post], 'trump_truth_social_posts'[post_type] = "Repost"), [Total Post])
```

**Deleted Posts**

```DAX
Deleted Posts = 
CALCULATE(COUNT('trump_truth_social_posts'[id]), 'trump_truth_social_posts'[deleted_flag] = TRUE())
```

**% Deleted Posts**

```DAX
% Deleted Posts = 
DIVIDE(CALCULATE([Total Post], 'trump_truth_social_posts'[deleted_flag] = TRUE()), [Total Post])
```

---

### 📅 Time-Based Insights

**Peak Posting Day**

```DAX
Peak Posting Day = 
MAXX(VALUES('trump_truth_social_posts'[post_date]), CALCULATE([Total Post]))
```

**Peak Engagement Day**

```DAX
Peak Engagement Day = 
MAXX(VALUES('trump_truth_social_posts'[post_date]), CALCULATE([Total Engagement]))
```

**Highest Engagement Month**

```DAX
Highest Engagement Month = 
VAR MonthTable =
    ADDCOLUMNS(VALUES('trump_truth_social_posts'[post_month]), "MonthEngagement", [Total Engagement])
VAR TopMonth = TOPN(1, MonthTable, [MonthEngagement], DESC)
RETURN CONCATENATEX(TopMonth, 'trump_truth_social_posts'[post_month], ", ")
```

**Highest Engagement Year**

```DAX
Highest Engagement Year = 
VAR YearTable =
    ADDCOLUMNS(VALUES('trump_truth_social_posts'[post_year]), "YearEngagement", [Total Engagement])
VAR TopYear = TOPN(1, YearTable, [YearEngagement], DESC)
RETURN CONCATENATEX(TopYear, 'trump_truth_social_posts'[post_year], ", ")
```

---

### 🌐 Platform Analysis

**Active Platforms**

```DAX
Active Platforms = DISTINCTCOUNT(trump_truth_social_posts[platform])
```

**Truth Social Posts**

```DAX
Truth Social Posts = 
CALCULATE(COUNT('trump_truth_social_posts'[id]), 'trump_truth_social_posts'[platform] = "Truth Social")
```

**Twitter Posts**

```DAX
Twitter Posts = 
CALCULATE(COUNT('trump_truth_social_posts'[id]), 'trump_truth_social_posts'[platform] = "Twitter")
```

**Platform Logo (Dynamic KPI)**

```DAX
Platform_Logo = 
SWITCH(
    SELECTEDVALUE('trump_truth_social_posts'[platform]),
    "Truth Social", "https://media.licdn.com/dms/image/v2/D4D12AQGEsvWXDyVi_Q/article-cover_image-shrink_720_1280/article-cover_image-shrink_720_1280/0/1688499594663",
    "Twitter", "https://img.freepik.com/premium-vector/circle-twitter-x-logo-social-media-app-button_197792-9716.jpg",
    BLANK()
)
```

---

These KPIs form the **analytical backbone of the dashboard**, enabling trend analysis, performance benchmarking, content strategy evaluation, and engagement optimization across platforms — all implemented **purely using Power BI and DAX**.
