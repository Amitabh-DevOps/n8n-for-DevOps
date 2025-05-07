# **Enhanced Comprehensive Guide to n8n: From Foundational Concepts to Advanced Usage**

This guide provides a complete reference for developers, DevOps engineers, automation professionals, and system architects, covering n8n’s foundational concepts, advanced features, security best practices, and DevOps-specific applications, including the latest updates from 2025.

---

## **1. Introduction to n8n**

### **What is n8n?**

n8n is an open-source workflow automation tool designed to connect applications and services through APIs, enabling users to automate tasks with minimal coding. Its visual workflow builder allows users to create complex automation sequences by linking nodes, which represent triggers or actions. n8n supports both cloud-based and self-hosted deployments, making it versatile for various use cases.

### **History and Motivation**

Founded in 2019 by Jan Oberhauser, n8n emerged from his frustration with existing automation tools during his work in film production. These tools were often expensive, inflexible, or lacked scalability and self-hosting options. Oberhauser initially developed n8n for personal use, later transforming it into a service for others. The name "n8n" (pronounced "n-eight-n") derives from "nodemation," reflecting its node-based architecture. The platform has since grown, raising $12 million in Series A funding in 2021 and €55 million in Series B in 2025, with over 200,000 active users.

### **Key Principles**

- **Open Source (Fair-Code):** n8n operates under a fair-code license, providing source code access while allowing monetization for sustainability.  

- **Workflow Automation:** Simplifies connecting apps and automating tasks, reducing manual effort.  

- **Flexibility:** Supports custom nodes and integrations, enabling tailored solutions.  

- **Privacy-Focused:** Self-hosting ensures data control and compliance with privacy regulations.  


### **Target Users and Use Cases**

n8n targets developers, DevOps engineers, automation professionals, and system architects. Use cases include:

- Data synchronization between apps (e.g., CRM and email platforms). 
 
- Automated notifications (e.g., Slack alerts for system events).  

- CI/CD pipeline automation.  

- Business process automation in industries like eCommerce and finance.  

---

## **Comparison with Other Automation Tools**

| Feature | n8n | Zapier | Make (Integromat) |
|--------|-----|--------|-------------------|
| License | Fair-code (source-available) | Proprietary | Proprietary |
| Self-Hosting | Yes | No | Limited |
| Custom Nodes | Yes | Limited (via webhooks) | Limited |
| Integrations | 400+ | 5,000+ | 1,500+ |
| Pricing | Free (self-hosted), paid cloud | Subscription-based | Subscription-based |
| Flexibility | High (custom code, APIs) | Moderate | Moderate |

n8n’s open-source nature and self-hosting make it ideal for users needing control and customization, while Zapier and Make excel in ease of use and extensive integrations for non-technical users.

---

## **2. Core Concepts and Architecture**

### **Workflows**

A workflow is a sequence of connected nodes that automate a specific task. Workflows can be triggered manually or automatically based on events, such as a new email or a scheduled time.

### **Nodes**

Nodes are the building blocks of workflows, categorized as:

- **Trigger Nodes:** Initiate workflows based on events (e.g., Webhook, Cron).  

- **Action Nodes:** Perform tasks like API calls, data manipulation, or integrations.  

### **Node Types and Categories**

- **Core Nodes:** Provide essential functionality (e.g., HTTP Request, IF).  

- **Community Nodes:** Custom nodes built by the community, installable via npm.  

- **Integration Nodes:** Connect to services like Slack, Google Sheets, or GitHub.  

### **Execution Modes**

n8n supports three execution modes for managing workflow processes:

| Mode | Description | Pros | Cons |
|------|-------------|------|------|
| Main | All executions run in a single process | Resource-efficient, low latency | Risk of bottlenecks |
| Own | Each execution runs in a new process | Better stability, isolation | Higher resource usage, latency |
| Queue | Uses a main instance and workers with Redis for scalability | Scalable, load-balanced | Requires Redis, complex setup |

Queue mode is ideal for large-scale deployments, using Redis to manage execution queues.

### **Expressions, Variables, and Data Handling**

n8n uses expressions (e.g., `{{ $json.key }}`) to reference data between nodes. Data is structured as an array of objects, with each object containing a `json` key for data and optionally a `binary` key for files.

Example:
```json
[
  {
    "json": { "name": "Alice", "age": 30 },
    "binary": { "data": "<buffer>" }
  }
]
```

