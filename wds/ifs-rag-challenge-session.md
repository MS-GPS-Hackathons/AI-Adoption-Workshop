# Challenge-Based WDS: IFS Knowledge Assistant Chatbot Workload

**Session Goal:** Design the architecture for a secure, internal Retrieval-Augmented Generation (RAG) chatbot application ("IFS Knowledge Assistant") for Innovate Financial Services (IFS) employees, based on the principles of the Azure OpenAI End-to-End Chat baseline. This includes selecting hosting options, ensuring security, and planning for deployment within IFS's established Azure Landing Zone environment.

**Assumed Context:** IFS has already established an Azure Landing Zone foundation, including network connectivity, identity management, and potentially a centralized "AI Hub" providing governed access to services like Azure OpenAI and Azure AI Search via private endpoints.

---

## Step 1: Define the Workload Scenario (15 minutes)

**Scenario:**
Innovate Financial Services (IFS) wants to empower its employees by providing quick and accurate answers to questions about internal policies, procedures, and market analysis reports. They have decided to build an internal web-based chatbot, the "IFS Knowledge Assistant". This application needs to:

1.  Understand employee questions asked in natural language.
2.  Securely access a curated set of internal documents (already indexed and available via an Azure AI Search instance managed by the platform team).
3.  Use an approved Azure OpenAI model (available via the central AI Hub) to generate relevant, grounded answers based on the retrieved documents.
4.  Present the answers through a simple, intuitive web interface.
5.  Adhere strictly to IFS's security and compliance standards, running entirely within their private Azure network.
6.  Be maintainable and deployable using modern DevOps practices (including IaC).

**Activity:** Discuss the core functional components required to fulfill this scenario. What are the high-level inputs, processes, and outputs?

---

## Step 2: Elicit Workload Requirements (45 minutes)

**Activity:** Based on the scenario and the assumed context, define the specific functional and non-functional requirements for the *IFS Knowledge Assistant application itself*.

**Outcome:** Document requirements covering:

* **Functionality:**
    * Requirement: Web-based user interface for submitting questions and viewing answers.
    * Requirement: Backend logic to orchestrate the RAG process (receive query, retrieve documents from AI Search, formulate prompt, call OpenAI, process response).
    * Requirement: Utilize the centrally managed Azure AI Search index for document retrieval.
    * Requirement: Utilize the centrally managed Azure OpenAI service (via the AI Hub) for answer generation.
* **Security & Compliance:**
    * Requirement: All application components (UI, backend API/logic) must reside within IFS's private network (integrate with existing Landing Zone VNets).
    * Requirement: All communication between components and with dependent Azure services (AI Search, OpenAI via AI Hub, Key Vault) must use private network connections (e.g., Private Endpoints, VNet Integration). No public endpoints for backend components.
    * Requirement: Securely manage all application secrets (e.g., connection details, API keys if needed) using Azure Key Vault.
    * Requirement: Implement appropriate authentication and authorization for application components using Managed Identities.
* **Operations & Deployment:**
    * Requirement: Implement comprehensive logging and monitoring for application health, performance, and usage (UI, backend, AI interactions).
    * Requirement: Define an automated deployment process for both infrastructure (using IaC like Bicep/Terraform) and application code/logic.
    * Requirement: The backend logic requires a well-defined authoring and testing process and a reliable hosting mechanism.
    * Requirement: The solution should be designed for reliability (e.g., consider zone redundancy for key components) and scalability.

---

## Step 3: Design the Chat Application Architecture (2 hours)

**Activity:** Design the end-to-end architecture for the "IFS Knowledge Assistant". Create diagrams showing components, interactions, networking, and key Azure services. Address the questions below.

**Challenge Questions:**

1.  **Component Breakdown:** What are the distinct logical components of this application (e.g., frontend, backend API, orchestration logic)? What Azure services map to these components?
2.  **User Interface (UI):**
    * Which Azure service is suitable for hosting the web UI? Why?
    * How will this service be integrated into the private network?
    * How will employees access the UI securely? What handles ingress traffic and potential security filtering (like WAF)? (Consider Application Gateway).
3.  **Backend Orchestration (Prompt Flow Logic):**
    * The core logic involves receiving user input, querying AI Search, constructing a prompt, calling Azure OpenAI, and returning the response. Where would this logic (potentially developed as a Prompt Flow) be authored and tested securely within the IFS environment? (Reference the authoring pattern in the baseline).
    * **Hosting:** What are the primary Azure options for *hosting* this backend orchestration logic once developed? Compare deploying it to:
        * Azure Machine Learning Managed Online Endpoint.
        * A self-hosted container within Azure App Service.
        * (Optional: Consider Azure Container Apps or Azure Functions).
    * **Pros & Cons:** Discuss the trade-offs of each hosting option regarding management overhead, scalability, cost, network integration complexity, and deployment process.
    * **Chosen Option Design:** For your recommended hosting option:
        * How is it deployed? (e.g., container image from ACR, ML model deployment).
        * How does it integrate securely into the VNet?
        * How does it securely connect to dependencies (AI Search, OpenAI via AI Hub, Key Vault) using private networking and managed identities?
