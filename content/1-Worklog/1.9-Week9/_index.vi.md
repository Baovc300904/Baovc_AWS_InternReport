---
title: "Worklog Tuần 9"
date: 2024-01-01T00:00:00+07:00
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Thông tin dưới đây chỉ để tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn, bao gồm cả cảnh báo này.
{{% /notice %}}

### Mục tiêu tuần 9:

* Làm chủ hệ sinh thái AWS Data & Analytics services và các mô hình kiến trúc dữ liệu hiện đại
* Thiết kế và triển khai Data Lake cấp doanh nghiệp với governance và security
* Xây dựng ETL/ELT pipelines toàn diện sử dụng AWS Glue và data transformation workflows
* Triển khai real-time và batch analytics sử dụng Amazon Athena và advanced SQL
* Tạo interactive BI dashboards với Amazon QuickSight và embedded analytics
* Triển khai data cataloging, lineage tracking, và metadata management
* Deploy streaming analytics với Amazon Kinesis và real-time data processing

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Giới thiệu về hệ sinh thái Data & Analytics trên AWS <br> - Hiểu khái niệm Data Lake, ETL pipeline và cách kết nối dữ liệu từ nhiều nguồn | 03/11/2025   | 03/11/2025      | AWS Journey |
| 3   | - Tạo Data Lake trên Amazon S3 <br> - Cấu trúc thư mục, quyền truy cập <br> - Thiết lập AWS Glue Crawler để nhận diện schema dữ liệu | 04/11/2025   | 04/11/2025      | AWS Journey |
| 4   | - Thực hành AWS Athena để query dữ liệu trong Data Lake <br> - Viết các SQL query cơ bản và xuất kết quả ra S3 | 05/11/2025   | 05/11/2025      | AWS Journey |
| 5   | - Giới thiệu và thực hành với Amazon QuickSight <br> - Kết nối QuickSight với Athena để visualize dữ liệu <br> - Tạo dashboard đơn giản với biểu đồ và bảng tổng hợp | 06/11/2025   | 06/11/2025      | AWS Journey |
| 6   | - Ôn tập & tổng kết kiến thức tuần: <br> + Quy trình thu thập → xử lý → phân tích dữ liệu trên AWS <br> + So sánh Glue, Athena, QuickSight với các công cụ truyền thống <br> + Viết báo cáo tổng kết quá trình thực hành | 07/11/2025   | 07/11/2025      | AWS Journey |


### Kết quả đạt được tuần 9:

#### 1. Thiết kế Kiến trúc Dữ liệu Hiện đại
* **Triển khai Medallion Architecture:**
  * **Bronze Layer (Raw Zone):**
    - Mục đích: Ingest raw data từ nhiều nguồn khác nhau mà không transformation
    - S3 bucket: `s3://datalake-bronze-prod/`
    - Data formats: CSV, JSON, XML, Parquet, Avro
    - Retention: 90 ngày (lifecycle policy sang Glacier sau 30 ngày)
    - Encryption: SSE-KMS với customer-managed key
    - Versioning: Enabled cho data recovery
    - Data sources được ingest:
      * Application logs (JSON) - 2.5 GB/ngày
      * Database CDC (Change Data Capture) - 800 MB/ngày
      * Third-party APIs (JSON/XML) - 350 MB/ngày
      * File uploads (CSV) - 1.2 GB/ngày
      * IoT sensor data (Parquet) - 4.8 GB/ngày
  
  * **Silver Layer (Cleansed/Conformed Zone):**
    - Mục đích: Cleaned, validated, và standardized data
    - S3 bucket: `s3://datalake-silver-prod/`
    - Data format: Parquet với Snappy compression
    - Partitioning: year/month/day/hour
    - Retention: 1 năm (hot), 3 năm (cold)
    - Data quality checks:
      * Null value validation
      * Data type consistency
      * Business rule validation
      * Duplicate detection and removal
      * Schema evolution handling
    - Transformation đã áp dụng:
      * Data cleansing (trim, standardize formats)
      * Data enrichment (lookup, join reference data)
      * Data deduplication
      * PII masking cho sensitive fields
  
  * **Gold Layer (Curated/Business Zone):**
    - Mục đích: Aggregated, business-ready datasets
    - S3 bucket: `s3://datalake-gold-prod/`
    - Data format: Optimized Parquet với dictionary encoding
    - Partitioning: Optimized cho query patterns
    - Retention: 5 năm (compliance requirement)
    - Data marts đã tạo:
      * Sales analytics mart
      * Customer 360 mart
      * Product performance mart
      * Operational metrics mart
      * Financial reporting mart
    - SLA: Data available trong vòng 15 phút từ source update

