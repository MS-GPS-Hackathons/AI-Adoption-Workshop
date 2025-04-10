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

## Step 2: Elicit Workload Requirements (30 minutes)

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
    * Requirement: The backend logic (prompt flow) needs a defined authoring/testing process and a reliable hosting mechanism.
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

## Step 4: Present and Justify (30 minutes)

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