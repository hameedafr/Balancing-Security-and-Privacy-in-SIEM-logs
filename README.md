# 🔐 Balancing Security and Privacy in SIEM Logs

A Final Year Project implementing a privacy-preserving and secure SIEM pipeline using the **ELK Stack (Elasticsearch, Logstash, Kibana)** and **Filebeat**. The system is designed to ensure compliance with privacy laws (GDPR, CCPA) while maintaining log utility for real-time threat detection.

---

## 📌 Project Overview

Modern SIEM systems collect extensive log data that often includes Personally Identifiable Information (PII). This project builds a secure logging architecture that protects sensitive information through:

- ✅ **End-to-End Encryption (TLS)**
- ✅ **Tokenization & Redaction of PII**
- ✅ **Field-level Anonymization with SHA-256**
- ✅ **Role-Based Access Control (RBAC)**
- ✅ **Real-Time Visualization Dashboards in Kibana**

---

## 🚀 Technologies Used

| Component      | Purpose                          |
|----------------|----------------------------------|
| Filebeat       | Log shipping from agent machines |
| Logstash       | Ingestion + privacy filters      |
| Elasticsearch  | Secure log storage               |
| Kibana         | Visualization & dashboards       |
| TLS / SHA-256  | Secure transport & hashing       |
| Ubuntu Server  | Hosting the ELK stack            |

---

## 🔧 Installation and Configuration

### 1. **Install ELK Stack on Ubuntu (Main Server)**

Follow [official Elastic Stack setup](https://www.elastic.co/guide/index.html) or refer to your [Installation Manual PDF] for:

- Installing **Elasticsearch**
- Configuring **Logstash pipelines with privacy filters**
- Running **Kibana** on port `5601` with secure HTTPS

### 2. **Configure Logstash Privacy Pipeline**

Sample filters:
```logstash
filter {
  grok { match => { "message" => "%{SYSLOGTIMESTAMP:timestamp} %{IP:ip} %{GREEDYDATA:msg}" } }
  fingerprint {
    source => ["ip"]
    target => "tokenized_ip"
    method => "SHA256"
  }
  mutate {
    replace => { "ip" => "[REDACTED]" }
  }
}
