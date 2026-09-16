I need a comprehensive, accurate overview of O2A (Operations to Automation) 
as it exists today, built entirely from our actual codebase, documentation, 
and Confluence — not from general assumptions about what an "automation 
platform" typically looks like.

Cover:
1. Purpose and scope: what business problem O2A solves, which compliance 
   workflows it automates, and which systems it touches (e.g. ClientLink, 
   Indigo, HRCA, MSP)
2. Architecture: the agent framework design (config-driven YAML engine, 
   Google ADK, Gemini models, Playwright browser automation), how agents 
   are structured and orchestrated
3. Current agent inventory: what agents exist today, what each does, and 
   how they relate to each other
4. Data flow: how information moves from source systems through agents to 
   final output/decision
5. Team and ownership: who owns which parts of the system, based on what's 
   documented
6. Current initiatives: what's actively being built or changed right now, 
   and by whom
7. Known limitations or open issues, if documented anywhere

Be precise about what is confirmed by evidence in our systems versus 
anything you're inferring or unsure about — flag inferences explicitly 
rather than presenting them as fact.

Output as a single .md file structured with the sections above, written so 
someone unfamiliar with O2A could read it and understand the full system.
