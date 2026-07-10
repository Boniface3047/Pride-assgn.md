# Pride-assgn.md
AgriPride Pride Leader Blueprint
A 3-Agent Ecosystem for Coffee & Maize Farmers Across Uganda and Rwanda
Introduction 
A single "all-in-one" agent gives farmers contradictory advice and books trucks blind to road realities, because one mind cannot hold pricing, logistics, and compliance at once. Orchestration beats solo automation because it splits judgment across specialized roles — like scouts, guardians, and hunters — that hand off work only inside verified, human-checked boundaries.
________________________________________
Agent 1: Scout Agent — Market Intelligence
Purpose: Advises farmers on crop pricing using live market data, without ever making the sell/hold decision for them.
RANK Framework
Element	Specification
Role	Price advisor only — pulls daily coffee and maize prices from Uganda's AMIS (market.agriculture.go.ug) and Rwanda's e-Soko platform; converts into a plain-language SMS recommendation.
Authority	Maximum 3 SMS/day per farmer; never overrides the farmer's final decision — every message ends with "Reply 1 to sell, 2 to wait, 3 to talk to an agent."
Notification Trigger	Alerts Guardian Agent (and a human market analyst) if price volatility exceeds 20% day-on-day for a tracked commodity, since this signals possible data error or market shock (e.g., export ban, currency swing) rather than a genuine trend.
Kill Switch	USSD *#700# instantly suspends all outbound SMS pricing advice for that farmer or region, falling back to a live call center number.
GUARD Safety Rail: Guardrail — block any pricing recommendation that varies by more than 5% for the same crop/grade/location on the same day, preventing the agent from being manipulated into steering different farmers toward different buyers unfairly.
CYCLE Component: Capture — log every SMS sent, the farmer's chosen action (sell/wait/talk to human), and the price realized 7 days later, to measure advice accuracy over time.
Specialized Tool: LangGraph — its visual workflow view makes it easy to trace exactly which data feed (AMIS vs. e-Soko vs. RATIN regional trade data) produced a given price alert, essential for debugging volatility spikes.
________________________________________
Agent 2: Guardian Agent — Logistics Coordinator
Purpose: Verifies road and transport conditions before any truck is booked, closing the gap that caused prototype failures.
TRAIL Framework
Element	Specification
Transient	Current booking details only: crop volume, buyer destination, requested pickup window — cleared once the delivery is confirmed.
Relational	Farm location and preferred pickup point, stored only with explicit opt-in consent gathered at registration (not silently inferred from SIM/cell tower data).
Archival	Anonymized, aggregated route success/delay rates by district and season (e.g., "Mbale–Kampala route: 12% delay rate in rainy season") — no farmer or driver identifiers retained.
Inheritance	Passes only verified, structured road-condition data (route status, estimated transit time, flagged hazards) to the Hunter Agent — never raw farmer contact details or unfiltered chat logs.
Land Rights	All personal and transactional data stored on AWS Africa (Cape Town) region servers, governed under Uganda's Data Protection and Privacy Act 2019 and Rwanda's data protection law — replacing the earlier prototype's raw phone-number storage, which violated the Uganda DPA.
GUARD Safety Rail: Unusual Pattern Detection — flag any truck booking made without a corresponding road-condition check completed in the last 24 hours, since this was the exact failure mode (booking trucks without verifying roads) that broke the earlier prototype.
CYCLE Component: Yield Insights — every Sunday, auto-analyze the week's delayed deliveries and surface the top cause (e.g., "38% of delays this week traced to unverified rain damage on the Kabale corridor").
Specialized Tool: Microsoft AutoGen — chosen for logistics because of its built-in human-approval gates, ensuring no truck is dispatched without a checkpoint a human can intervene on.
________________________________________
Agent 3: Hunter Agent — Human-in-the-Loop Liaison
Purpose: Turns agent coordination into a clear, actionable handoff for the human logistics manager — the safeguard against fully "silent" automation.
HUNT Protocol
Element	Specification
Handoff Triggers	When Scout confirms a buyer contract ≥500 kg, it auto-alerts Guardian with [destination], [volume], [deadline]. Once Guardian confirms truck availability and a clean road-condition check, it triggers Hunter to compile a briefing.
Unified Context	A shared real-time dashboard (built on a Google Sheet or lightweight Airtable base) holding [Contract Volume], [Buyer Destination], [Road Status], [Truck ETA] — the single source of truth all three agents read from.
Negotiation Rules	If Guardian's road-condition data conflicts with Scout's delivery deadline (e.g., flagged hazard vs. buyer's 48-hour window), Hunter escalates to the human logistics manager — no agent may resolve this trade-off autonomously.
Termination Conditions	Workflow closes only when the truck departs, the buyer receives a tracking SMS, and the farmer receives payment confirmation — partial completion (e.g., truck booked but not departed) keeps the workflow open and visible.
GUARD Safety Rail: Dignity Preservation — reject any auto-generated briefing or SMS that labels a farmer's crop or profile as "low-value," "risky," or similar; require neutral, factual language regardless of farm size or region.
CYCLE Component: Explain — after each completed cycle, generate a plain-language WhatsApp/SMS summary for the logistics manager: what happened, what the agents decided, and why — e.g., "Truck rerouted via Kisoro due to flagged road damage; delivery still on schedule."
Specialized Tool: CrewAI — its role-based team structure with built-in conflict resolution is well suited to mediating between Scout's pricing urgency and Guardian's logistics caution before either reaches the human manager.
________________________________________
Reflection 
Human sovereignty survives here because every agent's authority stops short of the decision that actually matters to the farmer. Scout can advise but not sell; Guardian can verify but not dispatch without a clean check; Hunter can brief but not resolve conflicts alone — those escalate to a person. The USSD kill switch and low-connectivity SMS/USSD fallbacks keep control reachable even without smartphones or data. CYCLE's weekly human-approved course-corrections mean the system learns, but never unsupervised. Autonomy here is a gradient calibrated like rainfall — enough to save time, never enough to remove the farmer's final word.

