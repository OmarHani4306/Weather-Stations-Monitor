# Weather Station Monitoring System

[![Java](https://img.shields.io/badge/Java-11+-orange.svg?style=for-the-badge&logo=java)](https://www.java.com)
[![Kafka](https://img.shields.io/badge/Apache_Kafka-231F20?style=for-the-badge&logo=apache-kafka&logoColor=white)](https://kafka.apache.org/)
[![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=for-the-badge&logo=elasticsearch&logoColor=white)](https://www.elastic.co/elasticsearch/)
[![Kibana](https://img.shields.io/badge/Kibana-005571?style=for-the-badge&logo=kibana&logoColor=white)](https://www.elastic.co/kibana/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)

A scalable, data-intensive application designed to handle high-frequency data streams from a network of IoT weather stations. This project, developed for the "Designing Data Intensive Applications" course (CSE-4E3), implements a full pipeline for data acquisition, processing, archiving, and analysis using modern stream-processing and storage technologies.

---

## System Architecture

The system is designed with a three-stage, decoupled architecture to ensure scalability and fault tolerance.

1.  **Data Acquisition**:
    * **10 Mock Weather Stations**: Each station is a containerized application that generates and sends weather status messages every second.
    * **Apache Kafka**: Acts as a central, high-throughput message bus that ingests data streams from all stations.

2.  **Data Processing & Archiving**:
    * **Base Central Station**: The core processing service. It consumes data from Kafka, processes it in real-time, and directs it to storage systems.
    * **Parquet File Archiving**: All incoming weather data is batched and archived into highly efficient, partitioned Parquet files for long-term storage and historical analysis.

3.  **Indexing & Analysis**:
    * **Bitcask Store**: A custom, high-performance key-value store implemented in Java to maintain the *latest* reading from each individual station for quick lookups.
    * **Elasticsearch & Kibana**: The archived Parquet files are indexed in Elasticsearch, enabling complex queries and the creation of rich, interactive visualizations and dashboards in Kibana.

---

## Key Features

-   **Real-time Data Streaming**: Utilizes Kafka to handle high-volume, high-velocity data from distributed IoT devices.
-   **Custom Key-Value Store**: A from-scratch implementation of a **Bitcask (Riak Core)** storage engine, featuring hint files for fast recovery and scheduled compaction to merge segment files.
-   **Efficient Data Archiving**: Employs batched writes to **Apache Parquet**, an optimized columnar storage format, with partitioning by time and station ID.
-   **Real-time Event Processing**: Implemented a **Kafka Processor** to detect "raining" conditions (humidity > 70%) in real-time and push alerts to a dedicated topic.
-   **Advanced Data Simulation**: Weather stations realistically simulate data with configurable battery statuses (`low`, `medium`, `high`) and a 10% random message drop rate to test system resilience.
-   **Performance Profiling**: The Central Station was profiled using **Java Flight Recorder (JFR)** to identify and optimize performance bottlenecks, memory consumption, and GC pauses.
-   **Interactive Dashboards**: Historical weather data is visualized in **Kibana**, with dashboards to analyze battery status distribution and message drop rates per station.
-   **(Bonus) Enterprise Integration Patterns**: Implemented 5-6 common EIPs such as the Dead-Letter Channel, Idempotent Receiver, and Polling Consumer for robust and reliable message handling.
-   **(Bonus) External API Integration**: Integrated with the **Open-Meteo API** to enrich the system's dataset with real-world weather data, managed via a Channel Adapter pattern.

---

## Tech Stack

| Category              | Technology                                                                                                                                                                                                                                                            |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Backend** | ![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)                                                                                                                                                                       |
| **Messaging** | ![Apache Kafka](https://img.shields.io/badge/Apache_Kafka-231F20?style=for-the-badge&logo=apache-kafka&logoColor=white) ![ZooKeeper](https://img.shields.io/badge/ZooKeeper-F5A623?style=for-the-badge&logo=apache-zookeeper&logoColor=white) |
| **Storage** | **Bitcask (Custom KV Store)**, **Apache Parquet** |
| **Search & Analytics**| ![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=for-the-badge&logo=elasticsearch&logoColor=white) ![Kibana](https://img.shields.io/badge/Kibana-005571?style=for-the-badge&logo=kibana&logoColor=white)            |
| **Infrastructure** | ![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white) ![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)                                             |
| **Profiling** | **Java Flight Recorder (JFR)** |

---

This project was developed as part of the CSE-4E3 course at the Faculty of Engineering, Alexandria University.
