# 🚀 Kafka Multi-Broker Cluster (Docker)

A production-style **Apache Kafka cluster with 3 brokers + Zookeeper**, fully containerized using Docker.

---

## 📌 Overview

This project demonstrates how to set up a **distributed Kafka cluster** with:

* 🧩 3 Kafka Brokers
* 🔁 Replication Factor = 3
* ⚖️ Leader Distribution across brokers
* 🔐 Fault-tolerant messaging
* 🐳 Docker-based deployment

---

## 🏗️ Architecture

```text
        +-------------------+
        |   Zookeeper       |
        |   (Coordination)  |
        +---------+---------+
                  |
     ---------------------------------
     |               |               |
+---------+     +---------+     +---------+
| Kafka1  |     | Kafka2  |     | Kafka3  |
| :9092   |     | :9093   |     | :9094   |
+---------+     +---------+     +---------+
```

---

## 🚀 Quick Start

```bash
git clone https://github.com/<your-username>/kafka-docker-cluster.git
cd kafka-docker-cluster

mkdir -p data/zookeeper data/kafka1 data/kafka2 data/kafka3
chmod -R 777 data

docker compose up -d
```

---

## 🔍 Verify Cluster

```bash
docker ps
```

You should see:

* kafka1
* kafka2
* kafka3
* zookeeper

---

## 🧪 Create Topic

```bash
docker exec -it kafka1 kafka-topics \
  --create \
  --topic test \
  --bootstrap-server kafka1:9092 \
  --partitions 3 \
  --replication-factor 3
```

---

## 📊 Describe Topic

```bash
docker exec -it kafka1 kafka-topics \
  --describe \
  --topic test \
  --bootstrap-server kafka1:9092
```

✔ Expected:

* ReplicationFactor: 3
* ISR: 1,2,3

---

## ✍️ Producer

```bash
docker exec -it kafka1 kafka-console-producer \
  --topic test \
  --bootstrap-server localhost:9092
```

---

## 📥 Consumer

```bash
docker exec -it kafka2 kafka-console-consumer \
  --topic test \
  --from-beginning \
  --bootstrap-server kafka2:9092
```

---

## 🔥 Fault Tolerance Test

```bash
docker stop kafka2
```

👉 Cluster continues working (replication in action)

---

## 🧠 Key Learnings

* Difference between **internal vs external listeners**
* Kafka **leader election & ISR**
* Multi-broker **replication**
* Docker networking pitfalls (`localhost` issue)

---

## 📂 Project Structure

```text
.
├── docker-compose.yml
├── README.md
├── .gitignore
└── data/   (ignored)
```

---

## 🛑 Stop Cluster

```bash
docker compose down -v
```


