Here’s a well-structured `README.md` file for your **Stock Market Project**:

---

````markdown
# 📈 Stock Market Project

This project demonstrates how to stream stock market data using **Apache Kafka**, deploy it on an **AWS EC2 instance**, and connect it to **Amazon S3**, **AWS Glue**, and **Amazon Athena** for further analysis.

## 🚀 Tech Stack

- Apache Kafka 3.3.1
- Amazon EC2
- Amazon S3
- AWS Glue (Crawler & Data Catalog)
- Amazon Athena
- Python (Jupyter notebooks)

---

## 📂 Kafka Setup on EC2

### ✅ Step 1: Start Zookeeper (Terminal 1)

```bash
ssh -i "stocks_key.pem" ec2-user@ec2-13-60-37-209.eu-north-1.compute.amazonaws.com

# Download Kafka
wget https://archive.apache.org/dist/kafka/3.3.1/kafka_2.13-3.3.1.tgz

# Extract Kafka files
tar -xzf kafka_2.13-3.3.1.tgz

# Install Java (Kafka dependency)
sudo yum install java-1.8.0-openjdk

cd kafka_2.13-3.3.1

# Create Kafka topic
bin/kafka-topics.sh --create --topic demo_test2 --bootstrap-server 13.60.37.209:9092 --replication-factor 1 --partitions 1
````

---

### ✅ Step 2: Start Kafka Server (Terminal 2)

```bash
ssh -i "stocks_key.pem" ec2-user@ec2-13-60-37-209.eu-north-1.compute.amazonaws.com

cd kafka_2.13-3.3.1

# Start Kafka server
bin/kafka-server-start.sh config/server.properties
```

🛠 **If "not enough space" error occurs:**

```bash
export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
bin/kafka-server-start.sh config/server.properties
```

---

### ✅ Step 3: Create Topic & Start Producer (Terminal 3)

```bash
ssh -i "stocks_key.pem" ec2-user@ec2-13-60-37-209.eu-north-1.compute.amazonaws.com

cd kafka_2.13-3.3.1

# Create topic
bin/kafka-topics.sh --create --topic demo_test2 --bootstrap-server 13.60.37.209:9092 --replication-factor 1 --partitions 1

# Start producer
bin/kafka-console-producer.sh --topic demo_test2 --bootstrap-server 13.60.37.209:9092
```

⚠️ **If TimedOut error occurs:**

1. Go to **EC2 > Security Groups**
   → Edit Inbound Rules
   → Add Rule
   → Type: **All Traffic**, Source: **Anywhere-IPv4**
   → Save

2. Update server.properties:

```bash
sudo nano config/server.properties
```

* Uncomment `listeners` and set to:

  ```
  listeners=PLAINTEXT://0.0.0.0:9092
  advertised.listeners=PLAINTEXT://13.60.37.209:9092
  ```
* Save and exit (Ctrl+O → Enter → Ctrl+X)

3. Rerun the producer command.

---

### ✅ Step 4: Start Consumer (Terminal 4)

```bash
ssh -i "stocks_key.pem" ec2-user@ec2-13-60-37-209.eu-north-1.compute.amazonaws.com

cd kafka_2.13-3.3.1

# Start consumer
bin/kafka-console-consumer.sh --topic demo_test2 --bootstrap-server 13.60.37.209:9092
```

---

## 🧪 Data Flow Pipeline

1. Kafka Producer sends stock data to **demo\_test2** topic.
2. Kafka Consumer receives and stores the data into **Amazon S3**.
3. AWS Glue **Crawler** reads the S3 data and creates a table in **AWS Glue Catalog**.
4. **Amazon Athena** queries the data directly from the Glue Catalog.

---

## 📓 Jupyter Notebooks

* `stock_producer.ipynb`: Sends data to Kafka topic
* `stock_consumer.ipynb`: Consumes data and saves it to S3

Run these notebooks locally or on EC2 after starting all Kafka services.

---

## ✅ Final Outcome

Real-time stock data is streamed via Kafka, stored in S3, and made queryable using Athena through the Glue Data Catalog — enabling efficient serverless data analytics.

---

## 🔐 Notes

* Ensure your EC2 Security Group allows open access or properly defined IP ranges.
* Replace IP `13.60.37.209` with your own EC2 public IP if needed.

---

## 📬 Contact

For questions or support, feel free to contact **Abdul Hannan**.

```

---

Let me know if you’d like it saved as a downloadable `.md` file or if you want to add screenshots, badges, or licensing.
```
