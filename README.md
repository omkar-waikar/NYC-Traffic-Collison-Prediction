# FlowSight: NYC Traffic Collision Analysis and Prediction
## Background
<br>
New York City’s traffic congestion and accident rates present both economic and safety challenges. Despite the vast amounts of publicly available traffic and collision data, real-time actionable insights remain difficult to extract due to the high volume, velocity, and variety of the data.

FlowSight was developed to bridge this gap by creating a real-time data pipeline and predictive platform that leverages big data and machine learning. The goal is to help NYC transportation officials and urban planners proactively identify high-risk areas, predict potential accidents, and design targeted traffic control strategies.

## Objectives
● Build a real-time data streaming system to ingest collision and traffic speed data from NYC Open Data sources.

● Store and manage data efficiently to support both real-time analytics and batch processing.

● Develop predictive models to estimate collision likelihood at different geo-locations based on historical and live traffic data.

● Visualize the data and predictions through interactive dashboards for stakeholders and decision-makers.

● Provide an extendable system that can be scaled and customized to other cities or datasets.

## Key Components
### 1. Producer-Consumer Pipeline
A Kafka-based system pulls live data from NYC Open Data APIs:

### Producer:
○ Polls collision data every 30 seconds and traffic speed data every 5 minutes.

○ Serializes and publishes JSON events to Kafka topics (raw_collisions, traffic_speeds).

○ Maintains watermarks to avoid duplicate records.

○ Retry logic ensures fault tolerance if brokers are unavailable.
<br>

### Consumer:
○ Listens to Kafka topics and inserts data into MongoDB collections (collisions_ts, traffic_speeds).

○ Performs data validation and automatic retries on insertion failure.
<br>

### 2. Data Storage
MongoDB stores both streaming and batch data. Collections are indexed for fast query performance and geographical lookups.
<br>

### 3. ML Prediction Pipeline
● Objective: Predict crash scores for live data.

● Data Handling: Used Dask for distributed data handling.

● Feature Engineering: Mapped collision and traffic datasets using Ball Trees based on geo-coordinates.

● Model: Linear Regression model trained on historical data to predict crash likelihood.

● Output: Predicts most crash-prone streets at any given time; confidence scores currently range from 0.5–0.6.
<br>

### 4. Frontend Dashboard
● Developed using React and TypeScript.

● Connected to backend APIs to pull real-time and predicted data.

● Provided interactive data visualizations, tables, maps, and charts.

<br>

## Architecture Diagram
![alt text](https://github.com/Omkar-A-Waikar/NYC-Traffic-Collison-Prediction/blob/main/arch.PNG)

## Tech Stacks Used

| Layer           | Tools / Libraries                |
| --------------- | -------------------------------- |
| Data Ingestion  | Python, Requests, Kafka          |
| Messaging       | Apache Kafka                     |
| Data Storage    | MongoDB                          |
| Data Processing | Dask, Scikit-learn               |
| Model Training  | Scikit-learn (Linear Regression) |
| Backend APIs    | Python (Flask)                   |
| Frontend        | React, TypeScript, MUI           |
| Infrastructure  | Docker, Kubernetes (locally)     |
