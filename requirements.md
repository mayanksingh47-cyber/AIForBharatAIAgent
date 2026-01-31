# AgriNexus AI — Product Requirements Document (PRD)

**Track:** AI for Rural Innovation & Sustainable Systems  
**Version:** 2.0 (Final)  
**Audience:** CEO-level, International Enterprise Grade

---

## 1. Executive Summary

AgriNexus AI is an AI-powered platform that enhances decision-making, efficiency, and sustainability across rural and agri-value chains. It combines **AI-driven insights** (Groq-powered LLM), **agentic workflows** (multi-step reasoning and tool use), **real-time intelligence** (weather, market, MSP), and **realistic user actions** (save to plan, set reminders, alerts) so users can act on advice, not just read it.

---

## 2. Goals & Success Criteria

| Goal | Success Metric |
|------|----------------|
| Improve farm-level decisions | Adoption of AI recommendations; user satisfaction |
| Reduce resource waste | Water/fertilizer savings; carbon footprint visibility |
| Enhance market linkage | Price transparency; MSP comparison; timely sell recommendations |
| User can take realistic actions | Add to plan, set reminder, copy, add alert; persisted in browser |
| Enterprise readiness | Scalability; security; multi-tenant readiness |
| Real-time value | Live dashboards; sub-minute AI responses; streaming |

---

## 3. User Personas

- **CEO / Agri-Business Leader:** KPIs, sustainability reports, portfolio view.
- **Field Officer / Extension Worker:** Crop advisory, weather, pest alerts; share/save recommendations.
- **Farmer / Cooperative:** What to grow, when to irrigate, where to sell; save advice and set reminders.
- **Policy / NGO:** Impact metrics, regional trends, resource efficiency.

---

## 4. Functional Requirements

### 4.1 AI Features (Must Have)

- **Conversational AI Agent**
  - Natural language queries (crops, market, weather, irrigation, pest, MSP).
  - Context-aware responses using Groq API (streaming, low latency).
  - **All Indian languages:** 15+ languages (English, हिंदी, বাংলা, తెలుగు, मराठी, தமிழ், ગુજરાતી, ಕನ್ನಡ, മലയാളം, ਪੰਜਾਬੀ, ଓଡ଼ିଆ, اردو, অসমীয়া, नेपाली). Language selector in header and chat; AI responds in selected language using native script.
  - **Voice input:** Web Speech API (en-IN, hi-IN, etc.) for hands-free queries.

- **Agent tools (simulated)**
  - **Weather tool:** Region-wise weather, soil moisture, forecast when user asks about weather/irrigation/sowing.
  - **Market tool:** Mandi-wise prices, trend, sentiment when user asks about selling/prices.
  - **MSP (Minimum Support Price):** Govt 2024–25 MSP data injected with market tool; AI mentions MSP vs mandi price when discussing selling.
  - UI shows "Used: Weather" / "Used: Market" badges on responses.

- **Structured outputs**
  - **Agent steps:** STEP 1 / STEP 2 / STEP 3 + RECOMMENDATION parsed and rendered as step cards + recommendation card.
  - **Crop cards:** CROPS_JSON parsed and shown as recommended-crop cards (name, season, reason).
  - **Comparison table:** COMPARISON_JSON (e.g. wheat vs mustard) rendered as table.
  - **Market sentiment:** SENTIMENT: Favorable/Hold/Unfavorable shown as badge.
  - **Confidence:** CONFIDENCE: High/Medium/Low shown as badge when AI provides it.

- **Follow-up suggestions:** After each AI response, show follow-up prompt chips (e.g. "When should I irrigate next?", "Compare with another crop", "What is MSP for this crop?", "Pest control for this crop?").

- **Market intelligence:** Price trends, "best time to sell" guidance, realistic Indian mandis (Azadpur, Vashi, Indore, Rajasthan, Nashik, Rajkot, Guntur, Dharwad), and MSP comparison.

### 4.2 Agentic Features (Must Have)

- **Multi-step reasoning:** "Analyze my farm" → agent uses weather/context → suggests crops → recommendation; steps and recommendation rendered in UI.
- **Task execution:** "Get weather" / "Check prices" → tool context injected → structured summary.
- **Proactive suggestions:** Dashboard AI-recommended actions (3 items from Groq on load); Quick actions (Get weather, Check prices, Plan sowing, Price alert) that open chat and send prompt.

### 4.3 User Actions — Realistic Actions (Must Have)

- **Copy:** Copy full AI response to clipboard from message actions.
- **Add to plan:** Save recommendation or advice to "My plan" (title + summary); persisted in localStorage; visible in Dashboard "My plan" section; user can remove.
- **Set reminder:** Create a reminder (title, date) from an AI response; persisted in localStorage; visible in "My plan" section; user can remove.
- **Alerts:** Add price/weather alert (title, crop, target); persisted in localStorage; "Alerts" section; user can add/remove.
- **Quick actions (dashboard):** One-tap actions that open AI Agent and send a predefined prompt (Get weather, Check prices, Plan sowing, Price alert).

### 4.4 Dashboard & Real-Time (Must Have)

- **CEO Dashboard:** Hero, KPIs (farmers reached, water saved, carbon impact, market linkage), live weather & market tickers, Quick actions card, My plan & Alerts section, AI-recommended actions, Features section.
- **Real-time:** Ticker rotation; last updated timestamp; streaming AI responses.

### 4.5 Non-Functional Requirements

- **Performance:** First contentful paint &lt; 2s; AI response &lt; 5s (Groq).
- **Security:** API key in `.env`; production should proxy via backend.
- **Accessibility:** WCAG 2.1 AA where feasible; keyboard navigation; aria-labels.
- **Scalability:** Stateless frontend; API-ready for future backend.

---

## 5. Out of Scope (V1)

- Full backend auth and multi-tenancy.
- Real soil/weather APIs (mock/simulated).
- Mobile native apps (responsive web only).
- Payments and e-commerce.
- Push notifications for reminders/alerts (browser storage only).

---

## 6. Dependencies

- **Groq API:** Primary LLM (Llama 3.3 70B) for chat and agentic reasoning.
- **Environment:** `VITE_GROQ_API_KEY` in `.env`.
- **Frontend:** Vite, React, Tailwind CSS.
- **Storage:** localStorage for My plan, reminders, alerts.

---

## 7. Acceptance Criteria

- [x] User can open a CEO-grade dashboard with KPIs, tickers, Quick actions, My plan, Alerts, AI insights.
- [x] User can chat with AI agent and receive relevant agri/sustainability advice in 15+ Indian languages.
- [x] Agent uses Weather & Market (with MSP) tools when relevant; badges shown.
- [x] Agent performs multi-step tasks (e.g. "analyze my farm") with clear steps and recommendation.
- [x] User can Copy, Add to plan, Set reminder on any AI response; data persisted and visible in My plan.
- [x] User can add/remove alerts; Quick actions open chat and send prompt.
- [x] Follow-up suggestion chips appear after AI response.
- [x] Dashboard shows real-time tickers and last updated time.
- [x] UI is responsive, professional, and enterprise-ready.
- [x] requirements.md and design.md are complete and aligned with implementation.
