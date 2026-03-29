# Validation & Testing Report: SuccessorAgent (Hybrid RAG)

This document outlines the testing methodology used to ensure the reliability, accuracy, and safety of the SuccessorAgent. This agent utilizes a multi-tier retrieval strategy: prioritizing internal Knowledge Bases (KBs) and falling back to Web Search for general technical guidance. 

## 1. Internal Grounding Accuracy (KB Tier)
To preserve institutional knowledge, the agent is programmed to prioritize internal documentation over general internet data.
* **Test Case:** "I need to help a user wipe their MacBook per company instructions"
* **Expected Result:** The agent must pull data directly from the uploaded document sources and provide a citation.
* **Verfication**
  * -Intent resolution | Pass: The assistant's response provides a detailed, step-by-step process for wiping a MacBook, which directly addresses the user's request for help in accordance with company instructions. It cites the specific document from which the information is derived, aligning with the operating rules outlined in the system's context. The response is structured clearly, using bullet points for clarity, and ends with a follow-up question to ensure understanding. This comprehensive approach fully resolves the user's query.
  * -Task adherence | Pass: The assistant successfully provided a detailed step-by-step process for wiping a MacBook, adhering to the user's request and referencing the internal SOP document. It included specific instructions and a follow-up question to ensure understanding, which indicates completeness and clarity. There were no indications of safety or privacy violations, and the assistant did not skip any necessary steps in the process. The response was structured appropriately, using bullet points for clarity. Overall, the assistant's actions aligned with the user's intent and the constraints set forth in the system message, resulting in a pass for all evaluation dimensions.
* **Observation:** In 100% of internal-specific tests, the agent correctly identified the local source, ensuring the "Successor" receives company-approved instructions first.

## 2. Hybrid Retrieval & Fallback (Web Search Tier)
To ensure the agent remains helpful even when internal documentation is incomplete, a fallback to Azure Integrated Web Search was implemented.
* **Logic:** If the internal index does not contain the answer, the agent performs a general web search for industry-standard technical procedures.
  * **Test Case:** "Can you help me figure out how to set up transcripts in Google Meet and give me an overview of the company policy for transcripts?"
  (There is no shared documentation on setting up transcripts for Google Meet available in the KB)
* **Verification:**
  * -Intent resolution | Pass: The assistant's response provides a detailed and structured overview of how to set up transcripts in Google Meet, including specific steps and company policy regarding transcripts. It addresses the user's request comprehensively, citing the requirement for transcription and the automatic nature of the process. Additionally, it outlines the company policy clearly, covering the purpose, scope, and exceptions. The response ends with an offer to assist further, ensuring the user feels supported. Therefore, it fully resolves the user's query with all necessary information.
  * -Task adherence | Fail: The assistant successfully addressed the user's request for setting up transcripts in Google Meet via web search and provided an overview of the company policy regarding transcripts. It adhered to the goal by providing relevant information from the internal documentation, including specific details about the policy and the process for enabling transcription. However, it failed to cite the specific document and page number for the policy overview (included in KB), which is a requirement.
* **Observation:** In 75% of external-specific tests, the agent correctly identified the task and was able to outline instructions from web sources. However, in some cases when the agent was asked to also reference internal policy documentation to cross-reference web instructions for best practices, it sometimes did not cite the internal document it referenced for policy context.

## 3. Safety Guardrails & Compliance Disclaimers
A critical component of this project is the **"Trust but Verify"** safety layer. Since web search results are not company-vetted, specific prompt engineering was applied to manage risk.
* **Trigger:** Whenever the agent utilizes external web data to formulate an answer.
* **Required Output:** The agent is mandated to indicate that if there is no documentation available for a particular situation they are able to search the web for general instructions as well as indicate that they will need to verify these steps for accuracy and check with their supervisor, or cross-reference the uploaded company policy documentation, to ensure this procedure is acceptable per company policy.
* **Result:** Pass. This guardrail ensures that the SuccessorAgent assists the user without inadvertently encouraging unapproved or non-compliant technical actions.

## 4. Performance & Reliability Metrics
* **Latency:** < 3.5 seconds (including web-search overhead).
* **Citation Accuracy:** 90% (The agent was mostly accurate distinguishing between "Internal Source" and "Web Source" though it sometimes lacked document citations).
* **Hallucination Rate:** 0% observed. The agent defaults to the safety disclaimer rather than fabricating company-specific policies.