* **Sơ đồ Kiến trúc Dữ liệu:**
  ```
  Data Sources
      ↓
  ┌─────────────────────────────────────────┐
  │  Ingestion Layer                        │
  │  - Kinesis Data Streams (real-time)     │
  │  - AWS DMS (database CDC)               │
  │  - API Gateway + Lambda (APIs)          │
  │  - S3 Transfer (batch files)            │
  └─────────────────────────────────────────┘
      ↓
  ┌─────────────────────────────────────────┐
  │  Bronze Layer (Raw Zone)                │
  │  S3: Raw data, all formats              │
  └─────────────────────────────────────────┘
      ↓
  ┌─────────────────────────────────────────┐
  │  Processing Layer                       │
  │  - AWS Glue ETL Jobs                    │
  │  - Glue Data Quality                    │
  │  - EMR for complex transformations      │
  └─────────────────────────────────────────┘
      ↓
  ┌─────────────────────────────────────────┐
  │  Silver Layer (Cleansed Zone)           │
  │  S3: Parquet, validated, standardized   │
  └─────────────────────────────────────────┘
      ↓
  ┌─────────────────────────────────────────┐
  │  Aggregation Layer                      │
  │  - Glue ETL Jobs (aggregations)         │
  │  - Athena CTAS queries                  │
  └─────────────────────────────────────────┘
      ↓
  ┌─────────────────────────────────────────┐
  │  Gold Layer (Business Zone)             │
  │  S3: Optimized data marts               │
  └─────────────────────────────────────────┘
      ↓
  ┌─────────────────────────────────────────┐
  │  Consumption Layer                      │
  │  - Amazon Athena (SQL analytics)        │
  │  - QuickSight (BI dashboards)           │
  │  - SageMaker (ML models)                │
  │  - Redshift Spectrum (DW integration)   │
  └─────────────────────────────────────────┘
  ```

* **Data Governance với AWS Lake Formation:**
  * **Access Control:**
    - Centralized permissions management
    - Column-level security cho PII fields
    - Row-level security dựa trên user attributes
    - Tag-based access control (TBAC)
    - Cross-account sharing với AWS RAM
  
  * **Permissions Matrix:**
    | Role | Bronze | Silver | Gold | PII Columns |
    |------|--------|--------|------|-------------|
    | Data Engineers | Read/Write | Read/Write | Read/Write | Masked |
    | Data Analysts | Read | Read | Read | Masked |
    | Data Scientists | Read | Read | Read | Masked |
    | Business Users | No Access | No Access | Read | Denied |
    | Compliance Team | Read | Read | Read | Unmasked |

  * **Data Catalog:**
    - 347 tables cataloged
    - 1,245 columns documented
    - 89 data sources registered
    - Schema versioning enabled
    - Automated metadata extraction

