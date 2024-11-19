

# **TrafficFailoverAutomation**

This project automates the failover of network traffic between routers by manipulating BGP attributes such as **Local Preference (LP)** and **AS Path Prepending**. It ensures seamless traffic failover during network maintenance, outages, or performance degradation.

---

## **Table of Contents**
- [Overview](#overview)  
- [Features](#features)  
- [Workflow](#workflow)  
- [Project Structure](#project-structure)  
- [Getting Started](#getting-started)  
  - [Prerequisites](#prerequisites)  
  - [Installation](#installation)  
- [Usage](#usage)  
- [Contributing](#contributing)  
- [License](#license)  
- [Next Steps](#next-steps)

---

## **Overview**
Network failover automation is critical to maintaining high availability and uptime. This project automates the process of redirecting traffic from **Router 1** to **Router 2** by dynamically adjusting BGP attributes. Using **Camunda BPMN workflow** for orchestration and Python scripts for network configuration and monitoring, the automation ensures traffic shifts based on router health status.

---

## **Features**
- **Automated Health Checks**: Verifies the operational status of Router 1 to determine if failover is required.
- **Dynamic BGP Attribute Adjustment**: Modifies AS Path Prepending and Local Preference values to shift traffic from Router 1 to Router 2.
- **Failover Verification**: Ensures that traffic is successfully redirected after the BGP manipulation.
- **Notifications**: Sends notifications (email/Slack) during the failover process to alert stakeholders.
- **Extensible Workflow**: Easily customizable BPMN workflow to fit different failover or network automation scenarios.

---

## **Workflow**
The **Traffic Failover Workflow** is composed of the following steps:

1. **Start Event**: The failover process is triggered manually or automatically.
2. **Health Check**: A script pings or queries Router 1 to verify its health.
   - If Router 1 is healthy, the workflow ends.
   - If Router 1 is unhealthy, proceed to failover.
3. **Decision Gateway**: 
   - If the router is healthy, the workflow terminates.
   - If the router is unhealthy, proceed to the next step.
4. **BGP Attribute Update**:
   - Adjust the **Local Preference** (LP) on Router 1 (lower LP) to reduce its preference.
   - Apply **AS Path Prepending** on Router 1 to make its path less preferable.
   - Increase the **Local Preference** and remove **AS Path Prepending** on Router 2 to make it the preferred route.
5. **Verification**: Validate the success of the failover by checking the BGP routing table to ensure traffic is redirected to Router 2.
6. **Notification**: Send an alert (email or Slack) to the designated stakeholders, notifying them of the failover process and completion.
7. **End Event**: Terminate the workflow once failover is complete and verified.

---

## **Project Structure**
```plaintext
TrafficFailoverAutomation/
│
├── workflows/
│   ├── traffic-failover.bpmn     # Camunda BPMN workflow file
│
├── scripts/
│   ├── health_check.py           # Health check logic for Router 1
│   ├── bgp_failover.py           # BGP manipulation script to trigger failover
│   ├── notification.py           # Notification logic (email/Slack)
│
├── README.md                     # Project documentation
├── requirements.txt              # Python dependencies
└── .gitignore
```

---

## **Getting Started**

### **Prerequisites**
To run this project, ensure that you have the following installed:
- **Camunda Modeler**: For designing and modifying BPMN workflows. [Camunda Modeler download](https://camunda.com/download/modeler/)
- **Camunda Engine**: For executing BPMN workflows.
- **Python 3.x**: To run the health check, BGP failover, and notification scripts.
- **Cisco/Arista Routers in Containerlab**: Set up routers for testing the BGP failover.

### **Installation**

1. **Clone the repository**:
   ```bash
   git clone https://github.com/sshukla90/TrafficFailoverAutomation.git
   cd TrafficFailoverAutomation
   ```

2. **Install Python dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Import the BPMN file**:
   - Open Camunda Modeler and import the `traffic-failover.bpmn` file from the `workflows/` folder.

4. **Configure your routers**:
   - Ensure that **Router 1** and **Router 2** are set up with BGP configurations in your containerlab environment.

---

## **Usage**

### **Starting the Workflow**

- **Manually Trigger Workflow**: Use the Camunda cockpit to manually start the failover process.
- **Automated Trigger**: You can integrate the workflow with an API for automated triggers, such as an external monitoring tool or a script that detects router health.

### **Running Python Scripts**

1. **Health Check**:
   Run this script to verify the health of Router 1:
   ```bash
   python scripts/health_check.py
   ```

2. **BGP Failover**:
   This script will manipulate the BGP attributes:
   ```bash
   python scripts/bgp_failover.py
   ```

3. **Send Notifications**:
   Notify stakeholders of the failover:
   ```bash
   python scripts/notification.py
   ```

### **Verification**

- Once the failover is completed, check the BGP route advertisements on the routers to confirm that traffic has been successfully redirected.

---

## **Contributing**

We welcome contributions! To contribute:
1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Make changes and submit a pull request with a clear description of what you’ve done.

---

## **License**

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

---

## **Next Steps**
- **Extend the Workflow**: Add more routers or failover strategies (e.g., active-active).
- **Integrate Monitoring Tools**: Implement integration with tools like **Prometheus** for automatic triggers based on router health metrics.
- **Enhance Notifications**: Include more notification channels like SMS, Teams, etc.

---

This updated **README.md** file should provide a more clear, concise, and logical flow for users and developers working with your project. Let me know if you need any further adjustments!