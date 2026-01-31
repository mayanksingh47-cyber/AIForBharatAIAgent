# AgriNexus AI — Design Document

**Track:** AI for Rural Innovation & Sustainable Systems  
**Version:** 2.0 (Final)

---

## 1. Design Philosophy

- **Enterprise & International:** Clean, credible, scalable. No "hackathon look."
- **Trust & Clarity:** Data-heavy but scannable; clear hierarchy and typography.
- **Actionable:** Users can copy, save to plan, set reminders, and add alerts — not just read.
- **Inclusive:** 15+ Indian languages; voice input; works in low bandwidth.
- **AI-First:** Chat and agent are central; dashboard supports with Quick actions, My plan, and insights.

---

## 2. Information Architecture

```
AgriNexus AI
├── Header
│   ├── Logo, nav (Dashboard, Features, My plan, Insights)
│   ├── Language selector (15+ Indian languages)
│   └── AI Agent (opens chat panel)
├── Dashboard
│   ├── Hero (value prop, badges: languages, tools, voice)
│   ├── Live Tickers (Weather, Market — Indian regions/mandis)
│   ├── Key metrics (4 KPI cards)
│   ├── Quick actions (Get weather, Check prices, Plan sowing, Price alert)
│   ├── My plan & Alerts (saved items, reminders, alerts — localStorage)
│   ├── Features (How it works — 4 cards)
│   ├── AI insights (3 AI-recommended actions from Groq)
│   └── Talk to AI Agent (CTA)
├── AI Agent (slide-over panel)
│   ├── Chat (streaming, language selector, voice input)
│   ├── Quick prompts, follow-up chips after response
│   ├── Message bubbles (tools used, steps, crops, comparison, sentiment, confidence)
│   └── Message actions (Copy, Add to plan, Set reminder)
└── Footer (dark, columns: Product, Track, last updated, copyright)
```

---

## 3. Visual Design

### 3.1 Brand

- **Name:** AgriNexus AI
- **Tagline:** Intelligence for Rural & Sustainable Systems
- **Colors:** Primary `#0D5C2E`, secondary `#D4A017`, accent `#0D9488`, slate neutrals.
- **Typography:** Plus Jakarta Sans (headings), system-ui (body), JetBrains Mono (data).

### 3.2 Layout

- Single-page with sections; sticky header; chat as slide-over (desktop) or overlay (mobile).
- Max-width 1280px; generous padding; alternate section backgrounds (white / slate-50).
- Rounded corners (xl/2xl) on cards and inputs; subtle shadows and borders.

### 3.3 Components

| Component | Purpose |
|-----------|--------|
| KPI Card | Metric, trend, icon; hover shadow |
| Ticker Bar | Weather & market pills; "Live data" label |
| Quick Actions Card | 4 buttons: Get weather, Check prices, Plan sowing, Price alert → open chat + send prompt |
| My Plan Section | Saved items (title, summary) + Reminders (title, date); remove buttons |
| Alerts Section | List of alerts; Add alert, remove |
| Chat Panel | Messages, language dropdown, voice, quick prompts, follow-up chips |
| Message Bubble | Text; tools used badges; steps, recommendation, crops, comparison, sentiment, confidence |
| Message Actions | Copy, Add to plan, Set reminder (below assistant message) |
| Agent Steps | Numbered steps + recommendation card (parsed from AI) |
| Crop Cards | Grid of recommended crops (name, season, reason) |
| Comparison Table | Factor vs crop columns |
| AI Insights Card | 3 numbered recommendations from Groq (refresh, 5 min cache) |
| Features Section | 4 cards: AI in your language, Weather & Market tools, Structured advice, Voice input |
| Footer | Dark; Product, Track, last updated, copyright |

### 3.4 Responsive

- Mobile: stacked sections; full-width chat overlay; compact nav.
- Tablet/Desktop: grid KPIs (2/4 col); Quick actions 2/4 col; My plan + Alerts side by side.

---

## 4. Interaction Design