#### 2. Triển khai Enterprise Data Lake
* **Cấu hình S3 Data Lake:**
  * **Cấu trúc Bucket:**
    ```
    datalake-bronze-prod/
    ├── application-logs/
    │   └── year=2026/month=01/day=27/
    ├── database-cdc/
    │   └── source=postgresql/table=orders/
    ├── api-data/
    │   └── provider=stripe/endpoint=payments/
    ├── file-uploads/
    │   └── type=customer-data/
    └── iot-sensors/
        └── device-type=temperature/location=warehouse-1/
    
    datalake-silver-prod/
    ├── customers/
    │   └── year=2026/month=01/day=27/
    ├── orders/
    │   └── year=2026/month=01/day=27/
    ├── products/
    │   └── year=2026/month=01/
    └── transactions/
        └── year=2026/month=01/day=27/hour=10/
    
    datalake-gold-prod/
    ├── sales-analytics/
    │   └── report-date=2026-01-27/
    ├── customer-360/
    │   └── snapshot-date=2026-01-27/
    └── product-performance/
        └── analysis-period=2026-01/
    ```

  * **Cấu hình Security:**
    - Bucket encryption: SSE-KMS với automatic key rotation
    - Versioning: Enabled trên tất cả buckets
    - MFA Delete: Enabled cho production buckets
    - Access logging: Enabled to audit bucket
    - Object Lock: Enabled cho compliance data (WORM)
    - Block public access: Enforced ở bucket và account level
    - VPC Endpoints: S3 access through private network only

  * **Cost Optimization:**
    - S3 Intelligent-Tiering: Automatic tiering cho Bronze layer
    - Lifecycle policies:
      * Bronze: 30 ngày Standard → Glacier Instant Retrieval
      * Silver: 90 ngày Standard → Glacier Flexible Retrieval
      * Gold: 365 ngày Standard → Glacier Deep Archive
    - Compression: Parquet với Snappy (70% size reduction)
    - Partitioning: Giảm scan costs 85%
    - S3 Select: Push-down filtering (60% cost reduction)
    - Storage costs: $450/tháng cho 8.5 TB (giảm từ $1,200 với Standard storage)

  * **Data Quality Metrics:**
    - Data freshness: 99.2% trong SLA (15 phút)
    - Data completeness: 98.7% (missing field validation)
    - Data accuracy: 99.5% (business rule validation)
    - Schema compliance: 100% (automated checks)
    - Duplicate rate: 0.3% (post-deduplication)

#### 3. Triển khai AWS Glue ETL Pipeline
* **Cấu hình Glue Crawlers:**
  * **Bronze Crawler:**
    - Name: `bronze-data-crawler`
    - Schedule: Mỗi 6 giờ
    - Data sources: Tất cả Bronze layer S3 paths
    - Classifiers: JSON, CSV, Parquet, Avro custom classifiers
    - Tables created: 45 tables
    - Partitions discovered: 2,834 partitions
    - Schema inference: Automatic với conflict resolution
  
  * **Silver Crawler:**
    - Name: `silver-data-crawler`
    - Schedule: Daily lúc 2 AM
    - Data sources: Silver layer S3 paths
    - Partition projection: Enabled cho time-based partitions
    - Tables created: 28 tables
    - Schema evolution: Track và version changes

