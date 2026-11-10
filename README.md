# End-to-End SRE Implementation using Datadog

**Infrastructure Monitoring | AWS | Datadog | Grafana | Elasticsearch | Kibana | Ubuntu | Red Hat | Observability | Alerting**

## 1. Project Overview

This project focuses on implementing a Site Reliability Engineering (SRE) and observability solution using monitoring tools in an AWS cloud environment.

The main objective is to monitor infrastructure health, application performance, and system logs through metrics collection, monitoring dashboards, and alerting mechanisms.

The project covers AWS EC2 infrastructure setup, Datadog Agent installation, infrastructure monitoring, dashboard creation, and email alert configuration.

It also includes Grafana visualization and ELK Stack (Elasticsearch and Kibana) installation procedures for learning metrics visualization and centralized log analysis.

## 2. Technologies Used

| Category | Technologies |
|---|---|
| Cloud Platform | AWS |
| Compute Service | Amazon EC2 |
| Cloud Monitoring | AWS CloudWatch |
| Monitoring | Datadog |
| Metrics Visualization | Grafana |
| Log Storage & Search | Elasticsearch |
| Log Visualization & Analysis | Kibana |
| Operating Systems | Ubuntu Linux, Red Hat Enterprise Linux (RHEL) |
| Alerting | Datadog Email Notifications |
| Scripting | Linux Shell |

## 3. Architecture & Workflow

```text
                    AWS Cloud
                        |
                        v
                    EC2 Instance
                        |
              +---------+---------+
              |                   |
              v                   v
         Datadog Agent        System Logs
              |                   |
              v                   v
        Datadog Platform     Elasticsearch
              |                   |
         +----+----+              v
         |         |            Kibana
         v         v              |
     Dashboards  Monitors         v
         |         |         Log Visualization
         v         v
      Metrics   Email Alerts
      Analysis      |
                    v
             Incident Detection

        Grafana --> Metrics Visualization
```

## 4. Project Implementation

### 4.1 AWS EC2 Infrastructure Setup

- Created and configured an AWS EC2 instance.
- Connected to the Linux instance using SSH.
- Updated system packages.
- Prepared the monitoring environment.
- Explored AWS CloudWatch metrics.

### 4.2 Datadog Agent Installation

- Installed and configured the Datadog Agent on EC2.
- Connected the EC2 environment to Datadog.
- Enabled infrastructure metrics collection.
- Verified Datadog Agent service status.

### 4.3 Infrastructure Monitoring

- Monitored CPU utilization.
- Tracked memory consumption.
- Monitored disk usage.
- Observed infrastructure performance.
- Created monitoring dashboards.

### 4.4 Datadog Dashboards

- Created monitoring dashboards.
- Visualized infrastructure performance metrics.
- Tracked CPU, memory, and disk utilization.
- Monitored system health.

### 4.5 AWS CloudWatch Monitoring

- Explored AWS CloudWatch monitoring.
- Worked with AWS infrastructure metrics.
- Studied cloud monitoring and observability.

### 4.6 Datadog Alerting & Email Notifications

- Created monitoring alerts using Datadog Monitors.
- Configured alert conditions to detect infrastructure issues.
- Set up email notifications for triggered alerts.
- Verified alert triggering through email notifications.
- Practiced proactive incident detection and response.

### 4.7 Grafana Installation & Configuration (Ubuntu)

**Step 1: Update the server**

```bash
sudo apt update
sudo apt upgrade -y
```

**Step 2: Install required packages**

```bash
sudo apt-get install -y apt-transport-https wget gnupg
```

**Step 3: Add Grafana GPG key**

```bash
sudo mkdir -p /etc/apt/keyrings

sudo wget -O /etc/apt/keyrings/grafana.asc \
https://apt.grafana.com/gpg-full.key

sudo chmod 644 /etc/apt/keyrings/grafana.asc
```

**Step 4: Add Grafana repository**

```bash
echo "deb [signed-by=/etc/apt/keyrings/grafana.asc] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
```

**Step 5: Install Grafana**

```bash
sudo apt-get update
sudo apt-get install grafana -y
```

**Step 6: Start Grafana service**

```bash
sudo systemctl daemon-reload
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```

**Step 7: Check Grafana status**

```bash
sudo systemctl status grafana-server
```

**Step 8: Access Grafana**

```text
http://<EC2-PUBLIC-IP>:3000
```

Default login:

```text
Username: admin
Password: admin
```

Change the default password when prompted.

### 4.8 Grafana Visualization & Dashboard Creation

**Step 1: Configure Data Source**

- Log in to Grafana.
- Navigate to Connections → Data Sources.
- Select a supported metrics data source, such as Prometheus.
- Configure the data source connection.
- Click Save & Test.

