# Challenge-Based Whiteboard Design Session: IFS AI Foundation & Hub

**Session Goal:** Design a secure, scalable, and governed Azure foundation for Innovate Financial Services (IFS) to support their AI transformation journey, addressing their business objectives and technical challenges.

---

## Step 1: Understand the Customer (15 minutes)

**Activity:** Review the [Innovate Financial Services Customer Story](./ifs-customer-story.md).

**Outcome:** As a team, discuss and answer the following based *only* on the customer story:

1.  **Why does IFS want to use Azure for AI, and what are their primary use cases for AI?**
    * *Hint: Consider their market pressures, strategic goals, and specific financial service applications mentioned.*
2.  **What are IFS's core business objectives driving this transformation?**
    * *Hint: Focus on the desired outcomes related to fraud, customer experience, costs, and innovation speed.*
3.  **How will IFS measure the success of this initiative?**
    * *Hint: Identify the specific metrics and KPIs mentioned in the story.*

> **Note:**
> The customer story is intentionally high-level. The goal is to extract the key business drivers and objectives that will inform your design decisions. A mission statement helps you align your objectives and key results to your organization's overall business mission. To help you derive a statement for their business, consider these initial questions:

> - Why are they exploring to use Azure and AI? Where do they want to be in the future? When do they want to achieve this?
> - What are their mandatory business objectives? And what are their nice-to-have objectives?
> - What is your "definition of done"?
> - What are the key metrics that will be used to measure success?
> - What are the KPIs that will be used to measure success?

> Use the answers to these questions to help define the customer's your mission statement for their AI adoption cloud strategy. For example:

> *"IFS will leverage Azure and AI to transform their financial services, enhancing customer experience and operational efficiency while ensuring compliance and security. Their goal is to innovate rapidly, reduce costs, and deliver high-quality services that meet the evolving needs of their customers."*

---

## Step 2: Define High-Level Requirements (30 minutes)

**Activity:** Based on the customer story and the goals of building a foundation for secure AI adoption, define the key business and technical requirements. Focus on *what* needs to be achieved, not *how*.

**Outcome:** Document requirements covering the following areas:

* **Foundation & Governance:**
    * Requirement: Establish standardized environments for different workload types (e.g., core infrastructure, applications, data analytics, AI).
    * Requirement: Implement centralized management and consistent governance policies across all cloud resources.
    * Requirement: Ensure clear separation of duties and environments for platform operations vs. application development.
    * Requirement: Provide robust identity and access management integrated with existing enterprise systems.
* **Security & Compliance:**
    * Requirement: Enforce stringent security controls to meet financial industry regulations (PCI DSS, GDPR, etc.) and protect sensitive customer data.
    * Requirement: Ensure all network traffic between critical components is private and isolated from public exposure.
    * Requirement: Implement comprehensive monitoring for security threats and compliance deviations.
    * Requirement: Adhere to data sovereignty and residency requirements.
* **Connectivity:**
    * Requirement: Enable secure and reliable connectivity between on-premises data centers and Azure environments.
    * Requirement: Segment network traffic effectively between different environments and workloads.
* **AI Service Enablement:**
    * Requirement: Provide a controlled, centralized mechanism for discovering, accessing, and managing approved AI services.
    * Requirement: Ensure AI services can be accessed securely by internal applications without direct exposure to external networks.
    * Requirement: Implement governance for AI model lifecycle management (deployment, versioning, monitoring).
    * Requirement: Track and potentially charge back the usage of shared AI services to different business units.
* **Scalability & Operations:**
    * Requirement: The architecture must scale to accommodate significant growth in data volume, users, and AI workload complexity.
    * Requirement: Optimize for operational efficiency, potentially through automation and standardized deployment patterns.
    * Requirement: Achieve target levels for reliability and disaster recovery for critical systems.

---

## Step 3: Design Challenges (2 hours)

**Activity:** Address the following technical challenges by designing the Azure architecture for IFS. Create diagrams and list key Azure services.

**Challenge 1: Foundational Azure Environment (Landing Zones)**

* **Task:** Design the core Azure environment structure. How will you organize subscriptions and resources to meet IFS's requirements for governance, security, and separation? Consider identity, management, and connectivity.
* **Questions:**
    * How will you structure Azure subscriptions to separate platform functions (like networking, identity) from application workloads?
    * What core services are needed for identity management, security monitoring, and policy enforcement across the entire Azure estate?
    * How will you establish secure network connectivity between Azure and IFS's on-premises locations?
    * How will you ensure consistent application of security policies and compliance standards?