Binary data is managed using nodes like **Read/Write Files from Disk**.

### **Environment and Configuration Architecture**

n8n’s architecture includes a database (SQLite by default, supports PostgreSQL), a message broker (Redis for queue mode), and environment variables for configuration. Self-hosted instances can customize storage paths and execution settings.

---

## **3. Nodes & Workflow Design**

### **Essential Nodes**

| Node | Purpose |
|------|---------|
| HTTP Request | Makes API calls to external services |
| Function | Executes custom JavaScript for data manipulation |
| IF | Implements conditional logic to branch workflows |
| Switch | Routes data based on multiple conditions |
| Set | Modifies or sets data values |
| Merge | Combines data from multiple branches |
| Wait | Pauses execution for a specified duration |
| Webhook | Receives incoming webhooks to trigger workflows |

**Function vs. Code Node:** The Function node is simpler, ideal for quick JavaScript tasks, while the Code node supports both JavaScript and Python, offering advanced features like external module imports (self-hosted only).

### **Built-in Integrations**

n8n supports over 400 integrations, including Google Sheets, Slack, Trello, GitHub, and OpenAI. These nodes simplify tasks like fetching data or sending messages.

### **Handling JSON and Binary Data**

- **JSON:** Processed as arrays of objects, accessible via expressions.  

- **Binary Data:** Handled by nodes like Convert to File or Extract From File, stored in memory or on disk based on configuration.  

### **Managing Credentials**

Credentials for integrations (e.g., API keys, OAuth2) are stored encrypted in the database. Nodes request credentials securely, ensuring safe authentication.

---

## **4. Building and Testing Workflows**

### **Creating Workflows**

Workflows are built in n8n’s visual editor, where users drag and drop nodes, configure parameters, and connect them to define data flow. The editor supports real-time previews.

### **Trigger Types**

- **Polling:** Checks for events at intervals (e.g., Cron node for schedules).  

- **Webhook-based:** Responds to real-time events via HTTP requests.  

### **Testing and Monitoring**

- **Live Testing:** Run workflows manually to verify functionality.  

- **Execution Logs:** Access detailed logs to troubleshoot issues.  

- **Real-time Monitoring:** View execution status in the UI.  

### **Best Practices**

- Use descriptive node and workflow names.  

- Break complex workflows into sub-workflows.  

- Test incrementally to catch errors early.  

---

## **5. Hosting Options**

### **n8n Cloud**

- **Features:** Managed hosting, automatic updates, easy onboarding.  

- **Pricing:** Subscription-based, with limits on active workflows.  

- **Pros:** No infrastructure management, ideal for small teams.  

- **Cons:** Less control, potential costs for high usage.  

### **Self-hosted**

- **Deployment:** Use Docker, npm, or cloud platforms (AWS, GCP, Azure).  

- **Scaling:** Configure queue mode with Redis for high workloads.  |

- **Configuration:** Set environment variables for storage, webhooks, and security.  

- **User Management:** Role-based access control for teams.  

---

## **6. Advanced Concepts**

- **Creating Custom Nodes:** Develop custom nodes using JavaScript or TypeScript, publishable to npm for community use.  

- **Working with External APIs:** n8n supports OAuth2, token-based, and custom authentication for secure API interactions.  

- **Nested and Sub-workflows:** Workflows can call other workflows, promoting modularity and reusability.  

- **Error Handling:** Use try/catch in Code nodes, error workflows, or fallback nodes to manage failures.  

- **Performance Optimization:** Optimize by limiting concurrent executions, pruning old data, and using queue mode for scalability.  

---

## **7. Automation Patterns and Best Practices**

- **Scheduling and Recurring Workflows:** Use Cron nodes for scheduled tasks, such as daily reports.  

- **Event-driven Automation:** Configure Webhook nodes for real-time triggers, like new GitHub commits.  

- **ETL-like Data Processing:** Design workflows for Extract, Transform, Load (ETL) processes using nodes like Aggregate and Split Out.  
- **External Databases and Queues:** Integrate with databases (e.g., MySQL) or message brokers (e.g., RabbitMQ) for advanced workflows.  
- **Idempotency:** Ensure workflows can be re-executed safely by checking for duplicates or using unique identifiers.  

---

## **8. Monitoring, Logging, and Maintenance**

- **Built-in Logging:** n8n provides execution logs and analytics in the UI for monitoring.  