**Step 2: Create Dashboard**

- Navigate to Dashboards.
- Click New Dashboard.
- Select Add Visualization.
- Choose the configured data source.
- Create dashboard panels using available metrics.

**Step 3: Configure Visualization Panels**

| Panel | Visualization Type |
|---|---|
| CPU Usage | Time-Series Graph |
| Memory Usage | Gauge |
| Disk Usage | Gauge |
| Network Traffic | Time-Series Graph |
| Server Performance | Dashboard |

**Step 4: Customize Dashboard**

- Add panel titles.
- Configure metric queries.
- Select visualization types.
- Set dashboard refresh intervals.
- Save the dashboard.

**Important:** Grafana requires a configured metrics data source. Installing Grafana alone does not automatically populate CPU, memory, or disk metrics.

### 4.9 Elasticsearch Installation & Configuration

Elasticsearch is used to store, index, and search log data. It is the backend required for the Kibana log visualization workflow.

#### A. Ubuntu Installation

**Step 1: Install required packages**

```bash
sudo apt-get update
sudo apt-get install -y wget gnupg apt-transport-https
```

**Step 2: Add Elasticsearch GPG key**

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```

**Step 3: Add Elasticsearch repository**

```bash
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-9.x.list
```

**Step 4: Install Elasticsearch**

```bash
sudo apt-get update
sudo apt-get install elasticsearch -y
```

**Step 5: Enable and start Elasticsearch**

```bash
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

**Step 6: Check Elasticsearch status**

```bash
sudo systemctl status elasticsearch
```

**Step 7: View Elasticsearch logs**

```bash
sudo journalctl -u elasticsearch -f
```

#### B. Red Hat Enterprise Linux (RHEL) Installation

**Step 1: Import Elasticsearch GPG key**

```bash
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

**Step 2: Create Elasticsearch repository**

```bash
sudo vi /etc/yum.repos.d/elasticsearch.repo
```

Add the following configuration:

```ini
[elasticsearch]
name=Elasticsearch repository for 9.x packages
baseurl=https://artifacts.elastic.co/packages/9.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=0
autorefresh=1
type=rpm-md
```

**Step 3: Install Elasticsearch**

```bash
sudo dnf install --enablerepo=elasticsearch elasticsearch -y
```

For older RHEL versions:

```bash
sudo yum install --enablerepo=elasticsearch elasticsearch -y
```

**Step 4: Enable and start Elasticsearch**

```bash
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

**Step 5: Check Elasticsearch status**

```bash
sudo systemctl status elasticsearch
```

### 4.10 Kibana Installation & Configuration

Kibana provides a web interface to search, explore, and visualize data stored in Elasticsearch.

#### A. Ubuntu Installation

**Step 1: Update packages**

```bash
sudo apt-get update
```

**Step 2: Install Kibana**

Use the Elasticsearch repository and signing key configured in Section 4.9.

```bash
sudo apt-get install kibana -y
```

**Step 3: Enable Kibana service**

```bash
sudo systemctl daemon-reload
sudo systemctl enable kibana
```

**Step 4: Start Kibana**

```bash
sudo systemctl start kibana
```

**Step 5: Check Kibana status**

```bash
sudo systemctl status kibana
```

**Step 6: View Kibana logs**

```bash
sudo journalctl -u kibana -f
```

#### B. Red Hat Enterprise Linux (RHEL) Installation

**Step 1: Import Elastic GPG key**

```bash
sudo rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

**Step 2: Create Kibana repository**

```bash
sudo vi /etc/yum.repos.d/kibana.repo
```

Add the following configuration:

```ini
[kibana-9.x]
name=Kibana repository for 9.x packages
baseurl=https://artifacts.elastic.co/packages/9.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

**Step 3: Install Kibana**

```bash
sudo dnf install kibana -y
```

For older RHEL versions:

```bash
sudo yum install kibana -y
```

**Step 4: Enable and start Kibana**

```bash
sudo systemctl daemon-reload
sudo systemctl enable kibana
sudo systemctl start kibana
```

**Step 5: Check Kibana status**

```bash
sudo systemctl status kibana
```

**Step 6: View Kibana logs**

```bash
sudo journalctl -u kibana -f
```

#### C. Kibana Enrollment & Access

Kibana must be connected to Elasticsearch before use.

**Step 1: Generate Kibana enrollment token**

Run this command on the Elasticsearch server:

```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
```

**Step 2: Check Kibana status**

```bash
sudo systemctl status kibana
```

If the service displays a setup URL and verification code, open that URL in your browser.

**Step 3: Complete setup**

- Open the Kibana setup page.
- Enter the Elasticsearch enrollment token.
- Enter the verification code if prompted.
- Complete the connection and authentication process.

**Step 4: Access Kibana**

