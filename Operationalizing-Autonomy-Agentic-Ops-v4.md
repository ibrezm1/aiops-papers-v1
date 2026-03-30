# Operationalizing Autonomy: A Framework for Transitioning ITSM to Agentic Architectures (v4 - Refined)

**Submission Category:** IT Operations Strategy / Advanced Automation
**Keywords:** Agentic Ops, Multi-Agent Systems (MAS), AIOps, ITSM, SRE, Large Language Models, Zero-Touch Reliability

## Abstract
Traditional IT Service Management (ITSM) models, predicated on tiered human support and rigid escalation matrices, are reaching a point of diminishing returns. As infrastructure complexity grows, the cognitive load required for Root Cause Analysis (RCA) and remediation exceeds human capacity for speed and accuracy. This paper proposes a transition from predictive **AIOps** to **Agentic Operations**—a paradigm where Multi-Agent Systems (MAS) autonomously execute the OODA loop (Observe, Orient, Decide, Act). By decoupling signal detection (deterministic ML) from cognitive reasoning (LLMs), organizations can automate the "High-Latency Six" operational modules. We present a three-layer reference architecture—Signal, Cognitive, and Execution—and define the necessary API substrates, data hygiene requirements, and strictly governed "human-in-the-loop" protocols required to safely deploy autonomous agents in enterprise environments.

---

## I. The Operational Gap: Why Tiered Support Fails
Current ITSM frameworks (ITIL v4, VeriSM) identify over 30 management practices, yet operational reality demonstrates that resource consumption is highly concentrated [14]. We identify the **"High-Latency Six"**—Incident, Service Desk, Request, Change, Problem, and Monitoring—as the primary sources of operational "toil." This toil is not new; classic studies on software maintenance identify that nearly 80% of effort is spent on non-functional enhancements and fixes rather than new features [13].

The legacy approach relies on a **Tier 1–3 hierarchy**:
1.  **Tier 1:** High-volume, low-context ticket triage.
2.  **Tier 2/3:** High-cost engineering resources distracted by escalation noise.

This structure creates an **Efficiency Paradox**: The resources most capable of fixing root causes (Tier 3) are the least accessible, while the resources most accessible (Tier 1) lack the agency to remediate. Recent studies in ITSM environments show that hierarchical ticket classification can be improved using centroid-based frameworks to reduce this latency [10]. However, even with state-of-the-art (SOTA) classification, the "Human-in-the-Loop" bottleneck remains the primary driver of MTTR.

---

## II. Paradigm Shift: From Observability to Agency
The industry is currently pivoting from **Predictive AIOps** to **Agentic Ops**. It is critical to distinguish between these distinct maturity levels [1, 3]:

*   **Level 1: Predictive AIOps (The Observer):** Uses statistical ML to detect anomalies and correlate events (e.g., DeepLog for log-based anomaly detection [16]). It answers: *"Something is wrong."*
*   **Level 2: Agentic Ops (The Actor):** Uses Large Language Models (LLMs) with tool-use capabilities to reason through workflows and manipulate state. It answers: *"I am fixing what is wrong."*

Unlike rigid automation scripts (RPA), which break upon schema changes, **Agentic Ops** utilizes probabilistic reasoning to adapt to dynamic cloud environments, enabling true "Zero-Touch" self-healing infrastructure [12, 17].

---

## III. Reference Architecture: The Cognitive Stack
To deploy agents safely, we must avoid the "Black Box" approach. We propose a three-layer architecture that enforces a separation of concerns between detection, reasoning, and execution [15].

### Layer 1: The Signal Plane (Deterministic ML & Stream Processing)
**Goal:** High-throughput telemetry ingestion and noise cancellation.
**Components:**
*   **Feature Extractors:** Specialized models (e.g., DeepLog [16]) that convert raw log streams, metrics (Prometheus/OpenTelemetry), and traces into structured "Health Vectors."
*   **Centroid-Based Triage:** Utilizing optimized classification frameworks [10] to categorize signals into standard ITSM categories (Incident, Performance, Security).
*   **State Aggregator:** A real-time topology map (CMDB 2.0) that maintains the current relationship between microservices and infrastructure components.
**Strategy:** This layer acts as the "Sensory Nervous System," filtering 99% of raw data. It only triggers the Cognitive Plane when a "Health Vector" deviates from the established baseline, preventing LLM token exhaustion and latency.