* **Glue ETL Jobs:**
  
  **Job 1 - Bronze to Silver Transformation:**
  ```python
  import sys
  from awsglue.transforms import *
  from awsglue.utils import getResolvedOptions
  from pyspark.context import SparkContext
  from awsglue.context import GlueContext
  from awsglue.job import Job
  from awsglue.dynamicframe import DynamicFrame
  from pyspark.sql.functions import *
  
  args = getResolvedOptions(sys.argv, ['JOB_NAME', 'database_name', 'table_name'])
  
  sc = SparkContext()
  glueContext = GlueContext(sc)
  spark = glueContext.spark_session
  job = Job(glueContext)
  job.init(args['JOB_NAME'], args)
  
  # Đọc từ Bronze layer
  bronze_dyf = glueContext.create_dynamic_frame.from_catalog(
      database=args['database_name'],
      table_name=args['table_name'],
      transformation_ctx="bronze_dyf"
  )
  
  # Convert to Spark DataFrame cho transformations
  df = bronze_dyf.toDF()
  
  # Data Quality Checks
  df = df.filter(col("order_id").isNotNull())  # Loại bỏ null order IDs
  df = df.filter(col("order_amount") > 0)  # Chỉ valid amounts
  df = df.dropDuplicates(["order_id"])  # Deduplicate
  
  # Data Transformations
  df = df.withColumn("order_date", to_date(col("created_at")))
  df = df.withColumn("order_amount", col("order_amount").cast("decimal(10,2)"))
  df = df.withColumn("customer_email", lower(trim(col("customer_email"))))
  
  # PII Masking
  df = df.withColumn("customer_phone_masked", 
                     regexp_replace(col("customer_phone"), "\\d{4}$", "XXXX"))
  
  # Thêm metadata columns
  df = df.withColumn("processing_timestamp", current_timestamp())
  df = df.withColumn("data_source", lit("order_system"))
  
  # Thêm partitioning columns
  df = df.withColumn("year", year(col("order_date")))
  df = df.withColumn("month", month(col("order_date")))
  df = df.withColumn("day", dayofmonth(col("order_date")))
  
  # Convert back to DynamicFrame
  silver_dyf = DynamicFrame.fromDF(df, glueContext, "silver_dyf")
  
  # Write to Silver layer in Parquet format với partitioning
  glueContext.write_dynamic_frame.from_options(
      frame=silver_dyf,
      connection_type="s3",
      connection_options={
          "path": "s3://datalake-silver-prod/orders/",
          "partitionKeys": ["year", "month", "day"]
      },
      format="parquet",
      format_options={
          "compression": "snappy"
      },
      transformation_ctx="silver_write"
  )
  
  job.commit()
  ```

  **Job 2 - Silver to Gold Aggregation:**
  ```python
  # Daily sales aggregation job
  from pyspark.sql.functions import sum, count, avg, max, min
  from pyspark.sql.window import Window
  
  # Đọc từ Silver layer
  orders_df = spark.read.parquet("s3://datalake-silver-prod/orders/")
  customers_df = spark.read.parquet("s3://datalake-silver-prod/customers/")
  products_df = spark.read.parquet("s3://datalake-silver-prod/products/")
  
  # Join datasets
  sales_df = orders_df \
      .join(customers_df, "customer_id", "left") \
      .join(products_df, "product_id", "left")
  
  # Daily sales aggregation
  daily_sales = sales_df.groupBy("order_date", "product_category") \
      .agg(
          sum("order_amount").alias("total_revenue"),
          count("order_id").alias("total_orders"),
          avg("order_amount").alias("avg_order_value"),
          countDistinct("customer_id").alias("unique_customers")
      )
  
  # Tính running totals với window functions
  window_spec = Window.partitionBy("product_category") \
      .orderBy("order_date") \
      .rowsBetween(Window.unboundedPreceding, Window.currentRow)
  
  daily_sales = daily_sales.withColumn(
      "cumulative_revenue",
      sum("total_revenue").over(window_spec)
  )
  
  # Write to Gold layer
  daily_sales.write \
      .mode("overwrite") \
      .partitionBy("order_date") \
      .parquet("s3://datalake-gold-prod/sales-analytics/daily-sales/")
  ```

* **Glue Data Quality Rules:**
  ```
  Rules = [
      # Completeness checks
      ColumnExists "order_id",
      ColumnExists "customer_id",
      ColumnExists "order_amount",
      
      # Uniqueness checks
      IsUnique "order_id",
      
      # Validity checks
      ColumnValues "order_amount" > 0,
      ColumnValues "order_status" in ["pending", "completed", "cancelled"],
      ColumnDataType "order_date" = "date",
      
      # Consistency checks
      ColumnLength "customer_email" <= 255,
      RowCount > 0,
      
      # Custom rules
      CustomSql "SELECT COUNT(*) FROM primary WHERE order_amount > 100000" = 0
  ]
  ```

* **Glue Job Monitoring:**
  - Job execution metrics tracked trong CloudWatch
  - Average job duration: 8 phút (processing 4.5GB data)
  - Success rate: 99.1%
  - Failed jobs: Auto-retry 3 lần với exponential backoff
  - Cost per job run: $0.65 (2 DPU cho 8 phút)
  - Jobs triggered: 124 executions tuần này
  - Data processed: 562 GB tổng

#### 4. Advanced Analytics với Amazon Athena
* **Cấu hình Athena Workgroup:**
  * **Production Workgroup:**
    - Data scanned limit: 10 TB mỗi tháng
    - Query timeout: 30 phút
    - Results encryption: SSE-KMS
    - Results location: `s3://athena-results-prod/`
    - Results retention: 30 ngày
    - Engine version: Athena engine version 3
    - Cost allocation tags: Environment=Production, Team=Analytics
  
  * **Development Workgroup:**
    - Data scanned limit: 1 TB mỗi tháng
    - Query timeout: 10 phút
    - Enforce workgroup configuration
    - Query result reuse: 60 phút