```text
http://<EC2-PUBLIC-IP>:5601
```

For remote browser access, configure `server.host` in `/etc/kibana/kibana.yml` as appropriate and restrict access through AWS Security Groups.

**Important:**

- Elasticsearch must be running before Kibana can connect.
- Use compatible Elasticsearch and Kibana versions.
- Keep both components on the same version.
- Follow the enrollment and authentication process.
- Do not expose Elasticsearch port 9200 publicly.

### 4.11 Grafana & Kibana Visualization

**Grafana:**

- Explore dashboard creation.
- Visualize infrastructure metrics.
- Configure metrics data sources.
- Create graphs and gauge panels.
- Monitor system performance.

**Kibana:**

- Explore log monitoring.
- Search and filter log data.
- Analyze application and system logs.
- Create visualizations from indexed data.
- Build dashboards to identify error patterns.

**Note:** Kibana visualizations require log data to be ingested into Elasticsearch. Installing both services alone does not automatically collect system logs.

## 5. Linux Commands Used

### Update Packages - Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

### Update Packages - RHEL

```bash
sudo dnf update -y
```

### Install Packages - Ubuntu

```bash
sudo apt install curl wget git -y
```

### Install Packages - RHEL

```bash
sudo dnf install curl wget git -y
```

### Check System Information

```bash
cat /etc/os-release
uname -a
hostname
uptime
```

### Monitor CPU

```bash
top
```

### Check Memory

```bash
free -h
```

### Check Disk

```bash
df -h
```

### Check Running Processes

```bash
ps aux
```

### Check IP Address

```bash
ip addr
```

## 6. Datadog Agent Commands

### Install Datadog Agent

```bash
DD_API_KEY="<YOUR_DATADOG_API_KEY>" \
bash -c "$(curl -L https://install.datadoghq.com/scripts/install_script_agent7.sh)"
```

### Check Agent Status

```bash
sudo datadog-agent status
```

### Check Agent Service

```bash
sudo systemctl status datadog-agent
```

### Restart Agent

```bash
sudo systemctl restart datadog-agent
```

### Start Agent

```bash
sudo systemctl start datadog-agent
```

### Stop Agent

```bash
sudo systemctl stop datadog-agent
```

### View Agent Logs

```bash
sudo tail -f /var/log/datadog/agent.log
```

## 7. Service Management Commands

| Service | Start Command | Status Command |
|---|---|---|
| Datadog | `sudo systemctl start datadog-agent` | `sudo systemctl status datadog-agent` |
| Grafana | `sudo systemctl start grafana-server` | `sudo systemctl status grafana-server` |
| Elasticsearch | `sudo systemctl start elasticsearch` | `sudo systemctl status elasticsearch` |
| Kibana | `sudo systemctl start kibana` | `sudo systemctl status kibana` |

## 8. Application Access Ports

| Tool | Default Port |
|---|---|
| Datadog | Cloud Platform |
| Grafana | 3000 |
| Elasticsearch | 9200 |
| Kibana | 5601 |

**Security:** Configure AWS Security Groups carefully. Do not expose Elasticsearch port 9200 publicly. Restrict Grafana and Kibana access to trusted IP addresses and use secure authentication.

## 9. Monitoring Metrics

| Metric | Purpose |
|---|---|
| CPU Usage | Monitor processor utilization |
| Memory Usage | Track memory consumption |
| Disk Usage | Monitor storage utilization |
| System Load | Observe system workload |
| Infrastructure Metrics | Monitor EC2 performance |
| Alert Status | Identify infrastructure issues |
| System Logs | Search and analyze system events |

## 10. SRE Practices

- Proactive infrastructure monitoring.
- Infrastructure performance monitoring.
- Metrics visualization through dashboards.
- Centralized log analysis concepts.
- Alert configuration and email notifications.
- Incident detection and troubleshooting.
- Root cause analysis.
- System reliability improvement.

## 11. Key Learning Outcomes

- AWS EC2 infrastructure monitoring.
- Datadog Agent installation and configuration.
- Infrastructure metrics collection.
- Datadog dashboards and email alerts.
- Grafana installation and visualization concepts.
- Elasticsearch and Kibana installation.
- Log search and visualization concepts.
- Ubuntu and RHEL Linux administration.
- Linux service management.
- Incident detection and troubleshooting.
- SRE and observability fundamentals.

## 12. Conclusion

This project focuses on Site Reliability Engineering and infrastructure observability using AWS and monitoring technologies.

It covers infrastructure monitoring, dashboard visualization, alert configuration, and email notifications using Datadog, along with Grafana visualization and Elasticsearch-Kibana installation concepts.

The project helps build practical knowledge of cloud monitoring, Linux server administration, metrics visualization, log analysis, observability, and incident management.