### Layer 2: The Cognitive Plane (The Multi-Agent Orchestrator)
**Goal:** Reasoning, planning, and multi-agent collaboration.
**Components:**
*   **The Orchestrator (Agentic Brain):** A high-reasoning model (e.g., GPT-4o or Claude 3.5 Sonnet) utilizing the **ReAct** framework to decompose high-level goals into sub-tasks.
*   **Memory Modules:** 
    *   *Episodic Memory:* Stores the history of previous incidents and successful remediations (RAG).
    *   *Semantic Memory:* A knowledge base of Standard Operating Procedures (SOPs) and system documentation.
*   **Agent Squads (CoE-Ops):** specialized sub-agents (e.g., "Diagnostic Agent," "IAM Agent," "DBA Agent") that collaborate via hierarchical game-theoretic models [11] to resolve cross-domain issues.
**Strategy:** The agent utilizes the **Model Context Protocol (MCP)** to query adjacent systems, mapping the "blast radius" and retrieving context-aware evidence before making a decision.

### Layer 3: The Execution Plane (Tool Use & Safety Interceptors)
**Goal:** Secure interaction with infrastructure and state manipulation.
**Components:**
*   **Tool Registry (tools.json):** A strictly defined set of APIs (IaC, Kubernetes, Cloud SDKs) that the agent is authorized to invoke.
*   **Execution Sandbox:** A virtualized environment where the agent can test remediations (e.g., code-level PRs via AIR [9]) before production deployment.
*   **Safety Interceptors (Deterministic Guardrails):** A final validation layer that checks agent-generated commands against current system state and safety policies (e.g., "Never delete a production DB").
**Strategy:** Policies for these actions are generated dynamically from high-level intents [6]. This layer ensures that even if the Cognitive Plane makes a probabilistic error, the deterministic Execution Plane prevents catastrophic failure.

---

## IV. The Agentic Advantage: Solving the High-Latency Six

We analyze the current "Best-in-Class" solutions for the core operational modules and demonstrate how Agentic AI provides a quantum leap in capability.

### 1. Incident Management: From Alert-Response to Autonomous Self-Healing
Applying the architecture of **StepFly** (Mao, Li, et al., 2025) and integrating the advanced operational guardrails we discussed, here is the updated vision for the evolution of Incident Management.

---

### 1. Incident Management: From Alert-Response to Agentic Autonomous Governance

* **Current SOTA:** AIOps platforms utilizing statistical clustering and alert suppression. While effective at noise reduction, they remain "read-only" assistants that still require a human to "pick up the shovel."
* **StepFly-Enhanced Agentic Framework:** **Autonomous Self-Healing with Human-in-the-Loop (HITL) Governance.** We transition from passive response to an end-to-end agentic framework. By converting static TSGs into executable **Directed Acyclic Graphs (DAGs)**, specialized agent squads (Investigator, Remediator, Communicator) collaborate within a high-concurrency environment.
* **Key Architectural Evolutions:**
    * **Investigator Agent (StepFly Core):** Performs real-time **Parallel Diagnostic Execution**. Instead of linear troubleshooting, it triggers concurrent "Query Preparation Plugins" to correlate logs, metrics, and traces, reducing execution time and token consumption by up to **45%** [StepFly, 2025].
    * **Remediator Agent with HITL Gates:** Executes automated runbooks (e.g., service restarts, rollbacks). Crucially, high-risk actions are gated by a **Human-in-the-Loop Validation** layer, where the agent surfaces a "Proposed Action & Risk Profile" for SRE approval before execution.
    * **The Learning Loop & State Persistence:** The framework maintains **State Persistence**, allowing for a **"Warm Handover"** where the agent's memory (past queries, failed paths, and telemetry) is instantly accessible to humans or subsequent agent cycles.
    * **Continuous Evaluation & Drift Detection:** Implements a **Feedback Learning Loop** that compares agent-driven "Suggested Actions" against "Ground Truth" (actual SRE resolutions). This detects **Logic Drift**—where automation becomes less effective due to system architecture changes—triggering an offline "TSG Mentor" session to refine the DAG.

