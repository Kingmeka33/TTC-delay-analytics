# 🚇 TTC Delay Analytics  

Analyzing **Toronto public transit delay data** (bus, subway, streetcar) to identify delay hotspots, root causes, and generate valuable insights.  
This project builds an **ETL (Extract, Transform, Load) pipeline** on Google Cloud Platform (GCP), automates workflows using **Airflow**, and visualizes results with **Looker Studio dashboards**.  
<img width="2100" height="1125" alt="Image" src="https://github.com/user-attachments/assets/d5c91e4d-3eea-4c07-9fbb-c6590fb17cb9" />
---

## 📑 Table of Contents  
- [Overview](#overview)  
- [Architecture](#architecture)  
- [Source Data](#source-data)  
- [Subway Data](#subway-data)  
- [Bus & Streetcar Data](#bus--streetcar-data)  
- [Infrastructure](#infrastructure)  
- [ETL Pipeline](#etl-pipeline)  
- [Airflow DAGs](#airflow-dags)  
- [Dashboard](#dashboard)  
- [Key Learnings](#key-learnings)  
- [Acknowledgments](#acknowledgments)  

---

## 📂 Overview  

- **Data Source:** City of Toronto [Open Data Catalogue](https://open.toronto.ca/)  
- **Goal:** Detect delay patterns, understand root causes, and provide actionable insights.  
- **Stack:**  
  - **Terraform** for infrastructure as code.  
  - **Google Cloud VM** for orchestration.  
  - **GCS** as the data lake.  
  - **BigQuery** as the data warehouse.  
  - **Spark** for batch processing.  
  - **Docker + Airflow** for workflow automation.  
  - **Looker Studio** for visualization.  

---

## 🏗️ Architecture  

![Architecture Diagram](diagram)  

---

## 🗂️ Source Data  

### 🚆 Subway Data  
| Field Name | Description | Example |  
|------------|-------------|---------|  
| Date | Date (YYYY/MM/DD) | 12/31/2016 |  
| Time | Time (24h clock) | 1:59 |  
| Day | Day of week | Saturday |  
| Station | TTC subway station | Rosedale Station |  
| Code | TTC delay code | MUIS |  
| Min Delay | Delay (minutes) | 5 |  
| Min Gap | Gap between trains (minutes) | 9 |  
| Bound | Direction of train | N |  
| Line | TTC line (YU, BD, SHP, SRT) | YU |  
| Vehicle | Train number | 5961 |  

### 🚌 Bus & Streetcar Data  
| Field Name | Description | Example |  
|------------|-------------|---------|  
| Report Date | Incident date (YYYY/MM/DD) | 6/20/2017 |  
| Route | Bus route number | 51 |  
| Time | Incident time (hh:mm:ss AM/PM) | 12:35:00 AM |  
| Day | Day of week | Monday |  
| Location | Incident location | York Mills Station |  
| Incident | Description of delay cause | Mechanical |  
| Min Delay | Delay in minutes | 10 |  
| Min Gap | Scheduled gap (minutes) | 20 |  
| Direction | Route direction (NB, SB, EB, WB, BW) | N |  
| Vehicle | Vehicle number | 1057 |  

---

## ⚙️ Infrastructure  

- **Terraform** provisions resources.  
- **Google Cloud VM instance** runs Spark + Airflow.  
- **Google Cloud Storage (GCS):** Stores raw and processed data (data lake).  
- **BigQuery:** Stores structured and cleaned datasets (data warehouse).  
- **Dockerized Airflow:** Automates pipeline orchestration.  

<img width="2052" height="1576" alt="Image" src="https://github.com/user-attachments/assets/ad4a0375-275d-4255-9dbc-2842c841ba60" />

---

## 🔄 ETL Pipeline  

- **Batch Processing:** Data processed yearly using Spark (Airflow scheduled).  
- Can be scheduled monthly, but not cost-efficient for large-scale runs.  

### Airflow DAGs  

1. **`src_to_gcs_dag.py`**  
   - Downloads raw TTC delay data from Toronto Open Data Portal.  
   - Converts to **Parquet format**.  
   - Uploads to **GCS bucket**.  
   - Removes local copy from VM to save storage.  
   <img width="3175" height="292" alt="Image" src="https://github.com/user-attachments/assets/7db396c1-021c-42db-aff0-095f26a75b75" /> 

2. **`dataproc_job_dag.py`**  
   - Uploads Spark job (`spark_job.py`) to GCS for Dataproc.  
   - Configures service account for GCP integration.  
   - Creates Dataproc cluster → runs job → deletes cluster (cost optimization).  
  <img width="2548" height="1371" alt="Image" src="https://github.com/user-attachments/assets/f12c7c10-1c01-4ab8-942b-6ca25934e481" />

---

## 📊 Dashboard  

- **Looker Studio Dashboard:** [View Dashboard ↗](looker studio dashboard)  
- Provides insights into:  
  - Delay hotspots (routes, stations).  
  - Root causes (mechanical, weather, infrastructure).  
  - Patterns by day, time, and direction.  

<img width="2069" height="1593" alt="Image" src="https://github.com/user-attachments/assets/9eb00bf0-8666-4110-ac5b-617cbfe84056" />  

> Note: No delay records available for **July – December 2023** (data snapshot taken in August 2023).  

---

## 🌐 Key Learnings  

✅ Building scalable pipelines with **Airflow + Spark** on GCP.  
✅ Leveraging **Terraform** for reproducible infrastructure.  
✅ Cost optimization with **Dataproc ephemeral clusters**.  
✅ Translating raw transit data into **actionable BI dashboards**.  

---

## 🎉 Acknowledgments  

- **City of Toronto Open Data Portal** for making TTC delay data publicly available.  
- Special thanks to the **open-source data engineering community** for inspiration on best practices in cloud pipelines.  
<img width="2065" height="1605" alt="Image" src="https://github.com/user-attachments/assets/338ca0eb-4434-4952-98bb-0f1d030092ef" />
<img width="2079" height="1589" alt="Image" src="https://github.com/user-attachments/assets/f8917dae-564b-4a1d-afcd-ccd34b3cd145" />
---
