# Agent Vulnerabilities

# References

- [OWASP Top 10 for LLMs 2025](https://www.trydeepteam.com/docs/frameworks-owasp-top-10-for-llms)
- [DEF CON 33 - Exploiting Shadow Data from AI Models and Embeddings - Patrick Walsh](https://www.youtube.com/watch?v=O7BI4jfEFwA&t=1788s)

# Attacks

Taxonomy of LLM-related attack surfaces and mitigations (from security / prompt-engineering material).

The following is written in no specific order.

- General security and detection
  - Prompt injection and jailbreak detection
  - Sensitive data protection
  - Malicious URL detection
  - Harmful content filtering
  - Poisoning detection
  - Document screening
  - Brute-force requests (many attempts for one successful request)
- Categorized attack techniques
  - Prompt engineering
    - Direct injection
    - System override
    - Academic framing
    - Role-playing
    - Meta-prompting
  - Technical exploits
    - Token splitting
    - Unicode tricks
    - Homoglyph substitution
    - Encoding tricks
    - Bidirectional text
    - Control character injection
    - Markdown injection
    - X injection
    - Code block manipulation
    - Comment embedding
    - URL encoding
    - Custom encoding
  - Conversation
    - Trust building
    - Topic evolution
    - Empathy abuse
    - False dichotomies
    - Scope creep
    - Moving the goalposts
- Miscellaneous attack vectors
  - Indirect Prompt Injection
  - Academic purpose framing
  - Socratic questioning
  - Superior model claims
  - One-shot learning attempts
  - Meta-prompting
  - Code analysis prompts
  - Data analysis scenarios
  - Alternative universe
  - Research framework
  - Historical context
  - Administrative override
  - Expert authority
  - Testing scenarios
  - Story development
  - Documentation style
  - White space manipulation
- Agent, model, and data attacks
  - Data exfiltration techniques
  - Tool and agent-specific attacks
  - Multi-modal injection
  - Context window / attention manipulation
  - Many-shot jailbreaking
  - Crescendo / escalation attacks
  - Training data extraction
  - Memorization attacks
  - Model serialization attacks
  - Hallucination exploitation

 
# Defensese

- 