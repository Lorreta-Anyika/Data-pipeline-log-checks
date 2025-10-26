# Data Pipeline Health Monitoring System
### This is an assignment given by Ability and Mr. Mayowa Akindele for the python series of our DE journey. It is meant to test my use of condtional statements and logical thinking.

## Backgroud to the Challenge

At **AeroLake Analytics**, a fictional, fast-growing data company, hundreds of data pipelines move raw data from multiple sources — web analytics, IoT sensors, customer transactions — into a centralized **data lakehouse** every day.  

But as our system scaled, we faced a growing challenge:  
> “How do we know when a pipeline silently fails, slows down, or ingests incomplete data before it affects dashboards and business decisions?”

Manual checks weren’t scalable. We needed an **automated monitoring system** that could analyze pipeline logs and classify their health in real-time.  

That is where **LakeWatch** was born — a simple but powerful data quality evaluator that analyzes pipeline metadata logs and flags performance or reliability issues before they escalate.  

---

## Problem Definition

In modern **data lakehouse architectures**, multiple pipelines extract, transform, and load (ETL) data every few minutes or hours.  
Each run produces **log metadata** describing:

- Request and response codes  
- Data ingestion time and record counts  
- Duration and latency  
- Errors or warnings  

Without a centralized health evaluator, teams spend hours digging through logs just to confirm if a pipeline succeeded or failed.  
**LakeWatch** automates this evaluation by reading pipeline logs, applying health rules, and producing an instant health classification.

---

## Objectives

The system aims to:
1. **Analyze** pipeline logs for quality, performance, and reliability.  
2. **Classify** each run into one of three health categories:  
   - ✅ **HEALTHY**  
   - ⚠️ **WARNING**  
   - 🚨 **CRITICAL**  
3. **Summarize** pipeline performance across multiple sources.  
4. (Bonus) Flag **High Priority Alerts** for critical failures that occur during midnight operations (00:00–04:00 UTC).

---

## Evaluation Logic

Each log entry follows a JSON-like format, for example:

```python
{
    "pipeline_name": "user_activity_ingestion",
    "status_code": 200,
    "errors": [],
    "warnings": ["late data arrival"],
    "duration_seconds": 480,
    "max_latency_seconds": 8,
    "record_count": 1200,
    "ingestion_time": "2025-10-25T01:45:00Z"
}
```
The evaluator applies the following rules:

**HEALTHY** if:

- status_code == 200

- errors is empty

- warnings is empty or only "late data arrival"

- duration_seconds < 600

- max_latency_seconds < 10

**WARNING** if:

- status_code == 200 and

- 600 ≤ duration_seconds ≤ 1200, or

- 10 ≤ max_latency_seconds ≤ 30, or

- warnings contain other messages
- or fewer than 100 records ingested (record_count < 100)


**CRITICAL** if:

- status_code != 200, or

- errors exist, or

- duration_seconds > 1200, or

- max_latency_seconds > 30, or

- record_count == 0

💡 Bonus Rule:

If a pipeline is CRITICAL and its ingestion_time is between 00:00–04:00 UTC, it is flagged for High Priority Alert.

 ## Implementation

Two core functions power the system:

```python
def evaluate_pipeline_health(log: dict) -> dict:
    # Evaluates a single log and appends a 'health_status' key.
    ...

def evaluate_all_pipelines(logs: List[dict]) -> List[dict]:
    # Applies the evaluation function to multiple logs and prints a summary.
```
## Summary

LakeWatch helps data engineers:

- Detect bottlenecks before downstream systems break.

- Maintain data reliability across distributed pipelines.

- Reduce manual log inspection time.

It’s a small but meaningful step toward intelligent data observability — ensuring every byte that flows into your lakehouse is monitored, trusted, and timely.

## Tools Used

- Python 3.x

- Google Colab for prototyping

- GitHub for version control and documentation

🧑🏽‍💻 Author:

**Data Enthusiast**

- Linkedin @ Uchechukwu Lorreta Anyika
- X @ Lorreta Anyika

Building data-driven systems that bridge insights and i