- **Self-hosted Monitoring:** Integrate with Prometheus and Grafana for advanced metrics.  

- **Backup Strategies:** Regularly back up workflows and database to prevent data loss.  

- **Workflow Versioning:** Store workflows as JSON and use Git for version control.  

---

## **9. DevOps & GitOps Integration**

- **Workflow-as-Code:** Export workflows as JSON files for version control with Git.  

- **CI/CD Pipelines:** Automate workflow deployment using Jenkins, GitHub Actions, or GitLab CI.  

- **Secrets Management:** Use environment variables or tools like HashiCorp Vault for secure credential storage.  

- **Docker and Kubernetes:** Deploy n8n in production using Docker Compose or Kubernetes for scalability.  

---

## **10. Scaling n8n in Production**

- **Queue Mode:** Uses Redis and worker instances for horizontal scaling, ideal for high workloads.  

- **Horizontal Scaling and Load Balancing:** Add worker instances and use load balancers to distribute traffic.  

- **Workflow Concurrency:** Set limits on concurrent executions to manage resources.  

- **High Availability:** Configure redundant instances to ensure uptime.  

---

## **11. Real-World Projects and Case Studies**

### **Examples**

- **[Social Media Automation:](https://n8n.io/workflows/3669-publish-image-and-video-to-multiple-social-media-x-instagram-facebook-and-more/)** Post updates to X and LinkedIn using scheduled workflows.
   

- **[Alert System:](https://n8n.io/workflows/3493-monitor-ssl-certificate-expiry-with-google-sheets-and-multi-channel-alert/)** Monitor SSL Certificate Expiry with Google Sheets and Multi-Channel Alert.  


- **[CRM Integration:](https://n8n.io/workflows/3666-real-estate-lead-generation-with-batchdata-skip-tracing-and-crm-integration/)** Sync leads between HubSpot and Salesforce.  

### **Industry-Specific Examples**

- **[eCommerce:](https://n8n.io/workflows/2851-modular-and-customizable-ai-powered-email-routing-text-classifier-for-ecommerce/)** Automate order processing and inventory updates.  


- **[SaaS:](https://n8n.io/workflows/3342-automate-sales-for-digital-products-and-saas-with-ai-gpt-4o/)** Automate Sales for Digital Products & SaaS with AI (GPT-4o).  


- **[Finance:](https://n8n.io/workflows/3617-generate-monthly-financial-reports-with-gemini-ai-sql-and-outlook/)** Generate Monthly Financial Reports with Gemini AI, SQL, and Outlook.  


- **[DevOps:](https://n8n.io/workflows/categories/devops/)** Top 47 DevOps automation workflows.  

---

## **13. Migration and Interoperability**

- **Migrating from Zapier or Make:** n8n provides guides to import workflows, leveraging its JSON format for compatibility.  


- **Syncing with Existing Tools:** Integrate with cron jobs, APIs, or message queues to enhance existing systems. 


- **Embedding n8n:** Use n8n Embed to integrate automation into custom applications.  

---

## **14. n8n for DevOps Engineers**

### **Leveraging n8n for DevOps Automation**

DevOps engineers can use n8n to automate repetitive tasks, making their work more efficient. It connects tools like Jenkins for CI/CD, AWS for cloud management, and Slack for alerts, creating seamless workflows. For example, a workflow can monitor server health and automatically send Slack notifications if something goes wrong.

### **Use Cases Specific to DevOps**

1. **Monitoring Infrastructure**: Check server status regularly and trigger actions like alerts or reports.

2. **Alerting on Failures**: Automatically create GitHub issues or send Slack messages when systems fail.

3. **Managing Deployments**: Trigger deployments or rollbacks in tools like GitHub Actions or Azure DevOps.

4. **Automating Scripts**: Run Ansible or Terraform scripts from workflows to manage infrastructure.

5. **Incident Management**: Create tickets in systems like Azure DevOps when alerts are raised, streamlining response.

6. **Server Health Checks and Log Retrieval**: Schedule regular health checks and retrieve logs from systems like Datadog or Prometheus.

7. **Visualizing Pipeline Stages**: Create dashboards to monitor pipeline stages or track incidents, integrating with Grafana.


### Examples of DevOps-Focused Workflows

- **Automate Docker Container Updates**: Updates Docker containers with Telegram approval, ensuring controlled updates ([n8n Workflow](https://n8n.io/workflows/3386-automate-docker-container-updates-with-telegram-approval-system/)).

- **AI Linux System Administrator**: Manages Linux VPS instances, automating tasks like package updates ([n8n Workflow](https://n8n.io/workflows/3020-ai-linux-system-administrator-for-managing-linux-vps-instance/)).

- **Send Email for Upgradable Packages**: Sends emails when servers have upgradable packages ([n8n Workflow](https://n8n.io/workflows/2925-send-email-if-server-has-upgradable-packages/)).

- **Proxmox AI Agent**: Integrates Proxmox with AI for automated server management ([n8n Workflow](https://n8n.io/workflows/2749-proxmox-ai-agent-with-n8n-and-generative-ai-integration/)).

- **Automate GitLab Merge Requests Using APIs with n8n**: Automate GitLab Merge Requests Using APIs with n8n ([n8n Workflow](https://n8n.io/workflows/2858-automate-gitlab-merge-requests-using-apis-with-n8n/)).


### Integrations with DevOps Tools
| **Tool**          | **Integration Details**                                                                 | **Use Case**                                      |
|-------------------|---------------------------------------------------------------------------------------|--------------------------------------------------|
| Jenkins           | Supports listing builds, managing instances, creating jobs                             | Trigger builds, monitor CI/CD pipelines          |
| GitHub            | Automate repository management, issue tracking, pull requests                          | Manage code repositories, automate issue creation |
| Docker Hub        | Integrate for container image management and updates                                   | Automate container deployments                   |
| AWS               | Nodes for S3, DynamoDB, Lambda, etc., for cloud resource management                   | Manage cloud infrastructure, automate deployments |
| Azure             | Supports Azure DevOps, Blob Storage, and other services                               | Automate incident management, cloud operations   |
| GCP               | Integrate for Google Cloud services like Compute Engine, Cloud Storage                | Manage GCP resources, automate workflows         |
| Prometheus        | Monitor metrics and trigger alerts  | Enhance monitoring and alerting systems          |
| PagerDuty         | Automate incident response and notifications                                          | Streamline incident management                   |
| Datadog           | Retrieve and analyze logs, monitor system performance                                 | Automate log processing and alerting             |

### Best Practices for Infrastructure-as-Code and Automation-as-a-Service
- **Self-Hosting**: Host n8n on your infrastructure for control, using Docker or Kubernetes for scalability.
- **Queue Mode**: Use queue mode with Redis for handling high volumes of workflows.
- **Integration**: Connect with existing monitoring, logging, and CI/CD systems for seamless automation.
- **Community Resources**: Utilize the [n8n Community](https://community.n8n.io/) for support and templates.
- **Versioning and Backup**: Store workflows as JSON in Git and back up regularly to prevent data loss.

---

### 15. Latest Features and Updates in n8n (2025)
n8n continues to evolve, with frequent updates bringing new features, improvements, and bug fixes. Some notable updates in 2025 include:

- **Version 1.90.0 (April 2025):** 

  - Added **scopes to API keys** for finer-grained access control.
  - Support for **signed URLs** for binary data to improve security and efficiency.
  - **Drag-and-drop folder support** for a more organized file structure.
  - **Supabase node schema support**, expanding integrations with modern databases.

- **Version 1.89.0:** 

  - Introduced **nested folder search** to improve workflow management.
  - Updated **HTTP Request tool** for better handling of external APIs.
  - Enhanced **AI Transform node visibility**, improving debugging and monitoring for AI-related workflows.

- **Version 1.88.0:**

  - Added **Azure Cosmos DB** and **Milvus Vector Store** nodes, expanding database and machine learning operations.
  
- **Version 1.87.0:**

  - **Google Vertex embeddings** support for AI/ML workflows.
  - Introduced an **Insights dashboard** for better analytics and workflow monitoring.

These updates enhance n8n's capabilities, making it even more powerful for DevOps, AI integrations, and large-scale automation workflows.

---

### 16. Security Best Practices for n8n
Securing your n8n instance is crucial to protect sensitive data and ensure reliable operations. The following are best practices to follow:

- **Conduct Regular Security Audits:** Regularly review workflows, credentials, and logs to identify vulnerabilities.
  
- **Use SSL/TLS:** Encrypt data in transit by setting up HTTPS. Utilize services like **Let’s Encrypt** for free SSL certificates to secure your instance.
  
- **Environment Variables:** Store sensitive information such as API keys and credentials in environment variables, and ensure they are not hardcoded in workflows.
  
- **Role-Based Access Control (RBAC):** Implement RBAC for fine-grained user permissions when using self-hosted instances, especially for teams.
  
- **Backup and Recovery:** Regularly back up workflows, credentials, and databases to prevent data loss in case of failure or attack.
  
- **Secure External APIs:** When interacting with third-party services, ensure all communication is secure, and credentials are stored safely.

By following these practices, you can minimize potential security risks and ensure that your workflows remain safe and compliant.

---

### 17. High Availability and Disaster Recovery
To ensure that n8n is highly available and resilient, consider the following approaches for disaster recovery and uptime:

- **Redundant Instances:** Deploy multiple instances of n8n across different servers or regions to ensure high availability.
  
- **Load Balancing:** Use load balancers to distribute traffic evenly across instances, improving scalability and reliability.
  
- **Database Replication:** Use database replication (e.g., PostgreSQL clustering) to ensure data availability in case of a database failure.
  
- **Health Checks:** Set up regular health checks and automated restarts to prevent downtime in case of failures.

By setting up high availability and disaster recovery processes, you can ensure that your n8n instance will continue to operate smoothly under high loads and after failures.

---

### 18. Performance Tuning and Optimization
To optimize n8n for better performance, especially in large-scale deployments, consider these tips:

- **Queue Mode for Scalability:** Use Redis-backed queue mode for large workflows, ensuring efficient resource management and scalability.
  
- **Limit Concurrent Executions:** Set a limit on concurrent executions of workflows to avoid resource exhaustion and throttling.
  
- **Optimize Node Configurations:** Minimize resource-intensive nodes like HTTP Request by adjusting their configuration, such as using batch requests or caching responses.
  
- **Use Caching:** Implement caching strategies for frequently accessed data to reduce unnecessary API calls and processing.
  
- **Database Indexing:** If using SQL databases, ensure proper indexing on commonly queried fields to improve data retrieval speeds.

Regular performance tuning can help maintain the responsiveness and reliability of your workflows, especially as your usage scales.

---

## 19. AI and Machine Learning Integrations
n8n’s integration with AI services enhances automation capabilities:

- **Google Vertex Embeddings**: Use for AI-driven data analysis in DevOps workflows ([n8n Releases](https://github.com/n8n-io/n8n/releases)).

- **OpenAI Nodes**: Automate tasks like generating text or summarizing data.

- **Example Workflow**: Analyze server logs with AI to detect anomalies and trigger alerts.

---

## 20. Monitoring and Alerting Workflows for DevOps
n8n integrates with Prometheus and Grafana for robust monitoring:

- **Enable Metrics**: Set `N8N_METRICS=true` to enable the `/metrics` endpoint ([n8n Prometheus](https://docs.n8n.io/hosting/configuration/configuration-examples/prometheus/)).

- **Queue Metrics**: Use `N8N_METRICS_INCLUDE_QUEUE_METRICS=true` for detailed queue insights.

- **Grafana Dashboards**: Import Node.js dashboards (e.g., ID 11159) for visualization ([Grafana Dashboards](https://grafana.com/grafana/dashboards/11159)).

- **Example Workflow**: Monitor CPU usage via Prometheus, trigger PagerDuty alerts if thresholds are exceeded, and log incidents in Azure DevOps.

---

## 21. Containerization and Orchestration for DevOps
Deploy n8n in containerized environments for scalability:

- **Docker**: Use the official `n8n-io/n8n` image with Docker Compose for simple setups.

- **Kubernetes**: Deploy with Helm charts, configuring ingress for external access ([n8n Kubernetes](https://gist.github.com/bacarini/fa12ed61ca8fadfd7f0121cca3fb5fa1)).

- **Example Configuration**:
  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: n8n-ingress
  spec:
    ingressClassName: nginx
    tls:
    - hosts:
      - n8n.example.com
    rules:
    - host: n8n.example.com
      http:
        paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: n8n-service
              port:
                number: 5678
  ```

--- 

### 22. Troubleshooting n8n Workflows
Effective troubleshooting is key to maintaining smooth automation processes. Here are some strategies for diagnosing and resolving issues in n8n workflows:

- **Check Execution Logs:** View detailed execution logs for each node to trace errors and identify bottlenecks.
  
- **Use the Debug Mode:** Enable debug mode for workflows to get detailed outputs during execution and identify where issues occur.
  
- **Check Node Configurations:** Ensure that all nodes are correctly configured, especially those dealing with external APIs, credentials, and data formats.
  
- **Monitor Workflow Performance:** Use the real-time monitoring tools to spot performance issues like long execution times or resource bottlenecks.

By following these steps, you can quickly pinpoint issues and maintain smooth workflow operations.

---

### 23. n8n Integrations with Other Tools and Services
n8n integrates with numerous tools, services, and APIs, enabling users to automate complex tasks. Some key integrations include:

- **Version Control Systems:** Integrate with GitHub, GitLab, and Bitbucket for CI/CD automation and code management.
  
- **Cloud Platforms:** Automate workflows across cloud services such as AWS, Azure, GCP, and DigitalOcean, managing resources like EC2, S3, and Lambda functions.
  
- **Messaging Platforms:** Send and receive messages through Slack, Telegram, Microsoft Teams, and others to automate notifications and communication.

- **DevOps Tools:** n8n integrates with Jenkins, Docker, Terraform, and Ansible, allowing DevOps engineers to automate CI/CD pipelines, infrastructure provisioning, and more.
  
- **Monitoring Tools:** Integrate with Datadog, Prometheus, and Grafana to automate monitoring, alerting, and log processing.

These integrations expand n8n's functionality, allowing users to connect various systems and automate end-to-end workflows seamlessly.

---

### 24. Community and Ecosystem
n8n has a thriving community that actively contributes to its ecosystem. Here's how you can get involved and make the most of community resources:

- **n8n Forum:** Join the n8n Community Forum to ask questions, share workflows, and get advice from other users.
  
- **GitHub:** Contribute to the n8n repository on GitHub by reporting issues, creating pull requests, and developing new features or nodes.
  
- **Marketplace:** Browse the n8n Marketplace to find custom nodes, templates, and integrations that enhance the platform's functionality.

- **Events and Meetups:** Participate in n8n-hosted events or meetups to connect with other users and learn more about the platform.

By engaging with the community, you can share your knowledge, gain new insights, and stay up-to-date with the latest developments in the n8n ecosystem.

---

## Key Citations

- [n8n Official Documentation](https://docs.n8n.io/) – Comprehensive resource for n8n workflows, nodes, hosting, and integrations.


- [n8n Official Documentation: Privacy and Security](https://docs.n8n.io/privacy-security/) – Details on n8n’s privacy policies and security measures.


- [n8n Official Documentation: Securing n8n](https://docs.n8n.io/hosting/securing/overview/) – Guidelines for securing n8n instances, including SSL/TLS and user management.


- [n8n Official Documentation: Community Nodes Risks](https://docs.n8n.io/integrations/community-nodes/risks/) – Information on risks associated with community-developed nodes.


- [n8n Official Documentation: Prometheus Metrics](https://docs.n8n.io/hosting/configuration/configuration-examples/prometheus/) – Configuration details for integrating n8n with Prometheus for monitoring.


- [n8n GitHub Repository](https://github.com/n8n-io/n8n) – Source code, issue tracking, and contributions for n8n.


- [n8n GitHub Releases](https://github.com/n8n-io/n8n/releases) – Changelog and release notes for n8n updates, including 2025 features.


- [n8n Community Forum](https://community.n8n.io/) – Platform for user support, workflow sharing, and community discussions.


- [n8n Community: Prometheus Metrics Integration](https://community.n8n.io/t/prometheus-metrics-integration/1641) – Community discussion on integrating Prometheus with n8n.


- [n8n Blog](https://blog.n8n.io/) – Official blog with announcements, case studies, and updates.


- [n8n Blog: Series A Funding Announcement](https://blog.n8n.io/series-a-announcement/) – Details on n8n’s $12 million Series A funding in 2021.


- [n8n Blog: Series B Funding Announcement](https://blog.n8n.io/series-b/) – Announcement of n8n’s €55 million Series B funding in 2025.


- [n8n Pricing and Plans](https://n8n.io/pricing/) – Information on n8n Cloud pricing and subscription plans.


- [n8n Community Workflows for DevOps](https://n8n.io/workflows/categories/devops/) – Collection of DevOps-focused workflow templates.


- [n8n Workflow: Automate Docker Container Updates](https://n8n.io/workflows/3386-automate-docker-container-updates-with-telegram-approval-system/) – Workflow for automating Docker updates with Telegram approval.


- [n8n Workflow: AI Linux System Administrator](https://n8n.io/workflows/3020-ai-linux-system-administrator-for-managing-linux-vps-instance/) – AI-driven workflow for managing Linux VPS instances.


- [n8n Workflow: Send Email for Upgradable Packages](https://n8n.io/workflows/2925-send-email-if-server-has-upgradable-packages/) – Workflow for sending email notifications about server package updates.


- [n8n Workflow: Proxmox AI Agent](https://n8n.io/workflows/2749-proxmox-ai-agent-with-n8n-and-generative-ai-integration/) – Workflow integrating Proxmox with AI for server management.


- [n8n Workflow: Azure DevOps Work Items](https://n8n.io/workflows/2500-create-an-automated-workitemincidentbuguserstory-in-azure-devops/) – Workflow for creating incidents or bugs in Azure DevOps.


- [n8n Jenkins Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.jenkins/) – Documentation for Jenkins integration in n8n.


- [n8n GitHub Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.github/) – Documentation for GitHub integration in n8n.


- [n8n AWS Nodes](https://docs.n8n.io/integrations/builtin/credentials/aws/) – Documentation for AWS service integrations in n8n.


- [n8n Azure Nodes](https://docs.n8n.io/hosting/installation/server-setups/azure/) – Documentation for Azure service integrations in n8n.


- [n8n PagerDuty Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.pagerduty/) – Documentation for PagerDuty integration in n8n.


- [n8n Datadog Node](https://docs.n8n.io/integrations/builtin/credentials/datadog/) – Documentation for Datadog integration in n8n.


- [n8n Grafana Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.grafana/) – Documentation for Grafana integration in n8n.


- [n8n HTTP Request Node](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.httprequest/) – Documentation for making custom API calls in n8n.


- [n8n Hosting Documentation](https://docs.n8n.io/hosting/) – Guide for self-hosting n8n using Docker, npm, or cloud platforms.


- [n8n Queue Mode](https://docs.n8n.io/hosting/scaling/queue-mode/) – Details on configuring queue mode for scalable deployments.


- [n8n Multi-Instance Setup on AWS EKS](https://github.com/n8n-io/n8n-eks-cluster) – GitHub repository for deploying n8n on AWS EKS.


- [n8n Kubernetes Configuration Gist](https://gist.github.com/bacarini/fa12ed61ca8fadfd7f0121cca3fb5fa1) – Kubernetes configuration example for n8n deployment.


- [TechCrunch: n8n Seed Funding Announcement](https://techcrunch.com/2020/03/13/n8n-a-fair-code-workflow-automation-platform-raises-seed-from-sequoia-as-vc-firm-steps-up-in-europe/) – Article on n8n’s seed funding round.


- [TechCrunch: n8n Series A Funding Announcement](https://techcrunch.com/2021/04/26/n8n-raises-12m-for-its-fair-code-approach-to-low-code-workflow-automation/) – Article on n8n’s Series A funding round.


- [Cybernews: Comprehensive n8n Review 2025](https://cybernews.com/ai-tools/n8n-review/) – Review of n8n’s features and capabilities in 2025.


- [Fair-Code License Explanation](https://faircode.io) – Overview of n8n’s fair-code licensing model.


- [Streamlining DevOps Support with n8n](https://medium.com/@aiyajk/streamlining-devops-support-how-i-automated-ticket-distribution-for-our-team-using-n8n-and-64e91f45868c) – Medium article on automating DevOps ticket distribution with n8n.


- [Complete DevOps Solution Using n8n](https://old.reddit.com/r/n8n/comments/1ixres4/complete_devops_solution/) – Reddit post discussing a comprehensive DevOps solution with n8n.


- [n8n Security Best Practices by Mathias Michel](https://mathias.rocks/blog/2025-01-20-n8n-security-best-practices) – Blog post on securing n8n instances.


- [Grafana Node.js Dashboard](https://grafana.com/grafana/dashboards/11159) – Node.js dashboard for Grafana integration with n8n.


- [Observability on n8n by ANDREFFS](https://www.andreffs.com/blog/observability-on-n8n/) – Blog post on implementing observability in n8n with Prometheus and Grafana.