**Challenge 2: Application & Workload Environments (Landing Zones)**

* **Task:** Design the environment(s) where IFS will host its modernized applications, data platforms (like data lakes, analytics engines), and specific AI/ML workloads (excluding the central AI services hub for now).
* **Questions:**
    * What patterns will you use to provide standardized, secure environments for different types of applications (e.g., web apps, containerized services, data analytics)?
    * How will these environments securely connect to the foundational services (identity, networking, management)?
    * How will you ensure network segmentation between different applications or business units hosted in Azure?

**Challenge 3: Centralized & Secure AI Service Hub**

* **Context:** IFS requires a dedicated, secure environment (the "AI Hub") to host and manage shared Azure AI services (like Azure OpenAI, Azure AI Search) and potentially the logic that orchestrates them (e.g., prompt flows). This hub must act as the single, controlled gateway for accessing these services, enforcing strict security and governance. Private networking is mandatory for all components.
* **Core Requirements derived from baseline:**
    * **Network Isolation:** All Azure services used within this hub (AI services, data stores, compute/hosting for AI logic, access gateway) must communicate exclusively over private network connections, with no public internet exposure.
    * **Secure Access Point:** A managed, secure gateway or proxy must be implemented as the sole entry point for accessing the AI capabilities hosted within the hub. This gateway should also support security enforcement (e.g., request validation, potentially WAF).
    * **Secure Secrets Management:** API keys, connection strings, and other secrets must be stored securely and accessed by applications/services without being exposed in code or configuration files.
    * **Comprehensive Monitoring:** Detailed logging and monitoring must be in place for usage, performance, and security events across all components in the hub (gateway, AI services, compute).
    * **AI Workload Governance:** Apply specific policies to govern the resources and configurations within this AI Hub.
* **Design Questions:**
    * How will you ensure that access to the core PaaS services (like Azure OpenAI, Azure AI Search, Azure Storage if used for related data) is restricted solely to private network endpoints within the AI Hub's virtual network?
    * What specific Azure service(s) could function as the secure, managed entry point (reverse proxy/API gateway) for AI requests into the hub? How would you configure it to enhance security (e.g., firewall capabilities)?
    * Considering the strict private networking requirement, how will other IFS applications running in separate Application Landing Zones securely connect to *only* the designated entry point of this AI Hub? What networking mechanisms facilitate this private cross-environment communication?
    * How will applications and services within the AI Hub securely obtain necessary secrets (like the Azure OpenAI API key) at runtime?
    * What specific Azure services and configurations will you use to implement the required comprehensive monitoring and logging for the AI Hub components?
    * If IFS needs to host custom AI orchestration logic (like prompt flows) within this hub, what Azure compute service(s) could host this securely within the private network, and how would they integrate with the VNet?
    * Within Azure, how will internal DNS resolution be handled to ensure services correctly resolve the private IP addresses of the various components (like Azure OpenAI, the gateway, etc.) within the hub and across connected networks?
    * What Azure Policy definitions might be relevant to enforce governance specifically for the resources deployed within this AI Hub (e.g., enforcing private endpoints, specific SKUs, tagging)?

---

## Step 4: Present and Justify (30 minutes)

**Activity:** Prepare a high-level diagram of your proposed architecture, highlighting how it addresses the core requirements and challenges.

**Outcome:** Briefly present your design, explaining:
* The overall Landing Zone structure (Platform and Application).
* How the AI Hub provides secure, centralized access to AI services using private networking, referencing key components like private endpoints and the secure gateway.
* How your design meets key requirements like security, governance, scalability, and compliance.
* Key Azure services used in each major component.

---

## References

* [Microsoft Cloud Adoption Framework for Azure](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/)
* [Azure Cloud Adoption Framework - AI Scenario](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/scenarios/ai/)
* [Azure OpenAI baseline Landing Zone reference architecture](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/architecture/azure-openai-baseline-landing-zone) *(For facilitator reference - attendees should derive principles)*
* [AI Hub Gateway Solution Accelerator Concept](https://github.com/Azure-Samples/ai-hub-gateway-solution-accelerator/tree/main) *(For facilitator reference)*