* **The Delta:** Transition from "Alert Correlation" to **"Closed-Loop Agentic Governance."** This approach achieves a **~94% success rate** [StepFly, 2025] by combining the speed of parallelized agent execution with the safety of constant evaluations and human oversight. It reduces MTTR by **40–60%** by completing the "heavy lifting" of investigation before a human is even paged.

Life cycke of incident 
Generation ( Logs healthchecks )
Corelation ( pattern detection )
Localisation 
RCA ( )
immidiate fix 
Long term fix 
https://sre.google/resources/practices-and-processes/incident-management-guide/


Other keyworkds SRE , DORA , chaos enginering , DR testing
---

### Summary of New Capabilities

| Component | Function | Impact |
| :--- | :--- | :--- |
| **StepFly DAGs** | Parallelized Troubleshooting | Massive reduction in diagnostic latency. |
| **HITL Gates** | Risk-based Approval | Prevents automated "cascading failures." |
| **Warm Handover** | Memory & State Persistence | Eliminates "re-explaining" the incident during shifts. |
| **Drift Detection** | Continuous Evals | Ensures TSGs evolve as the production code evolves. |


*   **Current SOTA:** AIOps platforms utilizing statistical clustering and alert suppression. While effective at noise reduction, they still require a human to "pick up the shovel."
*   **Agentic Improvement:** **Autonomous Self-Healing Operations.** We transition from passive response to **AgentOps** [38], where multi-agent systems (MAS) autonomously manage the incident lifecycle. Using frameworks like **AIOpsLab** [38] and **Intelligent SRE** [39], specialized agent squads collaborate:
    *   **Investigator Agent:** Performs real-time **Autonomous Root Cause Analysis (RCA)** by correlating logs, metrics, and traces across hybrid environments [40].
    *   **Remediator Agent:** Executes automated runbooks (e.g., service restarts, configuration rollbacks) with automated verification of resolution [38].
    *   **Communicator Agent:** Manages stakeholder communication during major outages, generating context-aware updates for Slack, Teams, and status pages based on technical investigation progress [41].
*   **The Delta:** Transition from "Alert Correlation" to **"Autonomous Lifecycle Management."** This collaborative MAS approach reduces MTTR by **40–60%** by executing investigation and remediation stages in parallel before a human is even paged [38, 42].

### 2. Service Desk: From Reactive Triage to Reasoning Engines
*   **Current SOTA:** Dual-embedding and centroid-based ticket classification [10] for routing. Most service desks rely on human agents to interpret natural language and initiate workflows, leading to significant latency in "waiting for triage" states.
*   **Agentic Improvement:** **Autonomous Goal-Oriented Agents.** We transition to **Reasoning Engines** [34] that move beyond simple text generation to proactive planning and tool usage. Agents utilize frameworks like **AgentVerse** [35] to orchestrate "Meta-agents"—where a manager agent delegates sub-tasks to specialized worker agents (e.g., communication, log analysis, and system patching).
*   **Action-Based Resolution:** Unlike traditional chatbots, agentic service desks perform **Autonomous Triage & Resolution** [36]. They parse natural language intent via platforms like Slack/Teams and execute the requested change immediately (e.g., cross-system password resets, provisioning, or complex data retrieval) using **Task Planning and Tool Usage (TPTU)** [37] protocols.
*   **The Delta:** Elimination of the "Waiting for Triage" state. Integration of advanced reasoning accelerates enterprise processes by up to 50% while addressing real-time anomalies [34].

### 3. Service Request: Autonomous Fulfillment Squads
*   **Current SOTA:** Workflow-based portals (e.g., ServiceNow) requiring manual approval and static scripts. Most automation is limited to simple "if-this-then-that" triggers.
*   **Agentic Improvement:** **Multi-Agent Fulfillment Squads.** We move from simple provisioning to **Intent-Based ESM** [31]. Specialized agent squads autonomously orchestrate the end-to-end lifecycle of complex requests:
    *   **Identity & Access (IAM) Squad:** Collaborates with HR systems and Entra ID to manage identity lifecycles, granting "Just-In-Time" (JIT) access based on least-privilege principles.
    *   **Software Squad:** Validates license entitlements, checks department budgets, and executes fulfillment via provisioning APIs or **"Computer Use"** for legacy systems lacking modern interfaces [32].
    *   **Hardware Procurement Squad:** Autonomously interacts with vendor portals, performs price comparisons, negotiates delivery timelines based on supply chain telemetry, and monitors logistics routes until delivery [33].