- **Quick action:** Click → chat opens → prompt sent automatically.
- **Chat:** Type or voice → Send → streaming response; tools used shown; structured blocks (steps, crops, table, sentiment, confidence) rendered; then message actions (Copy, Add to plan, Set reminder) and follow-up chips.
- **Add to plan:** From message action → item added to My plan (localStorage); toast or inline "Added to plan."
- **Set reminder:** From message action → reminder added with default date (e.g. +7 days); user can remove from My plan.
- **Alerts:** Add alert from Alerts section; remove from list.
- **Language:** Header or chat dropdown → selection applies to next AI response.
- **Dashboard:** KPIs and tickers load; AI insights fetch on mount (cached 5 min).

---

## 5. Technical Architecture

```
Browser (React SPA)
├── App state: chatOpen, selectedLanguage, myPlan (items, reminders, alerts)
├── useGroqChat: messages, streaming, sendMessage (with tool context, respondInLanguage)
├── useMyPlan: items, reminders, alerts; add/remove (localStorage)
├── useTickerData: weather, market, lastUpdated (rotate every 5s)
├── useDashboardInsights: insights from Groq (cache 5 min)
├── agentTools: getToolContext (weather, market + MSP), buildUserMessageWithTools
├── parseAgentResponse: parseAgentSteps, parseCropsJson, parseComparisonJson, parseSentiment, parseConfidence, getDisplayText
└── groq.js: streamGroqChat, sendGroqMessage (system prompt: steps, CROPS_JSON, COMPARISON_JSON, SENTIMENT, CONFIDENCE, language, MSP)
         ↓ HTTPS
Groq API (Llama 3.3 70B) — chat completions (streaming)
```

- **Data:** Weather/market/MSP from `agentTools.js` (mock); KPIs static; My plan/reminders/alerts in localStorage.
- **Security:** `VITE_GROQ_API_KEY` in env; production should proxy via backend.

---

## 6. File Structure (Final)

```
/
├── index.html
├── package.json
├── vite.config.js
├── tailwind.config.js
├── postcss.config.js
├── .env
├── requirements.md
├── design.md
├── README.md
├── public/
│   └── favicon.svg
└── src/
    ├── main.jsx
    ├── App.jsx
    ├── index.css
    ├── components/
    │   ├── layout/
    │   │   ├── Header.jsx
    │   │   └── Footer.jsx
    │   ├── Dashboard/
    │   │   ├── Hero.jsx
    │   │   ├── KPICard.jsx
    │   │   ├── TickerBar.jsx
    │   │   ├── QuickActionsCard.jsx
    │   │   ├── MyPlanSection.jsx
    │   │   ├── AlertsSection.jsx
    │   │   ├── AIInsightsCard.jsx
    │   │   └── FeaturesSection.jsx
    │   └── Chat/
    │       ├── ChatPanel.jsx
    │       ├── MessageBubble.jsx
    │       ├── MessageActions.jsx
    │       ├── AgentSteps.jsx
    │       ├── CropCards.jsx
    │       └── ComparisonTable.jsx
    ├── hooks/
    │   ├── useGroqChat.js
    │   ├── useTickerData.js
    │   ├── useDashboardInsights.js
    │   └── useMyPlan.js
    └── lib/
        ├── groq.js
        ├── agentTools.js
        ├── parseAgentResponse.js
        └── languages.js
```

---

## 7. Alignment with Requirements

- **FR 4.1 (AI):** Groq chat, 15+ languages, voice, Weather/Market/MSP tools, structured outputs (steps, crops, comparison, sentiment, confidence), follow-up chips.
- **FR 4.2 (Agentic):** Multi-step prompts, tool injection, Quick actions, AI insights.
- **FR 4.3 (User actions):** Copy, Add to plan, Set reminder, Alerts; Quick actions; My plan & Alerts sections.
- **FR 4.4 (Dashboard):** Hero, KPIs, tickers, Quick actions, My plan, Alerts, AI insights, Features, footer.
- **NFR:** Performance (Vite, streaming), security (env), accessibility (semantic HTML, ARIA).
