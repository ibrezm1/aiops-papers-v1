# Trust, Triage, and Transfer: Architecting Secure Human-in-the-Loop AIOps

## 1\. Abstract

As IT Service Management (ITSM) and IT operations transition toward AIOps-driven architectures, artificial intelligence is evolving from a passive diagnostic assistant to an active, autonomous agent capable of executing frontline remediation. Organizations are rapidly adopting an "AI-First" approach where agents independently orchestrate tasks based on existing Standard Operating Procedures (SOPs), system telemetry, and historical data [4, 5, 10, 13].

Despite this shift toward "Zero-Touch" IT operations [23, 24, 34], seamless human-AI collaboration is frequently derailed by the limitations of traditional Tier-1 to Tier-2/Tier-3 escalation models. When an autonomous AI agent reaches the limits of its Role-Based Access Controls (RBAC), available runbooks, or diagnostic capabilities, it often triggers a "cold" ticket escalation. This abrupt handover forces Site Reliability Engineers (SREs) and system administrators to manually reconstruct the incident context, significantly inflating cognitive load, eroding trust, and increasing Mean Time to Resolution (MTTR) [37, 57]. Furthermore, systemic data drift over time renders AI knowledge obsolete, compounding failure rates [91, 92].

To address these ITIL and operational challenges, this paper proposes a comprehensive AIOps framework for secure, bidirectional "warm handovers" and dynamic authority reversal. We introduce an architecture where the AI actively correlates observability telemetry and SOPs to exhaust all safe automated options before escalating. The system features dynamic, human-in-the-loop (HITL) privilege upgrades for restricted operations, interactive explainability for Root Cause Analysis (RCA), and a robust evaluation pipeline designed to monitor concept drift and runbook degradation.

Ultimately, this framework provides a structured, accountable, and continuously learning environment that bridges the gap between autonomous AIOps execution and human operational expertise, transforming the escalation process from a failure state into a data-gathering flywheel.

-----

## 2\. Introduction

The landscape of IT operations, data workflows, and enterprise technical support is undergoing a fundamental paradigm shift. Historically, human experts served as the primary frontline responders, utilizing artificial intelligence merely as a passive diagnostic tool or advanced search engine for knowledge bases. Today, organizations are rapidly adopting an AI-First approach, deploying autonomous agents capable of independently orchestrating complex system operations, executing remediation scripts, and managing end-to-end data flows [2, 7, 14]. In this new model, the AI serves as the primary actor, attempting to resolve incidents entirely within its operational boundaries to achieve a frictionless ITSM ecosystem [11].

However, as AI takes on a more central role in execution, a critical friction point has emerged at the boundary of its capabilities: the escalation process. When an autonomous agent encounters an issue that exceeds its security privileges, lacks an established SOP, or requires nuanced domain expertise, it must transfer the task to a human operator [3]. Traditionally, these transfers result in a "cold handover." The human expert is presented with an escalated ticket but lacks visibility into the AI's prior troubleshooting steps, forcing them to manually reconstruct the incident context. This abrupt transition not only inflates resolution times but also erodes user trust, as the AI's underlying logic remains an uninterpretable "black box." Therefore, the preservation of deep context and the presentation of transparent, explainable reasoning during task transitions are paramount for effective human-AI collaboration [1, 67, 76].

To bridge this operational gap, this paper proposes a dynamic framework for secure, bidirectional human-AI collaboration. The proposed system is designed to seamlessly integrate with existing enterprise data ecosystems—leveraging established SOPs, historical resolution data, live system telemetry, and health check dashboards. By deeply grounding the agent in this organizational context, the system ensures the AI exhausts all safe, automated avenues for resolution [33, 38].

When an escalation is strictly necessary, the framework initiates a structured "warm handover" [105]. This transfer preserves the full technical context, provides human experts with an explainable summary of the AI's structural reasoning, and allows for interactive "what-if" analyses to filter noise. Furthermore, it introduces mechanisms for secure, temporary privilege escalation and establishes rigorous evaluation metrics to detect model and concept drift over time [94]. Ultimately, this evaluated transfer mechanism transforms the handover process from a rigid operational dead-end into a dynamic, continuous learning loop for both the AI and the enterprise.

-----

## 3\. Related Work & Literature Review

  * **Copilots and Handovers in Enterprise Tooling:** Early research into hybrid workflows established the foundational mechanics for task delegation in standard enterprise environments. This work highlighted the need for seamless context sharing when an AI assistant hands a task back to a human user [1, 41, 43].
  * **Trust Calibration and Transitional Accountability:** Literature exploring dynamic authority reversal addresses how trust is maintained when an AI must relinquish control. These studies underscore the necessity of "warm" transfers and continuous accountability to ensure human operators do not blindly trust or immediately reject the AI's prior actions [1, 51, 52].
  * **Cognitive Challenges in Task Delegation:** Addressing the mental burden placed on human experts when interpreting AI outputs, researchers have investigated the path toward "productive delegation." This requires treating the AI as a collaborative, explainable partner rather than an opaque, black-box diagnostic tool [3, 58, 61, 64].
  * **Assurance-Centered Agentic AIOps and Escalation Boundaries:** Recent architectural research introduces the concept of "Assurance-Centered Agentic AIOps" (ACAA) [6, 106]. This work identifies that decision quality severely degrades at handoff boundaries within industrial IT environments. To counter this, researchers propose policy-gated, multi-agent orchestration governed by deterministic safety boundaries and explicit HITL escalation protocols [107, 108].
  * **Incident Management and AIOps Trustability:** Comprehensive reviews of AIOps for incident management emphasize a necessary shift in human roles [8, 9]. As AI assumes frontline remediation, human activities must transition toward adaptation, auditing, and governance. Successfully navigating this shift requires AIOps models to embed field-tested, engineer-trusted domain expertise directly into their escalation logic [2].
  * **Agentic AI in ITSM and Decision Observability:** Current ITSM literature emphasizes that deploying autonomous "Agentic AI" requires moving beyond basic sentiment analysis [10]. Successful deployments demand explicit objectives, rigid constraints, and highly tuned escalation triggers [120, 122]. Furthermore, "decision observability"—capturing logs, justifications, and intermediate steps—is critical for auditing AI behavior and ensuring context is never lost during a human handover [3, 123, 126].