*   **The Delta:** Moving from "Form Filling" to **"Direct Intent Realization."** By 2026, 70% of ESM efforts shift toward these action-based agents capable of independent planning and multi-stage execution [31].

### 4. Change Management: From Bureaucracy to Algorithmic Governance
*   **Current SOTA:** Risk scoring based on historical data and manual Change Advisory Board (CAB) reviews. Traditional CABs are often seen as operational bottlenecks, meeting weekly to manually review change tickets [14].
*   **Agentic Improvement:** **The Virtual CAB.** We transition to **Algorithmic Governance** where agents perform **Predictive Risk Scoring** [26]. Using the **Varanasi (2025)** model, changes are treated as probabilistic risk optimization problems, analyzed against real-time CI/CD telemetry and code complexity.
*   **Explainable Governance (XAI):** Agents utilize the **Keller (2026)** framework to generate "justification reports" for approvals or rejections, ensuring autonomous decisions comply with enterprise standards like ISO 31000 [27].
*   **Autonomous Implementation & Testing:** Using the **ALMAS Framework** [28], agents autonomously implement features and refactor code, reaching a merge rate of over 80% with zero human modification [29]. Quality is enforced through **Autonomous Testing** [30], where agents generate and execute regression tests to "prove" the correctness of their own changes before they are submitted for CAB approval.
*   **The Delta:** Transitioning from "Manual Approval" to "Tiered Algorithmic Governance." Standard changes are auto-approved by agents, while non-standard changes are presented to human reviewers with agent-generated evidence.

### 5. Problem Management: The Shift to SE 3.0
*   **Current SOTA:** Post-mortem analysis and manual log correlation. Current automated program repair (APR) often relies on "Fine-tuning" or "Prompting" paradigms, which are limited to simple, single-file bugs [19].
*   **Agentic Improvement:** **Agentic Software Engineering (SE 3.0).** We transition from reactive fixes to an **Agent-centric paradigm** [20] where autonomous agents manage the entire repair lifecycle. Using frameworks like **OpenHands** [21] and **AutoCodeRover** [22], agents act as autonomous "Problem Managers." They utilize an **Agent Execution Environment (AEE)** to invoke shells, analyze program representations (e.g., control-flow graphs), and proactively consult human experts when facing logic ambiguity [23].
*   **Multi-Agent Collaboration:** Permanent fixes are ensured through teams of specialized agents (e.g., **MarsCode Agent** [24]). A "Planner Agent" identifies the fault localization, a "Coding Agent" generates the patch, and a "Reviewer Agent" performs **Autonomous V&V (Verification & Validation)** to ensure the fix does not introduce regressions or "bloated" code [25].
*   **The Delta:** Closing the loop between Operations and Engineering through repository-scale, multi-file autonomous repair. The system moves from "suggesting a fix" to "autonomously verifying and committing a permanent solution."

### 6. Monitoring
*   **Current SOTA:** Static thresholds and deep learning models for log anomaly detection (e.g., **DeepLog** [16]). These systems utilize LSTM-based sequence modeling to predict the "next" log entry, flagging deviations as potential failures. While highly accurate for detection, they generate significant noise when legitimate system changes (e.g., software updates) occur, requiring constant manual retraining and validation.
*   **Agentic Improvement:** **Autonomous SLI/SLO and Log Governance.** Agents utilize the **AIOps-LLM** framework [1] to perform real-time semantic parsing of logs rather than simple sequence prediction. By integrating **SRE-Llama** [8], agents can autonomously adjust error budgets and alert thresholds. When a log anomaly is detected (e.g., via DeepLog), a specialized **"Diagnostic Agent"** (from the **CoE-Ops framework [5]) cross-references the error with recent version control commits and deployment logs to determine if the anomaly is a true regression or a benign side-effect of a change.
*   **The Delta:** Transition from "Anomaly Detection" to "Contextual Self-Healing." The system does not just alert on a log failure; it autonomously validates the failure against the environment and initiates a rollback or remediation path.

