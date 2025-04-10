# Challenge-Based Whiteboard Design Session: AI Hub and Azure Landing Zones

## Overview

In this session, attendees will design a secure, scalable Azure architecture to support Contoso Health's AI adoption journey. The architecture must address current challenges, fulfill strategic requirements, and enable scalable growth.

## Step 1: Review the [Customer Story](./contoso-health-customer-story.md)

**Outcome**: Analyze Contoso Health's needs.

Timeframe: 15 minutes

Directions: As a team, review the customer story and discuss the following key questions.

### Key Questions

1. **Why does the customer want to use Azure for AI, and what are the customer's use cases for AI?**
   - Consider Contoso Health's current challenges, strategic goals, and how Azure AI technologies could address their specific healthcare scenarios.

2. **What are the customer's business objectives?**
   - Analyze Contoso Health's short-term and long-term objectives. How can a well-architected cloud foundation support these objectives?

3. **How will we measure success?**
   - Identify key performance indicators (KPIs) and metrics that would indicate successful implementation of the proposed architecture and AI adoption.

---

## Step 2: Define Business Requirements

Timeframe: 30 minutes

Based on the customer story, define business requirements aligned with Contoso Health's strategic vision.

### Business Requirements

1. **Security and Compliance**
   - Protect patient and operational data across all environments.
   - Comply with healthcare regulations including HIPAA and local healthcare compliance standards.
   - Implement a defense-in-depth security approach.
   - Apply consistent security policies across all environments.
   - Enable comprehensive security monitoring and threat detection.

2. **Governance and Management**
   - Establish centralized management of AI services.
   - Implement cost management and optimization strategies.
   - Standardize resource deployment and configuration.
   - Enable consistent policy enforcement.
   - Provide visibility into resource usage and performance.

3. **Connectivity and Integration**
   - Enable secure communication between on-premises healthcare facilities and Azure environments.
   - Implement network segmentation and traffic filtering.
   - Ensure high availability and disaster recovery capabilities.
   - Design for optimal latency and bandwidth for AI workloads.
   - Support hybrid identity management.

4. **AI Service Management**
   - Centralize control of AI model deployment and versioning.
   - Implement monitoring and observability for AI services.
   - Enable secure access to AI services exclusively through a centralized gateway.
   - Support MLOps practices for AI lifecycle management.
   - Ensure ethical AI implementation with appropriate oversight.

5. **AI Model Governance and Control**
   - Implement centralized governance to control which AI models can be deployed and used.
   - Establish an approval workflow ensuring only validated, compliant, and ethically approved AI models are accessible.
   - Provide auditing capabilities for AI model usage and deployment.

6. **Data Security and Privacy**
   - Ensure patient data privacy and confidentiality at all times.
   - Maintain strict adherence to data residency and sovereignty requirements.
   - Implement data encryption at rest and in transit across all environments.
   - Ensure data remains within organizational control at all times.

7. **Operational Efficiency**
   - Automate deployment and management processes to reduce operational overhead.
   - Enable self-service capabilities for internal teams while maintaining strict governance and compliance.
   - Provide comprehensive monitoring and alerting to proactively identify and resolve issues.

8. **Cost Optimization**
   - Implement cost management strategies to optimize cloud spending.
   - Provide detailed visibility into resource consumption and cost allocation across departments and workloads.
   - Establish budgets and alerts to prevent unexpected expenses.

9. **Scalability**
   - Ensure the architecture can scale efficiently to accommodate future growth in patient data, AI workloads, and additional healthcare facilities.
   - Design for elasticity and dynamic resource allocation.

---

## Step 3: Technical Challenges

Timeframe: 2 hours

Address the following technical challenges by designing appropriate Azure architectures.

### Challenge 1: Design Core Cloud Environment

Design a secure and scalable cloud environment addressing:

- Identity and access management strategy.
- Management and monitoring approach.
- Network topology and connectivity model.
- Security controls and policy enforcement.
- Subscription design and resource organization.

**Questions to consider:**
- What approach will you use to implement hybrid identity management?
- What network topology will best support Contoso Health's operations?
- How will you implement security controls consistently?
- What monitoring and management tools should be implemented?
- How will you ensure scalability to support future growth?

### Challenge 2: Design Workload Hosting Environment

Design secure environments optimized for:

- Containerized applications.
- Data analytics platforms.
- Web applications.
- AI/ML workloads.
- DevOps and CI/CD pipelines.

**Questions to consider:**
- How will you organize resources within these environments?
- What patterns will you use for workload segmentation?
- How will you enable DevOps practices across the environments?
- What data platforms will you implement to support different AI workloads?
- How will you ensure scalability and elasticity for future growth?

### Challenge 3: Design Centralized AI Management Environment

Create a centralized architecture for AI services with:

- Centralized gateway for secure access to AI services.
- Security controls for AI services.
- Private connectivity for all services.
- Governance model for AI resources.
- Integration with other environments.
- Centralized AI model governance and approval workflows.

**Questions to consider:**
- How will you implement a secure perimeter around AI services?
- What role will a centralized gateway play in securing and governing AI services?
- How will you implement private connectivity for all required services?
- What Azure AI services will be part of this centralized environment?
- How will you implement monitoring and observability for AI services?
- How will you implement centralized governance and approval workflows for AI models?
- How will you ensure patient data privacy and compliance with data residency requirements?

---

## Deliverables

1. High-level architecture diagram showing:
   - Core cloud environment components.
   - Workload hosting environment components.
   - Centralized AI management environment design.
   - Network connectivity between environments.
   - Security controls.

2. List of Azure services included in each environment and their roles.

3. Implementation plan outlining:
   - Phased approach for environment deployment.
   - Migration strategy for existing workloads.
   - Governance model implementation.
   - Security controls implementation.
   - AI model governance and approval workflow implementation.

4. Risk assessment and mitigation strategies.

---

## References

- [Microsoft Cloud Adoption Framework for Azure](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/)
- [Azure Cloud Adoption Framework for AI](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/scenarios/ai/)
- [Azure AI Hub Gateway Solution Accelerator](https://github.com/Azure-Samples/ai-hub-gateway-solution-accelerator/tree/main)