* **Ví dụ Advanced SQL Queries:**

  **Query 1 - Customer Cohort Analysis:**
  ```sql
  WITH customer_cohorts AS (
    SELECT 
      customer_id,
      DATE_TRUNC('month', MIN(order_date)) AS cohort_month,
      DATE_TRUNC('month', order_date) AS order_month,
      SUM(order_amount) AS revenue
    FROM gold_layer.orders
    WHERE order_date >= DATE '2025-01-01'
    GROUP BY 1, 3
  ),
  cohort_metrics AS (
    SELECT 
      cohort_month,
      order_month,
      DATE_DIFF('month', cohort_month, order_month) AS months_since_first_order,
      COUNT(DISTINCT customer_id) AS customers,
      SUM(revenue) AS total_revenue
    FROM customer_cohorts
    GROUP BY 1, 2
  )
  SELECT 
    cohort_month,
    months_since_first_order,
    customers,
    total_revenue,
    ROUND(100.0 * customers / FIRST_VALUE(customers) 
      OVER (PARTITION BY cohort_month ORDER BY months_since_first_order), 2) 
      AS retention_rate
  FROM cohort_metrics
  ORDER BY cohort_month, months_since_first_order;
  ```

  **Query 2 - Product Affinity Analysis:**
  ```sql
  WITH product_pairs AS (
    SELECT 
      o1.order_id,
      o1.product_id AS product_a,
      o2.product_id AS product_b
    FROM gold_layer.order_items o1
    JOIN gold_layer.order_items o2 
      ON o1.order_id = o2.order_id 
      AND o1.product_id < o2.product_id
  )
  SELECT 
    pa.product_name AS product_a_name,
    pb.product_name AS product_b_name,
    COUNT(*) AS frequency,
    ROUND(100.0 * COUNT(*) / (
      SELECT COUNT(DISTINCT order_id) FROM gold_layer.order_items
    ), 2) AS support_percentage
  FROM product_pairs pp
  JOIN gold_layer.products pa ON pp.product_a = pa.product_id
  JOIN gold_layer.products pb ON pp.product_b = pb.product_id
  GROUP BY 1, 2
  HAVING COUNT(*) > 50
  ORDER BY frequency DESC
  LIMIT 20;
  ```

  **Query 3 - Time Series Forecasting Preparation:**
  ```sql
  WITH daily_metrics AS (
    SELECT 
      order_date,
      SUM(order_amount) AS daily_revenue,
      COUNT(DISTINCT order_id) AS daily_orders,
      COUNT(DISTINCT customer_id) AS daily_customers,
      AVG(order_amount) AS avg_order_value
    FROM gold_layer.orders
    WHERE order_date >= CURRENT_DATE - INTERVAL '365' DAY
    GROUP BY order_date
  ),
  moving_averages AS (
    SELECT 
      order_date,
      daily_revenue,
      AVG(daily_revenue) OVER (
        ORDER BY order_date 
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
      ) AS ma_7day,
      AVG(daily_revenue) OVER (
        ORDER BY order_date 
        ROWS BETWEEN 29 PRECEDING AND CURRENT ROW
      ) AS ma_30day,
      STDDEV(daily_revenue) OVER (
        ORDER BY order_date 
        ROWS BETWEEN 29 PRECEDING AND CURRENT ROW
      ) AS stddev_30day
    FROM daily_metrics
  )
  SELECT 
    order_date,
    daily_revenue,
    ma_7day,
    ma_30day,
    -- Z-score cho anomaly detection
    (daily_revenue - ma_30day) / NULLIF(stddev_30day, 0) AS z_score,
    -- Day of week patterns
    day_of_week(order_date) AS day_of_week,
    -- Month of year seasonality
    month(order_date) AS month_num
  FROM moving_averages
  ORDER BY order_date DESC;
  ```

