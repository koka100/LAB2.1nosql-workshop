graph LR
    A[Стационарные/Мобильные Датчики<br/>Росгидромет, IoT RU<br/>Потоковые данные] -->|JSON Streams| B[Yandex Managed Kafka<br/>Topics: sensors_ru]
    C[Спутниковые Снимки<br/>Роскосмос API<br/>Batch данные] -->|ETL| D[Airbyte<br/>Self-hosted on Yandex Compute]
    B --> E[Yandex Object Storage<br/>Raw Zone, RU DCs]
    D --> E
    E --> F[Delta Lake<br/>Bronze/Silver/Gold Tables<br/>ГОСТ Encryption]
    F --> G[Apache Flink<br/>Real-time Processing on Yandex Data Proc<br/>(Alerts, Aggregation)]
    F --> H[Apache Spark on Yandex Managed Spark<br/>Batch/ML with CatBoost<br/>(Forecasting, Modeling)]
    G --> I[Delta Lake<br/>Curated Zone]
    H --> I
    I --> J[Yandex DataLens / Superset<br/>Dashboards & Reports, RU-compliant]
    I --> K[ML Models<br/>MLflow Self-hosted]
    L[Аналитики / Дашборды<br/>Росприроднадзор Reports] <-- J
    M[Apache Airflow<br/>Orchestration on Yandex Compute] -.->|DAGs| G
    M -.->|DAGs| H
    N[Apache Atlas<br/>Governance, Self-hosted] -.->|Metadata| F
    O[Prometheus + Grafana + Yandex Monitoring<br/>RU Metrics] -.->|Metrics| B
    O -.->|Metrics| G
    P[Yandex IAM + ГОСТ KMS<br/>Security, ФСТЭК Compliance] -.->|Access/Encryption| E
    P -.->|Access/Encryption| F
