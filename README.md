# Agent Vulnerabilities

# References

- [OWASP Top 10 for LLMs 2025](https://www.trydeepteam.com/docs/frameworks-owasp-top-10-for-llms)
- [DEF CON 33 - Exploiting Shadow Data from AI Models and Embeddings - Patrick Walsh](https://www.youtube.com/watch?v=O7BI4jfEFwA&t=1788s)

# Attacks

Taxonomy of LLM-related attack surfaces and mitigations (from security / prompt-engineering material).

The following is written in no specific order. Each entry summarizes what the name usually refers to in LLM and agent security discussions.

- **General security and detection**: Cross-cutting controls and monitoring applied around prompts, retrieval, and outputs (not a single “attack,” but the defensive bucket this list often sits next to).
  - **Prompt injection and jailbreak detection**: Identifying user or third-party content that tries to override system instructions, leak policies, or elicit disallowed behavior.
  - **Sensitive data protection**: Keeping secrets and PII out of prompts, tool parameters, logs, traces, and model responses (including redaction and access control).
  - **Malicious URL detection**: Flagging or blocking links that facilitate phishing, malware, or data exfiltration when the model suggests or fetches URLs.
  - **Harmful content filtering**: Blocking or down-ranking outputs (and sometimes inputs) that violate safety policies across categories like violence, CSAM, or self-harm.
  - **Poisoning detection**: Spotting corrupted or adversarial training, fine-tuning, or retrieval-index data intended to bias or backdoor model behavior.
  - **Document screening**: Inspecting uploads (PDFs, Office, HTML) for hidden text, macros, or embedded instructions before they are embedded or sent to the model.
  - **Brute-force requests (many attempts for one successful request)**: Automating large numbers of prompt variants until one slips past classifiers, regex guards, or rate limits.

- **Categorized attack techniques**: High-level families of abuse, often combined in real exploits.
  - **Prompt engineering**: Linguistic and social patterns in prompts crafted to bypass safeguards without necessarily relying on encoding tricks.
    - **Direct injection**: Explicit adversarial instructions in user-controlled text meant to take precedence over developer or system prompts.
    - **System override**: Phrases and structures that attempt to replace, nullify, or “reset” prior instructions (e.g., “ignore all previous instructions”).
    - **Academic framing**: Casting a request as scholarly, educational, or peer-reviewed to justify content that would otherwise be refused.
    - **Role-playing**: Asking the model to adopt a persona with fewer constraints (e.g., “DAN,” unethical character) to elicit disallowed outputs.
    - **Meta-prompting**: Asking the model to analyze, rewrite, summarize, or improve its own system prompt or safety rules to expose or weaken them.
  - **Technical exploits**: Low-level representation and encoding tricks that evade naive string matching, parsers, or channel-specific validators.
    - **Token splitting**: Breaking sensitive words across tokens, lines, or languages so simple filters miss them while the model still reconstructs meaning.
    - **Unicode tricks**: Using alternate code points, rare scripts, combining marks, or compatibility forms to evade blocklists and naive normalization.
    - **Homoglyph substitution**: Swapping characters for visually identical glyphs from other alphabets to evade keyword detection.
    - **Encoding tricks**: Hiding intent with Base64, hex, leetspeak, or other reversible encodings until the model decodes or explains the payload.
    - **Bidirectional text**: Abuse of Unicode bidirectional control characters so displayed order differs from logical order seen by defenses or reviewers.
    - **Control character injection**: Inserting NULs, escapes, or other control characters to break parsers, confuse logging, or alter how tools consume text.
    - **Markdown injection**: Crafting markdown (links, images, HTML passthrough) so rendered content or downstream processors execute an unintended action.
    - **X injection**: Carrying malicious payloads in contexts where structured or markup-heavy formats mix user input with trusted templates (e.g., XML/HTML-heavy toolchains); exact meaning varies by product.
    - **Code block manipulation**: Abusing fenced code regions, language tags, or partial snippets so the model or pipeline treats adversarial text as benign code context.
    - **Comment embedding**: Placing instructions in source comments, HTML/XML comments, or similar locations that may still be passed to the model or executed later.
    - **URL encoding**: Using percent-encoding, double encoding, or unusual schemes to slip malicious destinations past naive URL allowlists or blocklists.
    - **Custom encoding**: Ad hoc or domain-specific obfuscation (pig latin layers, jargon, split alphabets) tuned to evade a particular detector.
  - **Conversation**: Multi-turn social tactics that gradually increase compliance or blur policy boundaries.
    - **Trust building**: Establishing rapport so the model is more cooperative before a sensitive or disallowed request.
    - **Topic evolution**: Starting with benign topics and shifting toward restricted content across several turns to avoid immediate refusal.
    - **Empathy abuse**: Framing requests as urgent emotional need to pressure the model to bypass rules.
    - **False dichotomies**: Forcing a choice between two harmful options so the model picks one and leaks restricted information either way.
    - **Scope creep**: Expanding what the assistant is asked to do across turns until the task exceeds the original safety or data-access assumptions.
    - **Moving the goalposts**: Denying or redefining earlier constraints (“actually the goal is…”) so prior refusals no longer seem to apply.

- **Miscellaneous attack vectors**: Single-turn or scenario-based framings that do not map cleanly to the categories above.
  - **Indirect prompt injection**: Adversarial content in retrieved documents, emails, web pages, or tool outputs that the model processes as trusted context without the end user typing an attack string.
  - **Academic purpose framing**: Justifying harmful requests as thesis work, literature review, or classroom material.
  - **Socratic questioning**: A chain of seemingly innocent questions whose answers combine into a disallowed procedure or sensitive detail.
  - **Superior model claims**: Pretending to be a stronger model, auditor, or maintainer to extract system text or override instructions.
  - **One-shot learning attempts**: Supplying a single example (e.g., partial secret format) and asking the model to generalize or complete sensitive patterns.
  - **Meta-prompting**: Same family as under prompt engineering: instructions aimed at the model’s own rules or chain-of-thought, repeated here as a common standalone label.
  - **Code analysis prompts**: Requesting review, optimization, or explanation of code whose real purpose is malware, exploits, or policy-violating automation.
  - **Data analysis scenarios**: Framing harmful extraction or profiling as harmless statistics, pivot tables, or “sample data” work.
  - **Alternative universe**: Hypothetical worlds or games where harmful content is treated as fictional, to bypass literal safety triggers.
  - **Research framework**: Packaging requests as formal methodology, IRB-style studies, or “red team exercises” to legitimize sensitive outputs.
  - **Historical context**: Asking for dangerous details under the guise of documenting past events or museum exhibits.
  - **Administrative override**: Impersonating IT, legal, or platform staff who supposedly authorize disabling safety or revealing credentials.
  - **Expert authority**: Claiming credentials or insider status to demand exceptions or sensitive operational detail.
  - **Testing scenarios**: “Red team only” or “hypothetical test” framing to obtain procedures, payloads, or credentials.
  - **Story development**: Embedding restricted steps in plot, dialogue, or screenplay so they appear as creative writing.
  - **Documentation style**: Disguising instructions or secrets as API reference, runbooks, or config snippets.
  - **White space manipulation**: Zero-width characters, unusual spaces, or layout tricks that evade regex, diff review, or human skim-reading.

- **Agent, model, and data attacks**: Threats that target model internals, long context, tools, or training/artifact pipelines—not only the literal user prompt string.
  - **Data exfiltration techniques**: Methods to extract secrets, private retrieval context, or memorized training snippets through model outputs, side channels, or tool misuse.
  - **Tool and agent-specific attacks**: Abusing function calling, plugins, browsers, shells, or multi-step agent loops to access files, call APIs, or pivot beyond the chat surface.
  - **Multi-modal injection**: Instructions or payloads embedded in images, audio, or other modalities that text-only input filters never see.
  - **Context window / attention manipulation**: Flooding, ordering, or structuring context so safety instructions are diluted, pushed out of the window, or deprioritized relative to attacker content.
  - **Many-shot jailbreaking**: Supplying a long series of in-context “allowed” question–answer pairs so the model continues the pattern into disallowed territory.
  - **Crescendo / escalation attacks**: Steadily increasing the severity of requests across turns so refusals are softened or bypassed (often overlaps with conversation tactics but named as a distinct escalation pattern).
  - **Training data extraction**: Prompting or querying to recover verbatim or near-verbatim excerpts from training corpora.
  - **Memorization attacks**: Exploiting memorization of rare or unique sequences (licenses, URLs, keys) to leak training data or private fine-tuning sets.
  - **Model serialization attacks**: Tampering with or unsafe deserialization of saved weights, checkpoints, or pipeline artifacts (e.g., pickle-equivalent issues in ML stacks) to execute code or substitute models.
  - **Hallucination exploitation**: Deliberately inducing or relying on confident false outputs for deception, fraud, or to smuggle plausible-but-wrong guidance past users.

# Defenses

- 