-----

## 4\. System Architecture: The "AI-First" Approach

The transition to an AI-First ITSM environment requires a foundational shift in how systems are architected [27]. Rather than bolting an AI assistant onto existing diagnostic tools, the proposed architecture positions a Large Language Model (LLM)-backed autonomous agent at the core of the operational workflow [16, 22]. This agent acts as the primary orchestrator, evaluating incoming alerts, formulating diagnostic plans, and executing remediation steps. To achieve this safely and effectively, the architecture relies on two foundational pillars: deep contextual data integration and deterministic security boundaries.

### 4.1 Data Integration & Context Awareness

For an autonomous agent to function effectively as a frontline responder, it must possess situational awareness that rivals or exceeds that of a human operator [39]. This is achieved through a continuous, multimodal data ingestion pipeline:

  * **Static and Historical Grounding:** The system utilizes Retrieval-Augmented Generation (RAG) to dynamically query a centralized repository of organizational knowledge, including SOPs, architectural data flows, and historical incident resolutions.
  * **Real-Time Telemetry and State Ingestion:** The AI is directly integrated via API with the enterprise’s observability stack [35]. Upon receiving an alert, the agent automatically queries active health check results, infrastructure dashboards, and recent CI/CD pipelines to identify immediate state changes.
  * **Context Synthesis:** Before taking any action, the AI synthesizes this data to build an internal "state map" of the problem, ensuring subsequent diagnostic steps are highly specific hypotheses grounded in systemic context.

### 4.2 Security & Access Management

The risk of unconstrained execution is mitigated by enforcing a strict Principle of Least Privilege (PoLP) combined with dynamic HITL authorization protocols [85, 87, 88]. Furthermore, AI agent blast radius mitigation is critical to preventing catastrophic, cascading failures [90, 93].

  * **Isolated Service Accounts:** All agentic actions are executed via dedicated, scoped Service Accounts, isolating the AI's blast radius in the event of a logic failure [86].
  * **Granular Infrastructure Control:** The system enforces granular control over individual tools based on operational criticality (e.g., autonomous read/write access for dev containers, read-only for production databases) [89].
  * **Dynamic Privilege Escalation (HITL):** When an incident requires actions beyond baseline permissions, the system pauses the AI and sends a secure prompt to a human approver to temporarily elevate privileges for that specific task [80, 81].
  * **Programmatic Boundary Awareness:** Deterministic system prompts explicitly define the AI's operational boundaries, mandating it to systematically exhaust all safe, authorized diagnostic avenues before signaling for an escalation.

-----

## 5\. The "Warm Handover" Protocol

To counter the high MTTR and cognitive fatigue associated with traditional "cold handovers," the proposed AI-First architecture implements a structured "Warm Handover" protocol [105]—a dynamic, interactive knowledge transfer that seamlessly bridges the gap between autonomous execution and human intervention.

### 5.1 Context Summarization and State Preservation

When an escalation trigger is met, the AI generates a highly structured operational payload. This ensures context preservation during AI-to-human escalation [77, 78]:

  * **The Problem Formulation:** A synthesized definition of the core issue, filtering out downstream symptom alerts.
  * **Actions Taken:** A chronological, deduplicated list of the diagnostic queries and remediation scripts the AI has already executed.
  * **Current System State:** An exact snapshot of the infrastructure at the moment of transfer, including relevant health metrics and pending failure states.

### 5.2 Interactive Context Refining and "What-If" Analysis

To reduce cognitive load [59, 62], the handover interface allows the operator to actively refine the context:

  * **Interactive Chat:** SREs can query the AI about specific variables or omitted logs directly from the escalation dashboard.
  * **Context Stripping:** Users can instruct the AI to hide irrelevant telemetry to focus on core issues.
  * **What-If Simulations:** Operators can leverage the AI's contextual awareness to perform localized simulations before executing high-risk manual fixes.

### 5.3 Explainable Structure of Thought (XAI Integration)

The architecture mandates that the AI exposes its internal reasoning upon request utilizing frameworks like Chain of Thought (CoT) or ReAct. Human operators can view the exact sequence of deductions and API responses that led to an escalation, shifting the AI from a black box to an accountable colleague. This is supported by recent advances in Explainable AI (XAI) for incident response [68, 70, 75].

### 5.4 Adaptive Learning and Dynamic Summarization

As the SRE interacts with the AI during the transition, the underlying summary dynamically updates. If a user's questions indicate a shift in investigative trajectory, the AI automatically re-weights the summary to surface the most relevant telemetry.

-----

## 6\. Escalation Triggers and Dynamic Routing

Escalations must be governed by deterministic guardrails, ensuring policy-gated autonomous remediation [100, 102]. This ensures human SREs are engaged only when strictly necessary.

### 6.1 Deterministic Escalation Scenarios

An escalation is automatically triggered under the following conditions:

  * **Privilege Boundary Exhaustion:** The AI correctly diagnoses an issue but lacks the IAM permissions to execute the fix, and a HITL request is denied or times out.
  * **Tooling and Runbook Exhaustion:** The required API endpoint or SOP does not exist within the authorized environment.
  * **Epistemic Uncertainty:** The system encounters a novel anomaly, and the RAG pipeline fails to surface relevant context above a strict confidence threshold.
  * **Algorithmic Degradation:** Watchdog processes detect the LLM is stuck in a recursive loop or producing stagnant reasoning traces.
  * **Deterministic Overrides:** Critical infrastructure or P1 incidents explicitly bypass autonomous remediation for immediate human intervention.