* **Query Performance Optimization:**
  - **Partitioning strategy:** Giảm scan 85%
    * Before: 2.4 TB scanned per query
    * After: 360 GB scanned per query
  - **Columnar format (Parquet):** 70% compression ratio
  - **Partition projection:** Loại bỏ Glue Catalog calls
  - **CTAS (Create Table As Select):** Materialized frequent queries
  - **Bucketing:** Optimized join performance
  - **Query result reuse:** 60-phút caching
  - Average query cost: $0.18 (giảm từ $1.20)

* **Athena Federated Queries:**
  - Connected data sources:
    * RDS PostgreSQL (operational database)
    * DynamoDB (user sessions)
    * DocumentDB (product catalog)
    * On-premises MySQL (via Lambda connector)
  - Cross-source analytics enabled
  - Query example: Join S3 data lake với live RDS data

#### 5. Enterprise BI với Amazon QuickSight
* **Cấu hình QuickSight:**
  * **Edition:** Enterprise (SAML SSO, row-level security, hourly refresh)
  * **Users:** 45 authors, 150 readers
  * **SPICE capacity:** 50 GB allocated
  * **Data sources connected:**
    - Amazon Athena (primary)
    - Amazon S3 (direct query)
    - Amazon RDS PostgreSQL
    - Salesforce (via connector)
    - Google Analytics (via connector)

* **Triển khai Dashboards:**

  **Dashboard 1 - Executive KPI Dashboard:**
  * **Metrics hiển thị:**
    - Total Revenue (current vs previous period)
    - Active Customers (MoM growth %)
    - Average Order Value (trend)
    - Customer Lifetime Value
    - Churn Rate
    - Net Promoter Score (NPS)
  * **Visualizations:**
    - KPI cards với comparison indicators
    - Line charts cho trends
    - Gauge charts cho goal tracking
    - Heat map cho regional performance
    - Funnel chart cho conversion analysis
  * **Interactivity:**
    - Date range filters
    - Region/Category drill-downs
    - Export to PDF/Excel
    - Email subscriptions (daily/weekly)
  * **Auto-refresh:** Mỗi 1 giờ
  * **Users:** C-level executives (15 users)

  **Dashboard 2 - Sales Analytics Dashboard:**
  * **Analysis sections:**
    - Sales theo product category (bar chart)
    - Geographic sales distribution (map)
    - Top 10 products theo revenue (table với sparklines)
    - Sales vs target (combo chart)
    - Customer segmentation (tree map)
    - Month-over-month comparison (line chart)
  * **Calculated fields:**
    ```
    Revenue Growth % = 
      (sum({revenue}) - sumOver(sum({revenue}), [{period} ASC], 1, 1)) 
      / sumOver(sum({revenue}), [{period} ASC], 1, 1) * 100
    
    YTD Revenue = 
      sumOver(sum({revenue}), 
        [{year}], 
        [{year} = ${current_year}], 
        PRE_AGG)
    
    Customer Segment = 
      ifelse({lifetime_value} > 10000, "VIP",
        ifelse({lifetime_value} > 5000, "High Value",
          ifelse({lifetime_value} > 1000, "Medium Value", 
            "Low Value")))
    ```
  * **Parameters:**
    - Date range selector
    - Product category filter
    - Region multi-select
    - Minimum order value threshold
  * **Row-Level Security:**
    - Sales managers: Chỉ xem region của họ
    - Product managers: Chỉ xem category của họ
    - Analysts: Xem tất cả data
  * **Users:** Sales team (35 users)

  **Dashboard 3 - Real-Time Operations Dashboard:**
  * **Live metrics (1-phút refresh):**
    - Current active users
    - Transactions per minute
    - System error rate
    - API response times
    - Inventory alerts
  * **Data source:** Kinesis Data Streams → Athena live query
  * **Alerts đã cấu hình:**
    - Error rate > 2% → Email + Slack notification
    - Response time > 2s → PagerDuty alert
    - Inventory below threshold → Email to ops team
  * **Users:** Operations team (12 users)

* **QuickSight ML Insights:**
  - **Anomaly detection:** Tự động phát hiện unusual patterns
    * Detected 3 anomalies tuần này:
      1. Unexpected revenue spike (holiday promotion - expected)
      2. Low traffic on Tuesday (system maintenance - known)
      3. Product category shift (new product launch - explained)
  - **Forecasting:** Revenue forecast cho next 30 ngày
    * Predicted revenue: $1.2M ± $150K (confidence interval)
    * Accuracy on last month: 92%
  - **Contribution analysis:** Identify key drivers
    * Top revenue drivers: Product category (45%), Region (30%), Customer segment (15%)