4.  **Data Flow & RAG:** Diagram the flow of a user request from the UI through the backend orchestration, showing interactions with Azure AI Search and Azure OpenAI (via the AI Hub).
5.  **Security & Identity:**
    * How does the UI authenticate/authorize calls to the backend API?
    * How does the backend API/orchestration logic authenticate to Azure AI Search, Azure OpenAI (via AI Hub), and Key Vault? Detail the use of Managed Identities.
    * How are secrets (e.g., Key Vault URI, Search endpoint) provided securely to the application components at runtime?
6.  **Networking:**
    * Illustrate how the application components (UI, Backend Hoster) reside within or integrate with the Application Landing Zone VNet.
    * Show the network paths (using private endpoints/VNet integration) for communication:
        * User -> Ingress (App Gateway) -> UI Hoster
        * UI Hoster -> Backend Hoster
        * Backend Hoster -> Key Vault
        * Backend Hoster -> AI Search (via AI Hub or direct PE)
        * Backend Hoster -> Azure OpenAI (via AI Hub PE)
    * How is DNS resolution handled for these private connections?
7.  **Monitoring & Observability:**
    * What specific metrics and logs would you capture from the UI, the backend host, and the prompt flow logic itself?
    * Which Azure Monitor components (Application Insights, Log Analytics) would you use and how would they be configured for each application component?
8.  **Deployment (IaC & CI/CD):**
    * Outline the steps needed to deploy the Azure resources for this application using IaC (e.g., Bicep).
    * How would you package and deploy the UI code?
    * How would you package (e.g., containerize if applicable) and deploy the backend prompt flow logic to your chosen hosting option? What tools are involved (`pf` CLI, `az cli`, Docker, ACR)?

---

## Step 4: Integrate the Workload in the Customer's Landing Zone Environment (1 hour)

**Activity:** Work as a team to integrate the "IFS Knowledge Assistant" workload into the customer's Azure Landing Zone environment. This step focuses on making the workload production-ready by addressing networking, security, compliance, and operational requirements.

**Outcome:** Attendees will answer key integration questions, ensuring the workload is fully operational within the Landing Zone while adhering to IFS's governance policies.

**Challenge Questions:**

1. **Networking Integration:**
    * How will you ensure that all workload components (UI, backend, dependencies) are securely integrated into the Landing Zone Virtual Network (VNet)?
    * What steps are required to configure private endpoints or VNet integration for services like Azure AI Search, Azure OpenAI (via AI Hub), and Key Vault?
    * How will you verify DNS resolution for private endpoints to ensure seamless communication between components?

2. **Identity and Access Management:**
    * How will you assign Managed Identities to application components (UI, backend) to securely access Azure resources?
    * What role-based access control (RBAC) configurations are needed to enforce least-privilege access for each component?

3. **Secrets Management:**
    * Where will you store sensitive information (e.g., connection strings, API keys) to ensure secure access at runtime?
    * How will application components retrieve secrets securely using Managed Identities?

4. **Security and Compliance:**
    * How will you validate that all communication between components and Azure services occurs over private network connections?
    * What steps will you take to ensure compliance with IFS's security policies, including encryption in transit and at rest?
    * How will you conduct a security review to identify and mitigate potential vulnerabilities?

5. **Monitoring and Observability:**
    * What Azure Monitor components (e.g., Application Insights, Log Analytics) will you use to monitor the workload?
    * What critical metrics and alerts will you define to ensure the workload operates reliably in production?

6. **Scalability and Reliability:**
    * How will you configure autoscaling for hosting services (e.g., App Service, Container Apps) to handle varying workloads?
    * What measures will you implement to ensure high availability, such as zone redundancy for critical components?

7. **Deployment Readiness:**
    * How will you validate the Infrastructure as Code (IaC) templates (e.g., Bicep/Terraform) for deploying the workload in the Landing Zone?
    * What steps will you take to test the CI/CD pipelines for seamless deployment of application updates?

---

### Deliverables:

1. **Integration Plan:** A detailed plan outlining how the workload components will be integrated into the Landing Zone.
2. **Updated Architecture Diagram:** A diagram showing the integrated workload, including networking, security, and dependencies.
3. **Validation Checklist:** A checklist summarizing the results of integration tests, including network connectivity, security compliance, and performance benchmarks.

---

### Key Considerations:

* How will you ensure that all workload components adhere to IFS's Landing Zone governance policies?
* What steps will you take to validate that the workload can scale and operate reliably in production?
* How will you confirm that monitoring and alerting configurations are in place to support ongoing operations?

---

## Step 5: Present and Justify (30 minutes)

**Activity:** Prepare a high-level diagram of your proposed workload architecture.

**Outcome:** Briefly present your design, explaining:
* The key components and chosen Azure services.
* The selected hosting option for the prompt flow backend and the rationale.
* How the application integrates securely within the Landing Zone network and interacts with the AI Hub/platform services privately.
* How security, monitoring, and deployment requirements are met.

---

## References

* *Conceptual:* [Azure OpenAI End-to-End Chat Reference Architecture (Learn)](https://learn.microsoft.com/azure/architecture/ai-ml/architecture/baseline-openai-e2e-chat)
* *Platform Context:* The previously designed IFS Landing Zone and AI Hub architecture.