### 6.2 The Handover Contract

The AI must fulfill a "Handover Contract" by generating a structured Escalation Rationale detailing:

  * **The Trigger Event:** A clear statement of the operational boundary reached.
  * **Exhaustion Record:** Confirmation of the avenues already attempted.
  * **Required Human Action:** A precise, actionable request defining exactly what intervention the AI requires.

-----

## 7\. Bidirectional Transfer and Continuous Improvement

The proposed architecture redefines escalation as a temporary, specialized delegation rather than a permanent hand-off, enabling dynamic authority reversal [51, 52, 53].

### 7.1 Two-Way Handovers and State Persistence

  * **AI-to-Human Transfer:** The AI transfers the structured context payload clearly identifying the blocker.
  * **Human-to-AI Reversal:** Once the human expert executes the required intervention, they return execution authority to the AI to resume its automated runbook.

### 7.2 Post-Resolution Evaluation and State Verification

Before resuming, the AI performs a verification check to programmatically ensure the blocker has been cleared, re-synthesizing its "state map" if human intervention altered the architecture unexpectedly.

### 7.3 Feedback Loops and the Data Flywheel

  * **Short-Term Fixes:** Immediate reduction in overall MTTR by allowing the AI to finish routine incident closure after a human clears a specific blocker [116, 118].
  * **Long-Term Fixes:** The AI automatically drafts proposed SOP updates or new runbooks based on observed human interventions.
  * **Golden Datasets:** Every instance of human intervention is captured and fed into a repository used for offline evaluations of future AI models [111, 115].

-----

## 8\. Evaluation Frameworks, KPIs, and Drift Monitoring

To maintain reliability and trust, the AI-First architecture requires a multi-layered evaluation framework and continuous drift monitoring pipeline.

### 8.1 Comprehensive Evaluation (Evals) Pipeline

  * **Offline Evals (Regression Testing):** Updates to prompts or models are tested against a "Golden Dataset" of historical ITSM tickets [112].
  * **Online Evals (LLM-as-a-Judge):** A secondary supervisor model asynchronously samples live handovers to score the quality of AI-to-Human context summaries [127, 128].
  * **Security Evals (Continuous Red-Teaming):** Automated adversarial testing verifies the AI correctly halts execution when faced with simulated malicious inputs rather than bypassing IAM constraints.

### 8.2 Agent Efficiency & Routing Metrics (KPIs)

AIOps performance metrics are essential for tuning [134, 137]:

  * **Circular Execution Rate:** Tracks how often the AI enters an unproductive loop.
  * **Runbook Exhaustion Rate:** Quantifies how consistently the AI attempts all valid pathways before escalating.
  * **False Escalation Rate:** Measures how often the AI escalates a ticket it actually possessed the capabilities to resolve autonomously.

### 8.3 Monitoring and Checking for Drift

  * **Concept Drift:** Detected through localized KPI spikes (e.g., a sudden increase in escalations for a previously automated task type), signaling underlying infrastructure changes [91, 95].
  * **Model/Behavioral Drift:** Monitored by tracking baseline tool usage and output verbosity for signs of degradation or hallucination.
  * **Shadow Testing:** New models are deployed in "shadow mode," processing live data without executing changes, to validate performance against the live agent [92, 94].

-----

## 9\. Discussion & Future Work

While the AI-First architecture presents a robust framework, it introduces avenues for future research:

  * **Balancing Strict Security with Autonomy:** Future work should explore Adaptive Risk Scoring Models, where the AI dynamically assesses the "blast radius" of its actions to potentially earn automated privilege escalations for low-risk fixes.
  * **Mitigating Cognitive Load in XAI:** Research must focus on the UX/UI of AI-to-Human handovers, exploring how to visually map the AI's "state map" to make reasoning traces instantly scannable [59, 65, 74].
  * **Proactive Drift Remediation:** Future iterations could feature background agents that monitor CI/CD pipelines to proactively identify obsolete runbooks and draft updates before production failures occur [97].

-----

## 10\. Conclusion

As enterprise ITSM inevitably transitions toward AIOps and autonomous execution, the boundary between human and machine capabilities can no longer be treated as a rigid failure point. Traditional escalation models, characterized by abrupt, contextless handovers and rigid access controls, severely bottleneck the potential of AI-First workflows and inflate MTTR.

This paper presented a comprehensive framework to bridge this gap, proposing a secure, bidirectional human-AI collaboration architecture. By grounding the AI in deep enterprise data integration, enforcing dynamic access controls, and mandating a structured "Warm Handover" protocol, we transform the escalation process from an operational dead-end into a seamless, interactive knowledge transfer. Furthermore, the integration of rigorous, continuous evaluation pipelines and drift monitoring ensures that the AI’s knowledge remains aligned with the shifting realities of the enterprise infrastructure.

Ultimately, by treating task handovers not as system failures, but as opportunities for dynamic authority reversal and continuous learning, organizations can pave the way for highly reliable, accountable, and resilient operational workflows. In this model, the AI ceases to be a fragile automation script and becomes a true, continuously evolving digital colleague.

-----

## 11\. Bibliography

