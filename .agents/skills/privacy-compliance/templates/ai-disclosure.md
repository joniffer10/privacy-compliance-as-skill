# AI Feature Disclosure & Transparency Notice Template

> **Instructions for Agent**: Customize this document if the application integrates Large Language Models (LLMs), AI text/image generation, embeddings, or automated decision-making engines.

---

# Artificial Intelligence (AI) Transparency & Usage Disclosure

**Application Name:** [APPLICATION_NAME]  
**Last Updated:** [LAST_UPDATED_DATE, e.g. January 15, 2026]

This disclosure explains how artificial intelligence technologies are utilized within **[APPLICATION_NAME]**, how your data is processed by underlying machine learning models, and the safeguards and limitations applicable to AI-assisted features.

---

## 1. Overview of AI-Powered Features

The following capabilities within [APPLICATION_NAME] utilize artificial intelligence:

| Feature Name | Primary Purpose | Underlying Model Provider | Data Sent to Model |
| :--- | :--- | :--- | :--- |
| **[FEATURE_1, e.g. Smart Assistant / Summarizer]** | [PURPOSE, e.g. Generates concise summaries of user notes] | [PROVIDER, e.g. OpenAI GPT-4o via API] | User-selected text snippet and instruction prompt |
| **[FEATURE_2, e.g. Code Assistant]** | [PURPOSE, e.g. Suggests automated refactorings] | [PROVIDER, e.g. Anthropic Claude 3.5 Sonnet] | Active code block and user query |

---

## 2. Model Training & Data Privacy

We prioritize user confidentiality and data integrity when integrating external AI models:

* **No Foundation Model Training:** We access AI models through commercial enterprise API endpoints. Under our agreements with model providers ([AI_PROVIDERS, e.g. OpenAI / Anthropic]), **your inputs, prompts, and generated responses are NOT used to train, retrain, or improve public foundation models**.
* **Data Retention by Model Providers:** Prompts and responses are retained by model providers solely for transient processing and abuse monitoring (typically up to 30 days under zero-data-retention or standard enterprise security terms), after which they are deleted.
* **Confidentiality:** Do not submit sensitive personal identifiers, unhashed credentials, or proprietary trade secrets into open prompt fields.

---

## 3. Accuracy, Hallucinations, and Limitations

* **Probabilistic Nature:** AI models generate responses based on statistical language patterns. Outputs may occasionally contain factual errors, outdated information, or hallucinations.
* **Verification Responsibility:** AI-generated outputs are intended as drafting aids and creative suggestions. You are responsible for reviewing, testing, and confirming the correctness of any AI output before relying on it for mission-critical tasks.
* **No Professional Certification:** AI features do not provide certified legal, medical, financial, tax, or engineering safety advice.

---

## 4. Human Oversight and Controls

* **User-Initiated Triggering:** AI features are triggered only upon explicit user action (e.g., clicking "Generate Summary" or submitting a prompt). No automatic automated decisions affecting user account standing are made without human review.
* **Opt-Out & Feature Disabling:** [Developer action required: Explain if users can toggle AI features on/off in workspace settings, e.g. "Workspaces can disable AI-assisted features under Workspace Settings > Features > AI Toggle"].

---

## 5. Contact & Feedback

If you experience unexpected behavior, inaccurate outputs, or have privacy questions regarding our AI implementation, please reach out to:
* **Email:** [Developer action required: Insert support email, e.g. ai-support@example.com]