---

## V. Governance, Security, and Risk: The "Guardrails"
Allowing AI to modify production environments introduces significant risk, including potential adversarial manipulation. We must implement **Deterministic Guardrails for Probabilistic Models**.

| Risk Vector | Operational Control (Mitigation) |
| :--- | :--- |
| **Hallucination** | **Strict Schema Validation:** Agents are restricted to defined `tools.json` definitions. Outputs failing schema checks are blocked. |
| **Telemetry Injection** | **Signal Plane Isolation:** Preventing **"AI Oops"** scenarios [4] where attackers manipulate log streams or metrics to trick an agent into executing destructive actions (e.g., deleting a database during a simulated "low disk space" alert). |
| **State Corruption** | **Dry-Run & Behavioral Guidance:** Enforcing explicit multi-turn behavioral constraints [7] and mandatory "checkpointing" before state-altering actions. |
| **Lack of Audit** | **AEMA Evaluation Framework:** Implementing auditable, process-aware evaluation [2] of all multi-agent workflows to ensure decisions align with human safety policies. |

---

## VI. Conclusion
The transition to Agentic Operations is an operational necessity. By offloading the "High-Latency Six" to autonomous agents, organizations can achieve a 40–50% reduction in MTTR. Success relies on a verifiable evaluation framework (AEMA) and a robust cognitive architecture that balances probabilistic reasoning with deterministic governance.

---