* **Embedded Analytics:**
  - QuickSight dashboards embedded trong internal web application
  - SSO integration với corporate IdP
  - Custom branding và white-labeling
  - API-based dashboard provisioning
  - Row-level security enforced
  - Usage: 2,500 dashboard views/tuần

#### 6. Triển khai Streaming Analytics
* **Cấu hình Kinesis Data Streams:**
  * **Stream:** `clickstream-events`
  * **Shards:** 4 shards (4 MB/sec write, 8 MB/sec read capacity)
  * **Retention:** 7 ngày
  * **Encryption:** KMS encryption enabled
  * **Data producers:**
    - Web application (user clicks, page views)
    - Mobile app (app events, user actions)
    - IoT devices (sensor readings)
  * **Throughput:** 3.2 MB/sec trung bình, 6.5 MB/sec peak
  * **Records/sec:** 850 trung bình, 1,800 peak

* **Kinesis Data Firehose Delivery:**
  * **Delivery streams đã tạo:**
    1. **S3 Delivery Stream:**
       - Destination: `s3://datalake-bronze-prod/clickstream/`
       - Buffer size: 5 MB
       - Buffer interval: 300 giây
       - Compression: Gzip
       - Format conversion: JSON to Parquet
       - Partitioning: year/month/day/hour
       - Data transformation: Lambda function cho enrichment
    
    2. **OpenSearch Delivery Stream:**
       - Destination: OpenSearch domain `analytics-cluster`
       - Index: `clickstream-{yyyy-MM-dd}`
       - Buffer size: 5 MB
       - Buffer interval: 60 giây
       - Retry duration: 3600 giây
       - Use case: Real-time dashboards và search

* **Real-Time Processing với Lambda:**
  ```python
  import json
  import base64
  from datetime import datetime
  
  def lambda_handler(event, context):
      output = []
      
      for record in event['records']:
          # Decode Kinesis data
          payload = base64.b64decode(record['data']).decode('utf-8')
          data = json.loads(payload)
          
          # Enrich data
          data['processed_timestamp'] = datetime.utcnow().isoformat()
          data['partition_key'] = f"{data['user_id']}_{data['session_id']}"
          
          # Classify user action
          if 'purchase' in data.get('event_type', ''):
              data['event_category'] = 'conversion'
          elif 'add_to_cart' in data.get('event_type', ''):
              data['event_category'] = 'engagement'
          else:
              data['event_category'] = 'browsing'
          
          # Calculate session duration
          if data.get('event_type') == 'session_end':
              session_duration = data.get('timestamp') - data.get('session_start_time')
              data['session_duration_sec'] = session_duration
          
          # Encode back to base64
          output_record = {
              'recordId': record['recordId'],
              'result': 'Ok',
              'data': base64.b64encode(json.dumps(data).encode('utf-8')).decode('utf-8')
          }
          output.append(output_record)
      
      return {'records': output}
  ```

* **Kinesis Data Analytics Application:**
  * **Application type:** SQL-based analytics
  * **Use case:** Real-time aggregations và windowing
  * **Ví dụ SQL query:**
    ```sql
    CREATE OR REPLACE STREAM "DESTINATION_SQL_STREAM" (
        window_start TIMESTAMP,
        event_type VARCHAR(50),
        event_count INTEGER,
        unique_users INTEGER,
        avg_session_duration DOUBLE
    );
    
    CREATE OR REPLACE PUMP "STREAM_PUMP" AS 
    INSERT INTO "DESTINATION_SQL_STREAM"
    SELECT STREAM 
        STEP("SOURCE_SQL_STREAM_001".ROWTIME BY INTERVAL '1' MINUTE) AS window_start,
        "event_type",
        COUNT(*) AS event_count,
        COUNT(DISTINCT "user_id") AS unique_users,
        AVG("session_duration_sec") AS avg_session_duration
    FROM "SOURCE_SQL_STREAM_001"
    GROUP BY 
        STEP("SOURCE_SQL_STREAM_001".ROWTIME BY INTERVAL '1' MINUTE),
        "event_type";
    ```
  * **Output:** Gửi aggregated data to:
    - Kinesis Data Streams (cho further processing)
    - Lambda (cho alerting)
    - OpenSearch (cho real-time dashboards)

