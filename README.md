# ☁️ AWS Auto Scaling Group (ASG) Project

## Project Overview

This project demonstrates the implementation of a highly available and self-healing architecture on Amazon Web Services (AWS) using an **Auto Scaling Group (ASG)**. The configuration ensures that the application dynamically scales its capacity based on demand, optimizing performance while minimizing operational costs.

The core functionality is driven by a simple scaling policy linked to CPU Utilization, guaranteeing that resources are added when traffic increases and removed when demand subsides.

---

## 🚀 Architecture Components

The project is built upon the following AWS services, configured as shown in the attached monitoring image:

### 1. Auto Scaling Group (ASG)
* **Name:** `ASG` (as seen in the scaling policy configuration)
* **Service Name:** `Auto Scaling Group Project`
* **Launch Template:** Uses the `Mintud Template` and `Auxlinr Template` (referring to `t3.mirco` instances) to define the instance configuration.
* **Availability Zones:** Configured across **5** Availability Zones for maximum resilience and high availability.
* **Capacity:**
    * **Desired:** `2`
    * **Min:** `2`
    * **Max:** `5`

### 2. Launch Template
The project utilizes a launch template (e.g., `l-078bfc...`) to standardize the configuration of new instances launched by the ASG, including AMI ID, instance type (`t3.mirco`), security groups, and user data scripts (for bootstrapping the web server).

### 3. Dynamic Scaling Policy
The scaling behavior is managed by a **Simple Scaling Policy** named `My-Scale-Out-Policy`.

| Configuration Item | Detail |
| :--- | :--- |
| **Policy Type** | Simple scaling |
| **Policy State** | Enabled |
| **Alarm Threshold (Scale-Out)** | CPU Utilization **Greater-than-60%** for 1 consecutive period of 300 seconds (5 minutes). |
| **Action Taken** | The ASG will add one or more instances. |
| **Cooldown Period** | 300 seconds (5 minutes) before allowing another scaling activity, preventing rapid, oscillating scaling actions. |

---

## 🛠️ Implementation Details

### High Availability
By distributing the instances across **5** Availability Zones, the application is protected from failure in a single data center.

### Elasticity
The minimum capacity of **2** ensures baseline performance, while the maximum capacity of **5** protects against unexpected cost overruns during traffic spikes. The dynamic scaling policy manages the transition between these limits.

### Monitoring and Alarms
* **Alarms (2):** Two alarms are configured to monitor the health and performance of the ASG.
* **CPU Utilization:** CPU utilization is used as the primary metric for dynamic scaling, ensuring that instances are added only when they are actually under load.

---

## 💡 Benefits of this Configuration

* **Cost Optimization:** Resources are provisioned only when needed, avoiding the cost of underutilized idle servers.
* **Improved Reliability:** The minimum capacity and multi-AZ setup ensure the application remains operational even if multiple instances or an entire Availability Zone fails.
* **Performance Assurance:** Automatic scaling ensures the application maintains optimal performance and responsiveness during periods of high user traffic.

---

## 📌 How to Deploy (Placeholder)

To deploy this project, you would typically:

1.  Create the **Launch Template** defining the `t3.mirco` instance configuration.
2.  Create the **Auto Scaling Group** referencing the Launch Template and specifying the Min (2), Desired (2), and Max (5) capacities, and selecting the 5 target Availability Zones.
3.  Configure the **CloudWatch Alarm** to trigger when the average CPU Utilization of the ASG exceeds 60% for 5 minutes.
4.  Attach the alarm to the ASG as the **Scale-Out Policy**.