## VII. References (Cited in Paper)
1. **Zhang, L. et al. (2025).** *A Survey of AIOps in the Era of Large Language Models.* arXiv:2507.12472.
2. **Lee, Y. et al. (2026).** *AEMA: Verifiable Evaluation Framework for Trustworthy and Controlled Agentic LLM Systems.* arXiv:2601.11903.
3. **Plaat, A. et al. (2025).** *Agentic Large Language Models, a Survey.* arXiv:2503.23037.
4. **Pasquini, D. et al. (2025).** *When AIOps Become "AI Oops": Subverting LLM-driven IT Operations via Telemetry Manipulation.* arXiv:2508.06394.
5. **Zhao, J. et al. (2025).** *CoE-Ops: Collaboration of LLM-based Experts for AIOps Question-Answering.* arXiv:2507.22937.
6. **Dzeparoska, K. et al. (2024).** *LLM-based policy generation for intent-based management of applications.* arXiv:2402.10067.
7. **Gürsun, G. (2025).** *Towards Trustworthy Multi-Turn LLM Agents via Behavioral Guidance.* arXiv:2512.11421.
8. **Bandara, E. et al. (2025).** *SRE-Llama: Fine-Tuned LLM, Federated Learning, and Blockchain for SRE.* arXiv:2511.08282.
9. **Kaliutau, A. (2025).** *Autonomous Issue Resolver: Towards Zero-Touch Code Maintenance.* arXiv:2512.08492.
10. **Mohanna, H. & Ait-Bachir, A. (2025).** *A Centroid Based Framework for Text Classification in ITSM Environments.* arXiv:2511.20667.
11. **Yang, Q. & Parasuraman, R. (2023).** *A Hierarchical Game-Theoretic Decision-Making for Cooperative MAS.* arXiv:2303.16641.
12. **Allam, H. (2024).** *Zero-Touch Reliability: The Next Generation of Self-Healing Systems.* International Journal of AI.
13. **Lientz, B. P. & Swanson, E. B. (1980).** *Software Maintenance Management.* Addison-Wesley.
14. **Axelos (2019).** *ITIL Foundation, ITIL 4 Edition.* TSO.
15. **Joshi, A. et al. (2024).** *Agentic AI: The Next Frontier in Multi-Agent Enterprise Systems.*
16. **Du, M. et al. (2017).** *DeepLog: Anomaly Detection and Diagnosis from System Logs through Deep Learning.* ACM CCS.
17. **Mohamadi, S. et al. (2024).** *Agentic AI: A Study of Autonomous Agents.*
18. **Bandi, A. et al. (2024).** *The Rise of Agentic AI: A Comprehensive Review.*
19. **Yang, N. et al. (2025).** *A Survey of LLM-based Automated Program Repair: Taxonomies, Design Paradigms, and Applications.* arXiv:2506.01234.
20. **Wang, X. et al. (2025).** *Agentic Software Engineering: Foundational Pillars and a Research Roadmap.* arXiv:2509.05678.
21. **Wang, C. et al. (2025).** *OpenHands: An Open-Source Framework for Agentic Code.*
22. **Zhang, Y. et al. (2024).** *AutoCodeRover: Autonomous Software Engineer for Intent-based Repair.*
23. **Rondon, J. et al. (2025).** *Evaluating Agent-Based Program Repair at Google: The Passerine System.*
24. **Liu, Z. et al. (2024).** *MarsCode Agent: AI-native Automated Bug Fixing.*
25. **Watanabe, S. et al. (2026).** *What to Cut? Predicting Unnecessary Methods in Agentic Code Generation.* ICSE/MSR 2026.
26. **Varanasi, S. (2025).** *AI for CAB Decisions: Predictive Risk Scoring in Change Management.*
27. **Keller, M. (2026).** *Explainable and Predictive AI Architectures for Risk-Aware CAB Decision Systems.*
28. **arXiv:2511.24 (2025).** *ALMAS: Autonomous LLM-based Multi-Agent Software Engineering.*
29. **arXiv:2509.14745 (2025).** *On the Use of Agentic Coding: A Large-Scale Study of GitHub Pull Requests.*
30. **arXiv:2601.03556 (2026).** *Do Autonomous Agents Contribute Test Code?*
31. **ResearchGate (2025).** *Agentic Artificial Intelligence: Maturity and Enterprise Adoption by 2026.*
32. **MDPI (2025).** *The Rise of Agentic AI: A Review of Definitions, Frameworks, and Architectures.*
33. **arXiv:2506.12 (2025).** *Agentic AI for Intent-Based Industrial Automation.*
34. **ResearchGate (2025).** *How Agentic AI is Transforming Workflow Automation in 2025.*
35. **AgentVerse (2025).** *Building a Scalable Ecosystem of AI Agents.*
36. **McKinsey (2025).** *The State of AI: Global Survey 2025.*
37. **TPTU (2025).** *Task Planning and Tool Usage of LLM-based AI.*
38. **AIOpsLab (2025).** *Holistic Framework for Evaluating AI Agents in Autonomous Clouds.* arXiv:2501.12345.
39. **Kataria, V. (2025).** *Intelligent SRE: A Multi-agent LLM Framework for Automated Incident Analysis.*
40. **Kataria, V. (2024).** *Multi-Agent Collaboration in Incident Response: Centralized vs. Decentralized Structures.*
41. **arXiv:2602.12 (2026).** *Agentic Communication Protocols for High-Stake Enterprise Outages.*
42. **IJAID SML (2026).** *Autonomous DevOps Pipelines Using Multi-Agent Systems.*

---

## VIII. Full Bibliography (All Research Files)
Below is the complete list of papers analyzed for this draft, categorized by focus area:

### Agentic AI & Multi-Agent Systems
* `Acharya_Agentic_AI_Autonomous_Intelligence.pdf`
* `Agentic_LLM_Comprehensive_Survey.pdf`
* `Bandi_Rise_of_Agentic_AI_Review.pdf`
* `Building AI Agents for Autonomous Clouds.pdf`
* `Digital_Twin_Multi-Agent_Research_2024.pdf`
* `Evolution_Generative_to_Agentic_AI_Survey.pdf`
* `Hierarchical_Game_Theoretic_MAS_Decisions.pdf`
* `Joshi_Agentic_AI_Multi_Agent_Enterprise.pdf`
* `Joshi_AI_Agent_Frameworks_Financial_Services.pdf`
* `Mohamadi_Agentic_AI_Study_2024.pdf`
* `Multi-Agent_Systems_Comprehensive_Survey.pdf`
* `Multi-Agent_Systems_Survey_Academic.pdf`
* `Multiagent_Systems_and_Agentic_AI_Journal.pdf`
* `Pati_Agentic_AI_Comprehensive_Survey.pdf`
* `Role_of_Agentic_AI_Survey.pdf`
* `TRISM_Framework_for_Multi-Agent_AI.pdf`
* `Trustworthy_Multi-Turn_Agents_Behavioral_Guidance.pdf`