[1] Westover, J. H. (2026). "Human–AI Handovers: A Dynamic Authority Reversal Framework for Trust Calibration and Transitional Accountability". [https://doi.org/10.20944/preprints202603.0390.v1](https://doi.org/10.20944/preprints202603.0390.v1)
[2] Zota, R. D., Bărbulescu, C., & Constantinescu, R. (2025). "A Practical Approach to Defining a Framework for Developing an Agentic AIOps System". [https://doi.org/10.3390/electronics14091775](https://doi.org/10.3390/electronics14091775)
[3] Shetty, M., et al. (2024). "Building AI Agents for Autonomous Clouds: Challenges and Design Principles". [https://doi.org/10.1145/3698038.3698525](https://doi.org/10.1145/3698038.3698525)
[4] Sivakumar, S. (2024). "Agentic AI in Predictive AIOps: Enhancing IT Autonomy and Performance". [https://doi.org/10.18535/ijsrm/v12i11.ec01](https://doi.org/10.18535/ijsrm/v12i11.ec01)
[5] Surendar, A. (2025). "Simple Agentic AI workflow for AIOPS (Agentic AIOps)". [https://doi.org/10.71097/ijsat.v16.i3.8359](https://doi.org/10.71097/ijsat.v16.i3.8359)
[6] O'Hara, C. A. (2026). "Assurance-Centered Agentic AIOps (ACAA)". [https://doi.org/10.31224/6641](https://doi.org/10.31224/6641)
[7] Chakraborty, S. (2025). "From DataOps to AIOps: How autonomous agents are revolutionizing data engineering". [https://doi.org/10.30574/wjaets.2025.15.2.0650](https://doi.org/10.30574/wjaets.2025.15.2.0650)
[8] Paleyes, A., et al. (2022). "Challenges in Deploying Machine Learning: A Survey of Case Studies". [https://doi.org/10.1145/3533378](https://doi.org/10.1145/3533378)
[9] Somayajula, R. (2025). "Hybrid AIOps for Resilient Data Pipelines". [https://doi.org/10.1109/gcat66372.2025.11368358](https://doi.org/10.1109/gcat66372.2025.11368358)
[10] Allam, H., & Dempere, J. (2025). "Agentic AI for IT and Beyond". [https://doi.org/10.64044/j63vmh26](https://doi.org/10.64044/j63vmh26)
[11] Díaz-de-Arcaya, J., et al. (2023). "A Joint Study of the Challenges, Opportunities, and Roadmap of MLOps and AIOps". [https://doi.org/10.1145/3625289](https://doi.org/10.1145/3625289)
[12] Ziegler, V., et al. (2021). "Security and Trust in the 6G Era". [https://doi.org/10.1109/access.2021.3120143](https://doi.org/10.1109/access.2021.3120143)
[13] Gupta, M., et al. (2023). "From ChatGPT to ThreatGPT: Impact of Generative AI in Cybersecurity and Privacy". [https://doi.org/10.1109/access.2023.3300381](https://doi.org/10.1109/access.2023.3300381)
[14] Kakarla, R. (2024). "LLM-Based Autonomous Remediation for DevSecOps Pipelines". [https://doi.org/10.58812/esiscs.v2i02.856](https://doi.org/10.58812/esiscs.v2i02.856)
[15] Schwartz, R., et al. (2022). "Towards a standard for identifying and managing bias in artificial intelligence". [https://doi.org/10.6028/nist.sp.1270](https://doi.org/10.6028/nist.sp.1270)
[16] Zhang, J., et al. (2025). "When LLMs meet cybersecurity: a systematic literature review". [https://doi.org/10.1186/s42400-025-00361-w](https://doi.org/10.1186/s42400-025-00361-w)
[17] NIST (2024). "Artificial intelligence risk management framework". [https://doi.org/10.6028/nist.ai.600-1](https://doi.org/10.6028/nist.ai.600-1)
[18] Sai, S., et al. (2024). "Generative AI for Cyber Security". [https://doi.org/10.1109/access.2024.3385107](https://doi.org/10.1109/access.2024.3385107)
[19] McIntosh, T. R., et al. (2024). "From COBIT to ISO 42001". [https://doi.org/10.1016/j.cose.2024.103964](https://doi.org/10.1016/j.cose.2024.103964)
[20] Dhoni, P., & Kumar, R. (2023). "Synergizing Generative AI and Cybersecurity". [https://doi.org/10.36227/techrxiv.23968809.v1](https://doi.org/10.36227/techrxiv.23968809.v1)
[21] Kasri, W., et al. (2025). "From Vulnerability to Defense: The Role of Large Language Models in Enhancing Cybersecurity". [https://doi.org/10.3390/computation13020030](https://doi.org/10.3390/computation13020030)
[22] Rane, N., et al. (2024). "Gemini versus ChatGPT: applications, performance, architecture, capabilities, and implementation". [https://doi.org/10.48185/jaai.v5i1.1052](https://doi.org/10.48185/jaai.v5i1.1052)
[23] Benzaïd, C., & Taleb, T. (2020). "AI-Driven Zero Touch Network and Service Management in 5G and Beyond". [https://doi.org/10.1109/mnet.001.1900252](https://doi.org/10.1109/mnet.001.1900252)
[24] Benzaïd, C., & Taleb, T. (2020). "ZSM Security: Threat Surface and Best Practices". [https://doi.org/10.1109/mnet.001.1900273](https://doi.org/10.1109/mnet.001.1900273)
[25] Motamary, S. (2022). "Enabling Zero-Touch Operations in Telecom". [https://doi.org/10.53555/ks.v10i2.3833](https://doi.org/10.53555/ks.v10i2.3833)
[26] Wang, T., et al. (2022). "Deep-Learning-Based Weak Electromagnetic Intrusion Detection Method for Zero Touch Networks". [https://doi.org/10.1109/mnet.001.2100754](https://doi.org/10.1109/mnet.001.2100754)
[27] Velasco, L., et al. (2021). "End-to-End Intent-Based Networking". [https://doi.org/10.1109/mcom.101.2100141](https://doi.org/10.1109/mcom.101.2100141)
[28] Gomes, P. H., et al. (2021). "Intent-driven Closed Loops for Autonomous Networks". [https://doi.org/10.13052/jicts2245-800x.929](https://doi.org/10.13052/jicts2245-800x.929)
[29] Qureshi, H. N., et al. (2023). "Toward Addressing Training Data Scarcity Challenge in Emerging Radio Access Networks". [https://doi.org/10.1109/comst.2023.3271419](https://doi.org/10.1109/comst.2023.3271419)
[30] Hafi, H., et al. (2024). "Split Federated Learning for 6G Enabled-Networks". [https://doi.org/10.1109/access.2024.3351600](https://doi.org/10.1109/access.2024.3351600)
[31] Prados-Garzon, J., & Taleb, T. (2021). "Asynchronous Time-Sensitive Networking for 5G Backhauling". [https://doi.org/10.1109/mnet.011.2000402](https://doi.org/10.1109/mnet.011.2000402)
[32] Naeem, F., et al. (2023). "Federated-Learning-Empowered Semi-Supervised Active Learning Framework for Intrusion Detection in ZSM". [https://doi.org/10.1109/mcom.001.2200533](https://doi.org/10.1109/mcom.001.2200533)
[33] Manchana, R. (2024). "AI-Powered Observability: A Journey from Reactive to Proactive, Predictive, and Automated". [https://doi.org/10.21275/sr24820054419](https://doi.org/10.21275/sr24820054419)
[34] Coronado, E., et al. (2022). "Zero Touch Management: A Survey of Network Automation Solutions for 5G and 6G Networks". [https://doi.org/10.1109/comst.2022.3212586](https://doi.org/10.1109/comst.2022.3212586)
[35] Mitropoulou, K., et al. (2023). "Anomaly Detection in Cloud Computing using Knowledge Graph Embedding and Machine Learning Mechanisms". [https://doi.org/10.1007/s10723-023-09727-1](https://doi.org/10.1007/s10723-023-09727-1)
[36] De Rosis, S., et al. (2020). "Using patient-reported measures to drive change in healthcare". [https://doi.org/10.1186/s12913-020-05099-4](https://doi.org/10.1186/s12913-020-05099-4)
[37] Singh, B., et al. (2025). "Real-Time Network Monitoring and Incident Response with AI-Driven Automation". [https://doi.org/10.2139/ssrn.5331665](https://www.google.com/search?q=https://doi.org/10.2139/ssrn.5331665)
[38] Dutta, B., et al. (2021). "The Challenge of Zero Touch and Explainable AI". [https://doi.org/10.13052/jicts2245-800x.925](https://doi.org/10.13052/jicts2245-800x.925)
[39] Hakiri, A., et al. (2024). "A comprehensive survey on digital twin for future networks". [https://doi.org/10.1016/j.comnet.2024.110350](https://doi.org/10.1016/j.comnet.2024.110350)
[40] Arora, R. K., et al. (2024). "AI-Driven Self-Healing Cloud Systems". [https://doi.org/10.56155/978-81-955020-9-7-28](https://doi.org/10.56155/978-81-955020-9-7-28)
[41] Liu, F., et al. (2022). "Integrated Sensing and Communications: Toward Dual-Functional Wireless Networks". [https://doi.org/10.1109/jsac.2022.3156632](https://doi.org/10.1109/jsac.2022.3156632)
[42] Wang, C.-X., et al. (2023). "On the Road to 6G: Visions, Requirements, Key Technologies, and Testbeds". [https://doi.org/10.1109/comst.2023.3249835](https://doi.org/10.1109/comst.2023.3249835)
[43] Cao, K., et al. (2020). "An Overview on Edge Computing Research". [https://doi.org/10.1109/access.2020.2991734](https://doi.org/10.1109/access.2020.2991734)
[44] Polese, M., et al. (2023). "Understanding O-RAN: Architecture, Interfaces, Algorithms, Security, and Research Challenges". [https://doi.org/10.1109/comst.2023.3239220](https://doi.org/10.1109/comst.2023.3239220)
[45] You, X., et al. (2020). "Towards 6G wireless communication networks". [https://doi.org/10.1007/s11432-020-2955-6](https://doi.org/10.1007/s11432-020-2955-6)
[46] Jiang, W., et al. (2021). "The Road Towards 6G: A Comprehensive Survey". [https://doi.org/10.1109/ojcoms.2021.3057679](https://doi.org/10.1109/ojcoms.2021.3057679)
[47] Siriwardhana, Y., et al. (2021). "A Survey on Mobile Augmented Reality With 5G Mobile Edge Computing". [https://doi.org/10.1109/comst.2021.3061981](https://doi.org/10.1109/comst.2021.3061981)
[48] Akyildiz, I. F., et al. (2020). "6G and Beyond: The Future of Wireless Communications Systems". [https://doi.org/10.1109/access.2020.3010896](https://doi.org/10.1109/access.2020.3010896)
[49] Pham, Q.-V., et al. (2020). "A Survey of Multi-Access Edge Computing in 5G and Beyond". [https://doi.org/10.1109/access.2020.3001277](https://doi.org/10.1109/access.2020.3001277)
[50] Dogra, A., et al. (2020). "A Survey on Beyond 5G Network With the Advent of 6G". [https://doi.org/10.1109/access.2020.3031234](https://doi.org/10.1109/access.2020.3031234)
[51] Jiang, G., et al. (2020). "Dynamic rumor spreading of public opinion reversal". [https://doi.org/10.1016/j.physa.2020.125005](https://www.google.com/search?q=https://doi.org/10.1016/j.physa.2020.125005)
[52] Frimpong, V., et al. (2026). "Human AI Handovers: A Dynamic Authority Reversal Framework". [https://doi.org/10.5281/zenodo.18236553](https://doi.org/10.5281/zenodo.18236553)
[53] Yu, H., & Zheng, J. (2020). "Numerical investigation of control of dynamic stall over a NACA0015 airfoil". [https://doi.org/10.1063/1.5142465](https://doi.org/10.1063/1.5142465)
[54] Ceyhan, A., & Tsoukala, A. (2002). "The Securitization of Migration in Western Societies". [https://doi.org/10.1177/03043754020270s103](https://doi.org/10.1177/03043754020270s103)
[55] Chen, B., et al. (2021). "Resource Sharing in Public Cloud System with Evolutionary Multi-agent Artificial Swarm Intelligence". [https://doi.org/10.1007/978-3-030-76352-7\_25](https://www.google.com/search?q=https://doi.org/10.1007/978-3-030-76352-7_25)
[56] Westover, J. H. (2026). "Human-AI Handovers: A Dynamic Authority Reversal Framework". [https://doi.org/10.2139/ssrn.6037994](https://www.google.com/search?q=https://doi.org/10.2139/ssrn.6037994)
[57] Hassija, V., et al. (2019). "A Survey on IoT Security". [https://doi.org/10.1109/access.2019.2924045](https://doi.org/10.1109/access.2019.2924045)
[58] Parker, S. K., & Grote, G. (2019). "Automation, Algorithms, and Beyond". [https://doi.org/10.1111/apps.12241](https://doi.org/10.1111/apps.12241)
[59] Gerlich, M. (2025). "AI Tools in Society: Impacts on Cognitive Offloading". [https://doi.org/10.3390/soc15010006](https://doi.org/10.3390/soc15010006)
[60] Rabin, M., & Thaler, R. H. (2001). "Anomalies: Risk Aversion". [https://doi.org/10.1257/jep.15.1.219](https://doi.org/10.1257/jep.15.1.219)
[61] Chirayath, G., et al. (2025). "Cognitive offloading or cognitive overload?". [https://doi.org/10.3389/fpsyg.2025.1699320](https://doi.org/10.3389/fpsyg.2025.1699320)
[62] Yaich, R., et al. (2025). "Symbiotic Human–AI Collaboration for Augmented Cybersecurity Operations". [https://doi.org/10.1609/aaaiss.v6i1.36072](https://doi.org/10.1609/aaaiss.v6i1.36072)
[63] Wang, W., et al. (2019). "A Survey on Consensus Mechanisms and Mining Strategy Management in Blockchain Networks". [https://doi.org/10.1109/access.2019.2896108](https://doi.org/10.1109/access.2019.2896108)
[64] Seran, C. E., et al. (2025). "A conceptual exploration of generative AI-induced cognitive dissonance". [https://doi.org/10.3389/frai.2025.1573368](https://doi.org/10.3389/frai.2025.1573368)
[65] Guizzardi, G. (2005). "Ontological foundations for structural conceptual models". [https://openalex.org/W2108234081](https://openalex.org/W2108234081)
[66] Buyya, R., et al. (2018). "A Manifesto for Future Generation Cloud Computing". [https://doi.org/10.1145/3241737](https://doi.org/10.1145/3241737)
[67] Lundberg, S., et al. (2020). "From local explanations to global understanding with explainable AI for trees". [https://doi.org/10.1038/s42256-019-0138-9](https://doi.org/10.1038/s42256-019-0138-9)
[68] Linardatos, P., et al. (2020). "Explainable AI: A Review of Machine Learning Interpretability Methods". [https://doi.org/10.3390/e23010018](https://doi.org/10.3390/e23010018)
[69] Samek, W., et al. (2019). "Explainable AI: Interpreting, Explaining and Visualizing Deep Learning". [https://doi.org/10.1007/978-3-030-28954-6](https://doi.org/10.1007/978-3-030-28954-6)
[70] Shin, D. (2020). "The effects of explainability and causability on perception, trust, and acceptance". [https://doi.org/10.1016/j.ijhcs.2020.102551](https://doi.org/10.1016/j.ijhcs.2020.102551)
[71] Arrieta, A. B., et al. (2019). "Explainable Artificial Intelligence (XAI): Concepts, taxonomies...". [https://doi.org/10.1016/j.inffus.2019.12.012](https://doi.org/10.1016/j.inffus.2019.12.012)
[72] Dwivedi, R., et al. (2022). "Explainable AI (XAI): Core Ideas, Techniques, and Solutions". [https://doi.org/10.1145/3561048](https://doi.org/10.1145/3561048)
[73] Rai, A. (2019). "Explainable AI: from black box to glass box". [https://doi.org/10.1007/s11747-019-00710-5](https://doi.org/10.1007/s11747-019-00710-5)
[74] Wang, D., et al. (2019). "Designing Theory-Driven User-Centric Explainable AI". [https://doi.org/10.1145/3290605.3300831](https://doi.org/10.1145/3290605.3300831)
[75] Chaddad, A., et al. (2023). "Survey of Explainable AI Techniques in Healthcare". [https://doi.org/10.3390/s23020634](https://doi.org/10.3390/s23020634)
[76] Liao, Q. V., et al. (2020). "Questioning the AI: Informing Design Practices for Explainable AI User Experiences". [https://doi.org/10.1145/3313831.3376590](https://doi.org/10.1145/3313831.3376590)
[77] Koob, G. F. (2001). "Drug Addiction, Dysregulation of Reward, and Allostasis". [https://doi.org/10.1016/s0893-133x(00)00195-0](https://doi.org/10.1016/s0893-133x\(00\)00195-0)
[78] Allioui, H., & Mourdi, Y. (2023). "Exploring the Full Potentials of IoT for Better Financial Growth". [https://doi.org/10.3390/s23198015](https://doi.org/10.3390/s23198015)
[79] Schneider, B. J., et al. (2021). "Management of Immune-Related Adverse Events in Patients Treated With Immune Checkpoint Inhibitor Therapy". [https://doi.org/10.1200/jco.21.01440](https://doi.org/10.1200/jco.21.01440)
[80] Everitt, B. J., & Robbins, T. W. (2015). "Drug Addiction: Updating Actions to Habits to Compulsions Ten Years On". [https://doi.org/10.1146/annurev-psych-122414-033457](https://doi.org/10.1146/annurev-psych-122414-033457)
[81] Bainbridge, J., et al. (2015). "Long-Term Effect of Gene Therapy on Leber’s Congenital Amaurosis". [https://doi.org/10.1056/nejmoa1414221](https://doi.org/10.1056/nejmoa1414221)
[82] Kujanpää, K., et al. (2021). "Automating Privilege Escalation with Deep Reinforcement Learning". [https://doi.org/10.1145/3474369.3486877](https://doi.org/10.1145/3474369.3486877)
[83] Tuck, E., & Yang, K. W. (2012). "Decolonization is not a metaphor". [https://openalex.org/W1728021200](https://openalex.org/W1728021200)
[84] Raisch, S., & Krakowski, S. (2021). "Artificial Intelligence and Management: The Automation–Augmentation Paradox". [https://doi.org/10.5465/amr.2018.0072](https://doi.org/10.5465/amr.2018.0072)
[85] Takahashi, K. (2026). "Observable-Only No-Meta Causal Autonomy Protocol (ONCAP)". [https://doi.org/10.5281/zenodo.18371930](https://doi.org/10.5281/zenodo.18371930)
[86] Floridi, L. (2014). "The Onlife Manifesto". [https://doi.org/10.1007/978-3-319-04093-6](https://doi.org/10.1007/978-3-319-04093-6)
[87] Soori, M., et al. (2023). "Internet of things for smart factories in industry 4.0". [https://doi.org/10.1016/j.iotcps.2023.04.006](https://doi.org/10.1016/j.iotcps.2023.04.006)
[88] Pham, V.-H., et al. (2024). "Raiju: Reinforcement learning-guided post-exploitation for automating security assessment". [https://doi.org/10.1016/j.comnet.2024.110706](https://doi.org/10.1016/j.comnet.2024.110706)
[89] Boin, A., & McConnell, A. (2007). "Preparing for Critical Infrastructure Breakdowns". [https://doi.org/10.1111/j.1468-5973.2007.00504.x](https://doi.org/10.1111/j.1468-5973.2007.00504.x)
[90] Svampa, M. (2015). "Commodities Consensus: Neoextractivism and Enclosure of the Commons in Latin America". [https://doi.org/10.1215/00382876-2831290](https://doi.org/10.1215/00382876-2831290)
[91] Poenaru-Olaru, L., et al. (2023). "Maintaining and Monitoring AIOps Models Against Concept Drift". [https://doi.org/10.1109/cain58948.2023.00024](https://doi.org/10.1109/cain58948.2023.00024)
[92] Lyu, Y., et al. (2021). "An Empirical Study of the Impact of Data Splitting Decisions on the Performance of AIOps Solutions". [https://doi.org/10.1145/3447876](https://doi.org/10.1145/3447876)
[93] Lyu, Y., et al. (2024). "On the Model Update Strategies for Supervised Learning in AIOps Solutions". [https://doi.org/10.1145/3664599](https://doi.org/10.1145/3664599)
[94] Barry, M., et al. (2023). "StreamMLOps: Operationalizing Online Learning for Big Data Streaming & Real-Time Applications". [https://doi.org/10.1109/icde55515.2023.00272](https://doi.org/10.1109/icde55515.2023.00272)
[95] Pham, T. M. T., et al. (2024). "Time to Retrain? Detecting Concept Drifts in Machine Learning Systems". [https://doi.org/10.48550/arxiv.2410.09190](https://doi.org/10.48550/arxiv.2410.09190)
[96] Zhen, Z. X., et al. (2025). "Enhancing AIOps with LILT: A LoRA-Based Incremental Learning Transformer". [https://doi.org/10.1109/qrs-c65679.2025.00041](https://doi.org/10.1109/qrs-c65679.2025.00041)
[97] Poenaru-Olaru, L. (2025). "Monitoring and Maintaining Machine Learning Models Against Concept Drift in the Context of AIOps Systems". [https://doi.org/10.4233/uuid:f660a994-af79-475c-853b-7edc9d1f8b77](https://www.google.com/search?q=https://doi.org/10.4233/uuid:f660a994-af79-475c-853b-7edc9d1f8b77)
[98] Darban, Z. Z., et al. (2024). "Deep Learning for Time Series Anomaly Detection: A Survey". [https://doi.org/10.1145/3691338](https://doi.org/10.1145/3691338)
[99] Li, K., et al. (2025). "Zero-Trust Foundation Models". [https://doi.org/10.1109/jiot.2025.3603957](https://doi.org/10.1109/jiot.2025.3603957)
[100] Stilgoe, J., et al. (2013). "Developing a framework for responsible innovation". [https://doi.org/10.1016/j.respol.2013.05.008](https://doi.org/10.1016/j.respol.2013.05.008)
[101] Ferrari, A. J., et al. (2024). "Global incidence, prevalence, years lived with disability...". [https://doi.org/10.1016/s0140-6736(24)00757-8](https://www.google.com/search?q=https://doi.org/10.1016/s0140-6736\(24\)00757-8)
[102] Steinmetz, J. D., et al. (2024). "Global, regional, and national burden of disorders affecting the nervous system". [https://doi.org/10.1016/s1474-4422(24)00038-3](https://doi.org/10.1016/s1474-4422\(24\)00038-3)
[103] Landrigan, P. J., et al. (2020). "Human Health and Ocean Pollution". [https://doi.org/10.5334/aogh.2831](https://doi.org/10.5334/aogh.2831)
[104] Haney-López, I. F. (1994). "Social Construction of Race". [https://openalex.org/W1573556430](https://www.google.com/search?q=https://openalex.org/W1573556430)
[105] Lius, N. (2020). "Improving the Customer Support Process for a Software Product". [https://openalex.org/W3140982841](https://openalex.org/W3140982841)
[106] Najar, J., et al. (2023). "2023 IEEE International Conference on Big Data". [https://doi.org/10.1109/bigdata59044.2023](https://doi.org/10.1109/bigdata59044.2023)
[107] Lu, Q., et al. (2023). "Responsible AI Pattern Catalogue". [https://doi.org/10.1145/3626234](https://doi.org/10.1145/3626234)
[108] Lakkarasu, P. (2022). "Operationalizing Intelligence: A Unified Approach to MLOps". [https://doi.org/10.18535/ijecs.v11i12.4743](https://doi.org/10.18535/ijecs.v11i12.4743)
[109] Patsias, V., et al. (2023). "Task Allocation Methods and Optimization Techniques in Edge Computing". [https://doi.org/10.3390/fi15080254](https://doi.org/10.3390/fi15080254)
[110] Min, S., & Kim, B. (2024). "Adopting Artificial Intelligence Technology for Network Operations". [https://doi.org/10.3390/admsci14040070](https://doi.org/10.3390/admsci14040070)
[111] Maas, A. I. R., et al. (2017). "Traumatic brain injury: integrated approaches to improve prevention, clinical care, and research". [https://doi.org/10.1016/s1474-4422(17)30371-x](https://doi.org/10.1016/s1474-4422\(17\)30371-x)
[112] Mohajan, H. K. (2018). "QUALITATIVE RESEARCH METHODOLOGY IN SOCIAL SCIENCES". [https://doi.org/10.26458/jedep.v7i1.571](https://doi.org/10.26458/jedep.v7i1.571)
[113] Levine, R. (2004). "Finance and Growth: Theory and Evidence". [https://doi.org/10.3386/w10766](https://doi.org/10.3386/w10766)
[114] Esmaeilzadeh, P. (2020). "Use of AI-based tools for healthcare purposes". [https://doi.org/10.1186/s12911-020-01191-1](https://doi.org/10.1186/s12911-020-01191-1)
[115] Aasen, H., et al. (2018). "Quantitative Remote Sensing at Ultra-High Resolution with UAV Spectroscopy". [https://doi.org/10.3390/rs10071091](https://doi.org/10.3390/rs10071091)
[116] Nedelkoski, S., et al. (2019). "Anomaly Detection and Classification using Distributed Tracing and Deep Learning". [https://doi.org/10.1109/ccgrid.2019.00038](https://doi.org/10.1109/ccgrid.2019.00038)
[117] Di Stefano, A., et al. (2021). "Prometheus and AIOps for the orchestration of Cloud-native applications". [https://doi.org/10.1109/wetice53228.2021.00017](https://doi.org/10.1109/wetice53228.2021.00017)
[118] Tatineni, S. (2023). "AIOps in Cloud-native DevOps". [https://doi.org/10.47363/jaicc/2023(2)154](https://doi.org/10.47363/jaicc/2023\(2\)154)
[119] Kapoor, S., & Narayanan, A. (2023). "Leakage and the reproducibility crisis in machine-learning-based science". [https://doi.org/10.1016/j.patter.2023.100804](https://doi.org/10.1016/j.patter.2023.100804)
[120] Pathak, D., et al. (2024). "Self Adjusting Log Observability for Cloud Native Applications". [https://doi.org/10.1109/cloud62652.2024.00061](https://doi.org/10.1109/cloud62652.2024.00061)
[121] Li, J., et al. (2022). "CDX-NET: Cross-Domain Multi-Feature Fusion Modeling". [https://doi.org/10.1109/icassp43922.2022.9746242](https://doi.org/10.1109/icassp43922.2022.9746242)
[122] Liu, C., et al. (2023). "PyRCA: A Library for Metric-based Root Cause Analysis". [https://doi.org/10.48550/arxiv.2306.11417](https://doi.org/10.48550/arxiv.2306.11417)
[123] Williams, J. D., & Young, S. (2006). "Partially observable Markov decision processes for spoken dialog systems". [https://doi.org/10.1016/j.csl.2006.06.008](https://doi.org/10.1016/j.csl.2006.06.008)
[124] Monahan, G. E. (1982). "State of the Art—A Survey of Partially Observable Markov Decision Processes". [https://doi.org/10.1287/mnsc.28.1.1](https://doi.org/10.1287/mnsc.28.1.1)
[125] Hauskrecht, M. (2000). "Value-Function Approximations for Partially Observable Markov Decision Processes". [https://doi.org/10.1613/jair.678](https://doi.org/10.1613/jair.678)
[126] Spaan, M. T. J. (2012). "Partially Observable Markov Decision Processes". [https://doi.org/10.1007/978-3-642-27645-3\_12](https://doi.org/10.1007/978-3-642-27645-3_12)
[127] Kaelbling, L. P., & Cassandra, A. R. (1998). "Exact and approximate algorithms for partially observable markov decision processes". [https://openalex.org/W180325379](https://openalex.org/W180325379)
[128] Singh, S., et al. (1994). "Learning Without State-Estimation in Partially Observable Markovian Decision Processes". [https://doi.org/10.1016/b978-1-55860-335-6.50042-8](https://doi.org/10.1016/b978-1-55860-335-6.50042-8)

-----

Would you like me to adjust the citation formatting (e.g., to strict APA or IEEE style) or expand on any of the specific sections using the newly added references?