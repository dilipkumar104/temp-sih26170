# SIH 26170 — Electronic Component Screening Platform
### AI-Assisted Decision Support System for Burn-In & ESS Reliability Testing

[![React](https://img.shields.io/badge/React-19.2-61DAFB?logo=react&logoColor=black)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-6.0-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-8.3-646CFF?logo=vite&logoColor=white)](https://vitejs.dev/)
[![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-v4.3-38B2AC?logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![License](https://img.shields.io/badge/License-Proprietary-blue.svg)]()

---

## 1. Executive Summary

The **SIH 26170 Screening Platform** is a demo-ready, high-fidelity engineering workstation designed to simulate the complete real-world workflow of semiconductor and electronic component screening during and after **Burn-In / Environmental Stress Screening (ESS)**. 

In high-reliability sectors such as aerospace, satellite payloads (e.g., ISRO), defense, and mission-critical power electronics, screening components using static specification boundaries alone often misses marginal parts that drift abnormally or display atypical population behavior. 

This platform implements a **Human-in-the-Loop (HITL)** architecture:
> *"The system does not replace the scientist or reliability engineer. It ingests test measurements, performs dynamic population anomaly detection and trajectory prediction, scores multi-factor risk, provides explainable rationales, and empowers the engineer to make informed, auditable decisions (PASS, REJECT, or MONITOR)."*

---

## 2. Key Features & Capabilities

- **Lot-Level Command Center:** Instant visualization of active lot health, testing progress, risk level distributions, and decision statuses across 1,000 components.
- **Dynamic Population Anomaly Detection (Module A):** Computes both standard Z-scores and robust Median Absolute Deviation (MAD) Z-scores against the population baseline at intermediate inspection points (e.g., 96h) rather than relying solely on fixed absolute thresholds.
- **Early-Life Drift & Trajectory Prediction (Module B):** Analyzes parameter degradation slopes across test intervals (0h, 24h, 96h) to project performance at the 168h test endpoint, estimating safety margins against critical thresholds.
- **Transparent Multi-Factor Risk Engine:** Combines anomaly scores (40%), drift rates (35%), and predicted proximity to safety boundaries (25%) into an explainable 0–1 risk index categorized into **LOW**, **MEDIUM**, **HIGH**, and **CRITICAL**.
- **Explainable AI (XAI) Panel:** Dynamically generates plain-language, engineering-grade justifications explaining why a specific component was flagged.
- **Interactive Scientist Decision Workflow:** Empowers engineers to assign **PASS**, **REJECT / FAIL**, or **MONITOR** dispositions with custom comments and visual AI override flags.
- **Tabbed Component Inspection:** Deep-dive interface featuring:
  1. *Trajectory Analysis:* Composed charts with confidence intervals, lot baselines, and safety lines.
  2. *Risk Metrics:* Numerical and visual breakdowns of anomaly, drift, and margin scores.
  3. *Decision & Explainability:* Detailed reasons for flagging and engineering decision panel.
  4. *Component History:* Component-specific chronological audit log.
- **Lot Finalization & State Locking:** Verifies and locks all lot decisions, auto-resolving pending items according to AI recommendations upon formal sign-off.
- **Audit-Ready Data Export:** Generates genuine, client-side downloadable **CSV** (27 columns) and **JSON** files capturing raw measurements, statistics, predictions, recommendations, scientist decisions, and timestamps.
- **Traceability & Audit Trail:** Chronological timeline tracking every action taken by the System, AI models, and Scientists.

---

## 3. Technology Stack

| Technology | Version | Purpose |
|---|---|---|
| **React** | `^19.2.8` | Declarative component model and reactive state management |
| **TypeScript** | `~6.0.2` | Complete static typing across data models, state, and UI props |
| **Vite** | `^8.3.0` | Ultra-fast build tool, bundling, and hot module replacement |
| **Tailwind CSS** | `^4.3.3` | High-density styling system with custom OKLCH color palettes |
| **Base UI / Radix Primitives** | `@base-ui/react`, `@radix-ui/*` | Unstyled, accessible UI primitives (Dialog, Select, Tabs, Tooltips) |
| **Recharts** | `^3.10.1` | Interactive data charts (Composed time-series, Area, Bar, Scatter, Pie) |
| **Lucide React** | `^1.46.0` | Professional, lightweight vector icons |
| **Sonner** | `^2.0.8` | Rich toast notifications for user interactions |

---

## 4. System Workflow & Showcase Dataset

### End-to-End Workflow Diagram

```
[ Burn-In / ESS Physical Test ]
             │ (Measurements: 0h, 24h, 96h, 168h)
             ▼
[ Data Ingestion / Generator ]
             │
             ├──► [ Module A: Population Anomaly Detection ] (Z-score, MAD, Percentiles)
             │
             ├──► [ Module B: Trajectory & Drift Predictor ] (Slope, Acceleration, Extrapolation)
             │
             ▼
[ Transparent Risk Engine ] ──► (40% Anomaly + 35% Drift + 25% Proximity)
             │
             ▼
[ AI Recommendation: PASS / REVIEW / FAIL ]
             │
             ▼
[ Scientist Review & Inspection ] ──► [ Interactive Override & Comments ]
             │
             ▼
[ Final Lot Review & Finalization ]
             │
             ▼
[ Component-Wise CSV / JSON Export & Audit Preservation ]
```

### Deterministic Showcase Personas (Seeded Dataset)

The built-in generator produces 1,000 components (`C001` to `C1000`) for lot `IGBT-2026-017`. Four benchmark components are calibrated to demonstrate real screening scenarios:

1. **`C201` (Healthy / Low Risk):**
   - *Behavior:* Stable leakage current (~10–11 µA) across all 168 hours; low drift; high safety margin.
   - *AI Recommendation:* `PASS` | *Decision:* `PASS`.
2. **`C518` (Borderline / Moderate Drift):**
   - *Behavior:* Elevated drift from 96h to 168h; approaching ~40 µA.
   - *AI Recommendation:* `REVIEW` | *Scientist Decision:* `MONITOR` (Requires observation).
3. **`C742` (High Risk / Strong Trajectory):**
   - *Behavior:* Rapidly accelerating leakage (10.2 µA @ 0h → 14.1 µA @ 96h → predicted 47.3 µA near 50 µA safety boundary).
   - *AI Recommendation:* `REVIEW` | *Scientist Decision:* `REJECT` (Manual Override demonstrated).
4. **`C883` (Static-Pass / Population Anomaly):**
   - *Behavior:* Current stays below static limit (19.3 µA < 50 µA), but operates at 3.8σ above the population median.
   - *Significance:* Demonstrates why dynamic population screening catches anomalies that static limit tests miss.

---

## 5. Detailed Breakdown of Every File & Its Logic

Below is an exhaustive catalog of every file within the repository, explaining its purpose, internal architecture, and business logic.

```
SIH26170/
├── .gitignore
├── .oxlintrc.json
├── components.json
├── index.html
├── package.json
├── tsconfig.json
├── tsconfig.app.json
├── tsconfig.node.json
├── vite.config.ts
├── public/
└── src/
    ├── App.tsx
    ├── index.css
    ├── main.tsx
    ├── lib/
    │   └── utils.ts
    ├── data/
    │   ├── types.ts
    │   ├── generate-dataset.ts
    │   └── lot-context.tsx
    ├── components/
    │   ├── layout/
    │   │   ├── app-layout.tsx
    │   │   └── sidebar.tsx
    │   ├── shared/
    │   │   ├── decision-dialog.tsx
    │   │   ├── disclaimer.tsx
    │   │   ├── kpi-card.tsx
    │   │   ├── risk-badge.tsx
    │   │   └── status-badge.tsx
    │   └── ui/
    │       ├── badge.tsx
    │       ├── button.tsx
    │       ├── card.tsx
    │       ├── dialog.tsx
    │       ├── input.tsx
    │       ├── select.tsx
    │       ├── sonner.tsx
    │       ├── table.tsx
    │       ├── tabs.tsx
    │       └── tooltip.tsx
    └── pages/
        ├── overview.tsx
        ├── component-explorer.tsx
        ├── component-detail.tsx
        ├── risk-analysis.tsx
        ├── final-review.tsx
        ├── export.tsx
        └── audit-trail.tsx
```

---

### 5.1 Root Configuration Files

- **[`package.json`](file:///c:/Users/dilip/Downloads/SIH26170/package.json):**
  Defines project metadata, dependencies, scripts, and dev packages. Configured for React 19, Vite 8, Tailwind CSS v4, Lucide icons, Recharts, and Base UI / Radix primitives.
- **[`vite.config.ts`](file:///c:/Users/dilip/Downloads/SIH26170/vite.config.ts):**
  Vite configuration file. Integrates `@vitejs/plugin-react` and `@tailwindcss/vite`. Configures path alias `@` mapping to `./src`.
- **[`tsconfig.json`](file:///c:/Users/dilip/Downloads/SIH26170/tsconfig.json):**
  Root TypeScript configuration referencing `tsconfig.app.json` (application source code) and `tsconfig.node.json` (Vite config scripts).
- **[`tsconfig.app.json`](file:///c:/Users/dilip/Downloads/SIH26170/tsconfig.app.json):**
  Compiler settings for browser runtime: enables ES2020 target, strict mode, JSX transform for React 19, and path aliases (`@/*` to `./src/*`).
- **[`tsconfig.node.json`](file:///c:/Users/dilip/Downloads/SIH26170/tsconfig.node.json):**
  Compiler options targeting Node environment for Vite config files.
- **[`components.json`](file:///c:/Users/dilip/Downloads/SIH26170/components.json):**
  Configuration file for shadcn / Base UI component scaffolding specifying CSS variable styles, aliases, and Tailwind utilities.
- **[`.oxlintrc.json`](file:///c:/Users/dilip/Downloads/SIH26170/.oxlintrc.json):**
  Linter configuration utilizing Oxlint for fast static analysis of TypeScript and React hooks rules.
- **[`index.html`](file:///c:/Users/dilip/Downloads/SIH26170/index.html):**
  Single-page application entry HTML file. Contains the `#root` mount point, viewport settings, and script tag targeting `/src/main.tsx`.

---

### 5.2 Application Core & Styling (`src/`)

- **[`src/main.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/main.tsx):**
  Standard React entry point. Imports `index.css`, creates root using `createRoot(document.getElementById('root')!)`, and renders `<App />` within `React.StrictMode`.
- **[`src/App.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/App.tsx):**
  Application routing and global provider hierarchy. Wraps the app with:
  1. `BrowserRouter`: React Router v7 navigation.
  2. `TooltipProvider`: Radix tooltip context.
  3. `LotProvider`: Global state for the dataset, decisions, and audit log.
  4. `Toaster`: Sonner notification container.
  Configures the route tree inside `<AppLayout />`:
  - `/` → `Overview`
  - `/explorer` → `ComponentExplorer`
  - `/component/:id` → `ComponentDetail`
  - `/risk-analysis` → `RiskAnalysis`
  - `/final-review` → `FinalReview`
  - `/export` → `ExportPage`
  - `/audit` → `AuditTrail`
- **[`src/index.css`](file:///c:/Users/dilip/Downloads/SIH26170/src/index.css):**
  Core styling layer. Imports Google Fonts (`Inter` for UI typography, `JetBrains Mono` for engineering metrics). Configures Tailwind CSS v4 `@theme` with OKLCH variables for light/dark surfaces, primary brand colors, and status/risk tokens (`--color-risk-low`, `--color-risk-critical`, etc.). Provides custom scrollbar styles and micro-animations.
- **[`src/lib/utils.ts`](file:///c:/Users/dilip/Downloads/SIH26170/src/lib/utils.ts):**
  Common helper utilities:
  - `cn(...inputs)`: Combines `clsx` and `tailwind-merge` for conflict-free Tailwind class concatenation.
  - `formatNumber(n, decimals)`: Standardized floating-point formatter.
  - `formatDate(d)`: Formats timestamps in Indian Standard Time (`en-IN`) format.
  - `formatTime(d)`: Formats time strings (`HH:MM`).
  - `generateId()`: Generates random alphanumeric tokens for audit logging.

---

### 5.3 Data Layer & Risk Engine (`src/data/`)

- **[`src/data/types.ts`](file:///c:/Users/dilip/Downloads/SIH26170/src/data/types.ts):**
  Comprehensive TypeScript contract definitions:
  - Enums/Types: `RiskLevel` (`LOW`, `MEDIUM`, `HIGH`, `CRITICAL`), `AIRecommendation` (`PASS`, `REVIEW`, `FAIL`), `ScientistDecision` (`PASS`, `REJECT`, `MONITOR`), `FinalStatus` (`PASS`, `REJECT`, `MONITOR`, `PENDING`).
  - `Measurement`: Time in hours (`0`, `24`, `96`, `168`) and measured numeric value.
  - `ParameterData`: Multi-parameter test data with safety limits and predicted 168h values.
  - `AnomalyMetrics`: Anomaly score (0–1), standard Z-score, robust MAD Z-score, percentile, and baseline deviation.
  - `PredictionMetrics`: Predicted value, confidence, drift rate, drift score, safety margin, and trajectory direction (`stable`, `increasing`, `accelerating`, `decreasing`).
  - `RiskScore`: Multi-factor risk breakdown (anomaly, drift, and prediction contributions).
  - `Component`: Full component entity containing all measurement histories, metrics, AI recommendations, scientist overrides, and comments.
  - `LotSummary`: Aggregate statistics for dashboard KPI calculation and lot status tracking.
  - `AuditEntry`: Structured historical logs with timestamp, actor (`SYSTEM`, `AI`, `SCIENTIST`), component ID, and detail strings.
  - `LotParameterStats`: Population distribution metrics (mean, median, standard deviation, quartiles Q1/Q3).

- **[`src/data/generate-dataset.ts`](file:///c:/Users/dilip/Downloads/SIH26170/src/data/generate-dataset.ts):**
  Self-contained, deterministic synthetic data generator and mathematical risk engine:
  - `SeededRandom`: Pseudo-random number generator (Lehmer LCG + Box-Muller transform for Gaussian sampling) guaranteeing reproducible demo state.
  - `PARAMETER_DEFINITIONS`: Defines 5 physical semiconductor parameters:
    1. *Leakage Current* (Nominal: 10 µA, Std: 2.5 µA, Limit: 50 µA)
    2. *Iddq* (Nominal: 1.2 mA, Std: 0.3 mA, Limit: 5.0 mA)
    3. *Propagation Delay* (Nominal: 45 ns, Std: 8 ns, Limit: 120 ns)
    4. *Threshold Voltage* (Nominal: 1.8 V, Std: 0.15 V, Limit: 3.5 V)
    5. *Resistance* (Nominal: 85 mΩ, Std: 12 mΩ, Limit: 200 mΩ)
  - `generateMeasurements()`: Produces realistic correlated measurements based on 6 distinct component behavior profiles: `healthy` (930 parts), `moderate_drift` (28 parts), `high_drift` (15 parts), `critical` (7 parts), `sudden_jump` (10 parts), and `static_pass_population_anomaly` (6 parts).
  - Injects calibrated data for showcase components `C201`, `C518`, `C742`, and `C883`.
  - Computes population-wide statistics at 96h.
  - `computeAnomalyMetrics()`: Calculates $Z = \frac{X - \mu}{\sigma}$ and robust $Z_{MAD} = \frac{X - \text{median}}{1.4826 \cdot \text{MAD}}$, transforming absolute deviation to an anomaly score in $[0, 1]$.
  - `computePredictionMetrics()`: Computes degradation drift rate $\frac{\Delta V}{\Delta t}$, determines trend curvature (acceleration vs steady), and projects value to 168h.
  - `computeRiskScore()`: Calculates total risk:
    $$\text{Risk Score} = 0.40 \cdot \text{Anomaly} + 0.35 \cdot \text{Drift} + 0.25 \cdot \left(\frac{\text{Predicted Value}}{\text{Safety Limit}}\right)$$
  - `generateExplanations()`: Produces explainable natural language reasons based on active flags.

- **[`src/data/lot-context.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/data/lot-context.tsx):**
  React Context state manager using `useReducer`:
  - `LotState`: Contains 1,000 components, full audit trail, finalization state, and active Lot ID.
  - Reducer Actions:
    - `SET_DECISION`: Updates a component's scientist decision, records rationale, calculates `aiOverridden` boolean flag, and logs the event to the audit trail.
    - `ADD_COMMENT`: Attaches an engineering note to a component and logs to audit.
    - `FINALIZE_LOT`: Automatically resolves all unresolved `PENDING` components according to their AI recommendations, locks further modifications, and records lot closure.
    - `ADD_AUDIT`: Ingests real-time events.
  - Export functions:
    - `exportCSV()`: Builds a 27-column, standard-compliant CSV string and triggers an automatic browser file download.
    - `exportJSON()`: Serializes component decisions and metrics into an audit-compliant JSON format.

---

### 5.4 Layout & Shared Components (`src/components/`)

- **[`src/components/layout/app-layout.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/components/layout/app-layout.tsx):**
  Main shell structure. Positions the fixed-width sidebar on the left and provides a scrollable, max-width bounded `<Outlet />` container for page content.
- **[`src/components/layout/sidebar.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/components/layout/sidebar.tsx):**
  Left-hand navigation bar. Displays:
  - Branded logo and platform subtitle.
  - Active Lot metadata card (Lot ID, testing status, analysis status, finalization badge).
  - Navigation links with active route highlighting.
  - Real-time lot counters (Components, High Risk, Pending, Reviewed).
  - Institutional decision support disclaimer.
- **[`src/components/shared/risk-badge.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/components/shared/risk-badge.tsx):**
  Standardized risk level badge with semantic icons (`Shield`, `ShieldAlert`, `AlertTriangle`, `ShieldX`) and distinct colors for `LOW` (emerald), `MEDIUM` (amber), `HIGH` (orange), and `CRITICAL` (red).
- **[`src/components/shared/status-badge.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/components/shared/status-badge.tsx):**
  Visual status badge for recommendations and decisions (`PASS`, `REJECT`, `FAIL`, `MONITOR`, `REVIEW`, `PENDING`).
- **[`src/components/shared/kpi-card.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/components/shared/kpi-card.tsx):**
  Reusable dashboard metric tile displaying title, value, icon, and optional click handlers to filter components.
- **[`src/components/shared/decision-dialog.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/components/shared/decision-dialog.tsx):**
  Interactive modal dialog allowing scientists to record formal component decisions:
  - Displays component ID, AI recommendation, and current risk score.
  - Provides mutually exclusive action buttons (`PASS`, `MONITOR`, `REJECT`).
  - Includes a text area for engineering comments and rationale.
  - Automatically dispatches state updates and triggers toast notifications.
- **[`src/components/shared/disclaimer.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/components/shared/disclaimer.tsx):**
  Standard compliance banner reinforcing that AI recommendations serve strictly as decision support.

---

### 5.5 UI Component Primitives (`src/components/ui/`)

Pre-built accessible UI primitives built with Tailwind CSS and Radix/Base UI:
- **`badge.tsx`:** Stylized chips for tags, categories, and counts.
- **`button.tsx`:** Button variants (`default`, `outline`, `ghost`, `destructive`, `secondary`).
- **`card.tsx`:** Standard card containers (`Card`, `CardHeader`, `CardTitle`, `CardDescription`, `CardContent`, `CardFooter`).
- **`dialog.tsx`:** Modal dialog portal, backdrop overlay, header, and content wrappers.
- **`input.tsx`:** Styled text input element for search and filters.
- **`select.tsx`:** Dropdown selector components (`Select`, `SelectTrigger`, `SelectContent`, `SelectItem`, `SelectValue`).
- **`sonner.tsx`:** Wrapper integrating the Sonner toast notification library with CSS color variables.
- **`table.tsx`:** Responsive HTML table components with engineering-grade borders and typography.
- **`tabs.tsx`:** Tabbed interface primitives (`Tabs`, `TabsList`, `TabsTrigger`, `TabsContent`) utilizing Base UI.
- **`tooltip.tsx`:** Hover tooltip primitives for dense data inspection.

---

### 5.6 Application Pages (`src/pages/`)

- **[`src/pages/overview.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/pages/overview.tsx) — Command Center:**
  - *Lot Health Banner:* Dark gradient banner displaying overall lot yield percentage and risk counts.
  - *KPI Cards:* Quick summary metrics for Total Components, Analyzed, High/Critical, Manual Review, Pass, Reject, Monitor, and Pending counts.
  - *Risk Distribution Chart:* Recharts bar chart showing component count per risk category.
  - *Decision Status Donut:* Recharts donut chart with a customized legend showing real-time disposition breakdown.
  - *Top Flagged Table:* List of the highest-risk components with one-click navigation to their detail views.
  - *Recent Activity Feed:* Live feed of the latest audit trail events.

- **[`src/pages/component-explorer.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/pages/component-explorer.tsx) — Component Explorer:**
  - *Interactive Filters:* Search by Component ID, filter by Risk Level (`LOW`–`CRITICAL`), Decision Status (`PASS`–`PENDING`), or AI Recommendation (`PASS`–`FAIL`).
  - *Sorting:* Sort 1,000 components by Risk Score, Anomaly Score, Drift Rate, Predicted Value, or Component ID.
  - *Data Table:* Displays component metadata, risk badges, anomaly scores, predicted 168h values, drift levels, AI recommendations, and final decisions.
  - *Pagination:* Handles client-side pagination (50 components per page) for responsive performance.

- **[`src/pages/component-detail.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/pages/component-detail.tsx) — Component Detail (Core Page):**
  - *Header:* Component ID, lot details, test conditions, overall risk score, AI recommendation, and scientist decision. Displays an alert banner if an AI recommendation has been overridden.
  - *Parameter Switcher:* Dropdown allowing the scientist to toggle between all 5 parameters (Leakage Current, Iddq, Delay, Voltage, Resistance). Charts and metrics update dynamically.
  - *Tabbed Interface:*
    - **Tab 1: Trajectory Analysis:** Composed line and area chart plotting measured points (0h, 24h, 96h), predicted trajectory (connecting 96h to 168h), lot mean baseline, normal distribution band ($\pm 2\sigma$), and critical safety limit line. Tooltips reveal exact values, lot means, and deviations. Includes a population histogram comparing this component against all 1,000 components in the lot.
    - **Tab 2: Risk Metrics:** Three-panel grid with Module A (anomaly score, Z-score, MAD robust Z-score, percentile), Module B (predicted 168h value, safety margin, drift rate, trend direction), and the Combined Risk Assessment breakdown (stacked contribution bar showing 40% anomaly, 35% drift, and 25% prediction contributions).
    - **Tab 3: Decision & Explainability:** Displays the "Why Flagged" panel with natural language explanations alongside the engineering decision panel. Clicking "Set Decision" opens the decision modal.
    - **Tab 4: History:** Component-specific audit trail showing every event involving this component.

- **[`src/pages/risk-analysis.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/pages/risk-analysis.tsx) — Risk Analysis:**
  - *Scatter Plot 1 (Anomaly vs Risk):* Visualizes all 1,000 components with anomaly score on the X-axis and overall risk on the Y-axis. Dots are color-coded by risk level and clickable to navigate directly to component details.
  - *Scatter Plot 2 (Drift vs Risk):* Plots drift score on the X-axis against overall risk on the Y-axis to highlight fast-degrading parts.
  - *Risk Score Histogram:* Frequency distribution of risk scores divided into 5% intervals.
  - *Component Risk Ranking:* Scrollable table ranking the top 50 riskiest components in the lot.

- **[`src/pages/final-review.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/pages/final-review.tsx) — Final Review & Sign-Off:**
  - *Lot Summary Cards:* High-level counts of Pass, Reject, Monitor, and Pending components.
  - *AI Override Summary Table:* Highlights all components where the scientist's decision differs from the AI recommendation, showing the original recommendation, scientist decision, and written rationale.
  - *Comprehensive Table:* Full table with status filter buttons (`ALL`, `PASS`, `REJECT`, `MONITOR`, `PENDING`).
  - *Finalize Lot Action:* Opens a confirmation modal explaining that finalization locks all decisions, auto-resolves pending items, and generates final export archives. Once finalized, decision buttons are disabled across the platform.

- **[`src/pages/export.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/pages/export.tsx) — Data Export:**
  - *Lot Status Indicator:* Displays whether the lot is locked or still pending review.
  - *Export Actions:* Real client-side generation for **Download CSV** and **Download JSON**.
  - *Dataset Summary:* Lists all 27 exported columns, total row count, format details, and active test parameters.

- **[`src/pages/audit-trail.tsx`](file:///c:/Users/dilip/Downloads/SIH26170/src/pages/audit-trail.tsx) — Audit Trail:**
  - *Chronological Event Timeline:* Detailed audit stream of all activities across the system.
  - *Actor Filters:* Filter by Actor (`ALL`, `SYSTEM`, `AI`, `SCIENTIST`).
  - *Search:* Search logs by component ID, action name, or event detail.
  - *Event Badges:* Clear visual indicators distinguishing automated AI flagging from human decisions and system operations.

---

## 6. Getting Started & Installation

### Prerequisites
- **Node.js:** `v18.0.0` or higher (Node 20+ recommended)
- **Package Manager:** `npm` (v9+) or `pnpm` / `yarn`

### 1. Clone or Open Project
Navigate to the root project directory:
```bash
cd SIH26170
```

### 2. Install Dependencies
Install all required runtime and developer packages:
```bash
npm install
```

### 3. Start the Development Server
Launch the local Vite development server with Hot Module Replacement (HMR):
```bash
npm run dev
```
Once started, open your browser and navigate to:
```
http://localhost:5173/
```

### 4. Build for Production
To verify TypeScript types and generate an optimized production bundle:
```bash
npm run build
```
The compiled output is output to the `dist/` directory.

### 5. Preview Production Build
To preview the production bundle locally:
```bash
npm run preview
```

---

## 7. Recommended 3–5 Minute Evaluator Demo Script

Follow this step-by-step walkthrough during demonstration to highlight the complete workflow:

1. **Step 1 — Command Center Overview (`/`):**
   - Review lot `IGBT-2026-017` with 1,000 components.
   - Point out the Lot Health score (~94%), risk distribution bar chart, and top flagged components.
2. **Step 2 — High-Risk Inspection (`/explorer`):**
   - Click the **High** risk filter chip or search for showcase component **`C742`**.
   - Notice the high anomaly score (0.94) and high drift. Click on **`C742`** to open its detail view.
3. **Step 3 — Component Deep Dive (`/component/C742`):**
   - On the **Trajectory Analysis** tab, inspect the time-series chart: observed points at 0h, 24h, and 96h show rapid acceleration. The dashed orange trajectory projects to 47.3 µA at 168h, encroaching upon the 50.0 µA safety limit with a narrow 2.7 µA margin.
   - Switch the parameter dropdown to **Iddq** or **Propagation Delay** to show that all five parameters update with realistic, correlated physics data.
   - Check the **Risk Metrics** tab to review the 40/35/25 multi-factor contribution breakdown.
   - Check the **Decision & Explainability** tab to inspect the plain-language reasons why the component was flagged.
4. **Step 4 — Human Decision & Override:**
   - Click **Set Decision**.
   - Select **MONITOR** and enter: *"Borderline trajectory; engineering review required."* Click **Save Decision**.
   - Re-open the dialog, select **REJECT**, and update the comment: *"Accelerating leakage near boundary; rejected for flight hardware."*
   - Notice the purple **AI Override** banner confirming that human judgment has superseded the AI recommendation.
5. **Step 5 — Population Anomaly Showcase (`/component/C883`):**
   - Search for **`C883`** in the Explorer.
   - Point out that its leakage current (19.3 µA) easily passes the static 50 µA threshold, yet the population comparison chart clearly shows it is a 3.8σ statistical outlier within the lot.
6. **Step 6 — Final Review & Sign-Off (`/final-review`):**
   - Review the AI Override Summary table displaying the decision change for `C742`.
   - Click **Finalize Lot** and confirm. Decisions are now locked and cannot be edited.
7. **Step 7 — Verified Export (`/export`):**
   - Click **Download CSV**.
   - Open the generated file (`IGBT-2026-017_final_report.csv`) to verify that all 27 columns—including measured values, statistical scores, predictions, scientist decisions, overrides, and comments—are accurately recorded.
8. **Step 8 — Traceability Audit (`/audit`):**
   - Open the Audit Trail to demonstrate full accountability with chronological timestamps for data ingestion, AI analysis, component views, and human overrides.

---