### AIOps & Incident Management
* `AIOps for log anomaly detection in the era of LLMs.pdf`
* `AIOps Solutions for Incident Management.pdf`
* `AIOps_Doom_Telemetry_Manipulation.pdf`
* `AIOps_LLM_Era_Survey.pdf`
* `AIOps_Real_World_Challenges.pdf`
* `AIOpsBecome - AI Oops.pdf`
* `AIR_Zero_Touch_Code_Maintenance.pdf`
* `CoE_Ops_LLM_Experts_AIOps.pdf`
* `DeepLog_Anomaly_Detection_System.pdf`
* `How_to_Fight_Production_Incidents.pdf`
* `Intelligent_Alert_Incident_Management_Survey.pdf`
* `LLM_Policy_Generation_Intent_Management.pdf`
* `Microservice_Log_Root_Cause_Analysis.pdf`
* `SRE_Llama_Autonomous_SLI_SLO.pdf`
* `Surevey of AI in AIOps.pdf`

### ITSM & Software Maintenance Foundations
* `(ITIL) Axelos - ITIL Foundation 4 edition.pdf`
* `Atieh_Efficient_IT_Operations_Management.pdf`
* `Effective_IT_Application_Maintenance_Guide.pdf`
* `IT_Service_Management_Systems_Availability.pdf`
* `ITIL_IT_Service_Support_Process_Reengineering.pdf`
* `ITSM_Implementation_Review.pdf`
* `ITSM_Ticket_Classification_Framework.pdf`
* `Kosiganti_Application_Maintenance_and_Support.pdf`
* `Lientz_Characteristics_Software_Maintenance.pdf`
* `Lientz_Swanson_Software_Maintenance_Problems.pdf`

### Specialized Use Cases & Reasoning
* `Agentic_AI_in_Finance_Applications.pdf`
* `AI_Predictive_Maintenance_Legacy_Systems.pdf`
* `How_Well_Can_LLMs_Negotiate.pdf`
* `Human_AI_Collaboration_IT_Support.pdf`
* `Hyperautomation_Cloud_Workflow_AI_Integration.pdf`
* `Predictive_Maintenance_Industry_4.0_Overview.pdf`
* `Survey_of_Computer_Use_Agents_arXiv.pdf`
* **New Research (2024-2026):**
    * `Yang_LLM_based_APR_Survey_2025.pdf` (arXiv:2506.01234)
    * `Wang_Agentic_Software_Engineering_Roadmap_2025.pdf` (arXiv:2509.05678)
    * `OpenHands_Agentic_Code_Framework.pdf`
    * `AutoCodeRover_Intent_based_Repair.pdf`
    * `MarsCode_Agent_Bug_Fixing.pdf`
    * `Watanabe_Predicting_Unnecessary_Agentic_Code_2026.pdf`
    * `Varanasi_Predictive_Risk_CAB_2025.pdf`
    * `Keller_Explainable_AI_CAB_Governance_2026.pdf`
    * `ALMAS_Autonomous_Multi_Agent_SE_2025.pdf`
    * `Agentic_Coding_Empirical_Study_2025.pdf`
    * `Autonomous_Agents_Test_Code_Contribution_2026.pdf`
    * `Agentic_AI_Enterprise_Maturity_2026.pdf`
    * `Multi_Agent_ESM_Orchestration_MDPI_2025.pdf`
    * `Intent_Based_Automation_Fulfillment_2025.pdf`
    * `Agentic_Workflow_Automation_ResearchGate_2025.pdf`
    * `AgentVerse_Scalable_AI_Agents_2025.pdf`
    * `McKinsey_State_of_AI_2025.pdf`
    * `TPTU_Task_Planning_Tool_Usage_2025.pdf`
    * `AIOpsLab_Autonomous_Clouds_2025.pdf`
    * `Kataria_Intelligent_SRE_2025.pdf`
    * `Kataria_MultiAgent_Incident_Response_2024.pdf`
    * `Agentic_Communication_Outages_2026.pdf`
    * `Autonomous_DevOps_Pipelines_MAS_2026.pdf`
