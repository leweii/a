---
description: Weekly analysis of agent conversation logs from BigQuery
tags: [bigquery, analytics, agent, conversations, weekly-report]
created: 2025-12-31
---

# Agent Conversation Weekly Analysis

## Prompt

Help me fetch recent one week data from the agent_conversation_logs table in BigQuery, and analyze the data.

I like to know:
1. Who is the person that uses agent most frequently?
2. Are most of the conversation have a happy ending?

## Analysis Steps

### 1. User Activity Analysis
Find the most frequent users by analyzing:
- Total chat sessions
- Total invocations
- Total messages (user requests + agent responses)
- Activity breakdown

### 2. Satisfaction Analysis
Measure conversation success by:
- Thumbs up/down feedback
- Interruption rates
- Session completion rates
- Positive feedback percentage

### 3. Session-Level Analysis
Track individual sessions to understand:
- Which sessions received feedback
- Whether conversations were interrupted
- Session duration patterns

## SQL Queries Used

### Query 1: User Activity
```sql
SELECT
  user_name,
  user_email,
  COUNT(DISTINCT chat_session_id) as total_sessions,
  COUNT(DISTINCT invocation_id) as total_invocations,
  COUNT(*) as total_messages,
  SUM(CASE WHEN type = 'USER_REQUEST' THEN 1 ELSE 0 END) as user_requests,
  SUM(CASE WHEN type = 'AGENT_RESPONSE' THEN 1 ELSE 0 END) as agent_responses
FROM `wonder-recipe-prod.mongo_batch_recipe_agent.agent_conversation_logs`
WHERE created_time >= DATETIME_SUB(CURRENT_DATETIME(), INTERVAL 7 DAY)
GROUP BY user_name, user_email
ORDER BY total_messages DESC
```

### Query 2: Overall Satisfaction Metrics
```sql
SELECT
  COUNT(DISTINCT chat_session_id) as total_sessions,
  SUM(CASE WHEN thumbs_up = TRUE THEN 1 ELSE 0 END) as thumbs_up_count,
  SUM(CASE WHEN thumbs_down IS NOT NULL AND thumbs_down != '' THEN 1 ELSE 0 END) as thumbs_down_count,
  SUM(CASE WHEN is_interrupted = TRUE THEN 1 ELSE 0 END) as interrupted_count,
  ROUND(SUM(CASE WHEN thumbs_up = TRUE THEN 1 ELSE 0 END) * 100.0 /
    NULLIF(SUM(CASE WHEN thumbs_up = TRUE THEN 1 ELSE 0 END) +
    SUM(CASE WHEN thumbs_down IS NOT NULL AND thumbs_down != '' THEN 1 ELSE 0 END), 0), 2) as positive_feedback_rate
FROM `wonder-recipe-prod.mongo_batch_recipe_agent.agent_conversation_logs`
WHERE created_time >= DATETIME_SUB(CURRENT_DATETIME(), INTERVAL 7 DAY)
```

### Query 3: Session-Level Details
```sql
SELECT
  user_name,
  chat_session_id,
  MAX(CASE WHEN thumbs_up = TRUE THEN 1 ELSE 0 END) as has_thumbs_up,
  MAX(CASE WHEN thumbs_down IS NOT NULL AND thumbs_down != '' THEN 1 ELSE 0 END) as has_thumbs_down,
  MAX(CASE WHEN is_interrupted = TRUE THEN 1 ELSE 0 END) as was_interrupted,
  MIN(created_time) as session_start,
  MAX(created_time) as session_end
FROM `wonder-recipe-prod.mongo_batch_recipe_agent.agent_conversation_logs`
WHERE created_time >= DATETIME_SUB(CURRENT_DATETIME(), INTERVAL 7 DAY)
GROUP BY user_name, chat_session_id
ORDER BY session_start DESC
```

## Report Format

### Most Frequent User
- Identify the top user by total messages
- Show their session count, invocations, and activity level

### Usage Rankings Table
| Rank | User | Sessions | Messages | Activity |

### Conversation Satisfaction
- Analyze positive vs negative indicators
- Calculate feedback rates
- Identify concerns or red flags

### Key Insights
1. Engagement metrics
2. Satisfaction measurement quality
3. Power user identification
4. Signs of user happiness/unhappiness

### Recommendations
Suggest improvements for next week's analysis

## Notes
- Table: `wonder-recipe-prod.mongo_batch_recipe_agent.agent_conversation_logs`
- Time range: Last 7 days from current date
- Key metrics: sessions, messages, feedback, interruptions