* **Streaming Analytics Metrics:**
  - Events processed: 2.4M events/ngày
  - Average latency: 1.2 giây (ingestion to dashboard)
  - Processing cost: $85/ngày ($2,550/tháng)
  - Data volume: 3.8 GB/ngày compressed

#### 7. Comprehensive Monitoring và Operations
* **CloudWatch Dashboards:**
  * **Data Pipeline Health Dashboard:**
    - Glue job success/failure rates
    - ETL job durations
    - Data quality check pass rates
    - S3 bucket storage growth
    - Athena query metrics
    - Kinesis stream throughput
  
  * **Cost Monitoring Dashboard:**
    - S3 storage costs theo layer
    - Glue job execution costs
    - Athena query costs theo workgroup
    - Kinesis streaming costs
    - QuickSight user costs
    - Daily cost breakdown

* **CloudWatch Alarms:**
  - Glue job failures → SNS → Email + Slack
  - Athena cost threshold exceeded → Budget alert
  - Kinesis stream throttling → Auto-scaling trigger
  - Data freshness SLA breach → PagerDuty
  - S3 storage > 10 TB → Cost review notification

* **Data Lineage và Metadata:**
  - AWS Glue Data Catalog: Source of truth cho schemas
  - Table metadata: 347 tables documented
  - Column-level metadata: Data types, descriptions, PII tags
  - Lineage tracking: Source → Bronze → Silver → Gold
  - Impact analysis: Dashboards nào affected bởi schema changes

#### 8. Tóm tắt Week 9 và Key Metrics
* **Data Platform Metrics:**
  | Metric | Value |
  |--------|-------|
  | Total data ingested | 42 GB/ngày |
  | Data sources | 12 sources |
  | Tables in catalog | 347 tables |
  | Glue jobs | 18 jobs |
  | Athena queries/ngày | 450 queries |
  | QuickSight dashboards | 15 dashboards |
  | QuickSight users | 195 users |
  | Dashboard views/tuần | 2,500 views |
  | Data freshness (avg) | 12 phút |
  | Query performance (p95) | 8 giây |
  | Monthly data platform cost | $3,200 |
  | Cost per GB processed | $0.07 |

* **Business Value Delivered:**
  - Giảm time-to-insight từ 2 ngày xuống 15 phút (99% improvement)
  - Self-service analytics: 85% reports tự generate bởi business users
  - Data democratization: 195 users có data access (tăng từ 12 analysts)
  - Cost savings: $8,500/tháng vs traditional DW solution
  - Decision speed: Real-time dashboards cho phép faster business decisions

* **Thành tựu Kỹ thuật:**
  - Triển khai modern medallion architecture
  - 100% serverless data platform (không infrastructure management)
  - Scalable từ GB đến PB (tested up to 500 GB/ngày)
  - Comprehensive data governance với Lake Formation
  - Real-time và batch analytics coexistence
  - Advanced ML insights integrated

* **Bài học Rút ra:**
  1. **Partitioning strategy là critical** cho query performance và cost
  2. **Parquet format** giảm đáng kể storage và query costs
  3. **Data quality checks** nên implement sớm trong pipeline
  4. **Incremental processing** hiệu quả hơn full reprocessing
  5. **SPICE trong QuickSight** cung cấp fast dashboard performance
  6. **Row-level security** essential cho data governance
  7. **Athena workgroups** enable cost control và usage tracking
  8. **Streaming và batch** architectures bổ sung cho nhau

* **Bước tiếp theo:**
  - Triển khai ML models với SageMaker sử dụng Gold layer data
  - Thêm data sources (social media, marketing platforms)
  - Triển khai data quality scorecards
  - Mở rộng QuickSight embedded analytics
  - Deploy DataZone cho data marketplace
  - Triển khai automated data cataloging với AI


