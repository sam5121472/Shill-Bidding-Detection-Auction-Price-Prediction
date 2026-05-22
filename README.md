<div align="center">

# 🔍 Scammed in Australia — A Cry for Protection

### *$290.9 Million Stolen. 175,000 Victims. Zero Online Accountability.*

> An interactive data narrative exposing Australia's scam crisis and who is being left most vulnerable.

**Group 19 · 36104 Data Visualisation & Narratives · MDSI · UTS · Autumn 2026**

[![GitHub Repo](https://img.shields.io/badge/GitHub-Repository-181717?logo=github&logoColor=white)](https://github.com/16aryan/Scam_Project_Collaborator_Part3_Data_Visualisation)
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-Dashboard-FF4B4B?logo=streamlit&logoColor=white)](http://localhost:8501)
[![License](https://img.shields.io/badge/Data%20License-CC%20BY%204.0-lightgrey)](https://creativecommons.org/licenses/by/4.0/)

</div>

---

## 👥 Team Members

| Member | Student ID | Role | Contribution |
|---|---|---|---|
| **Ishaan Gaware** | 26164386 | Architect | Pipeline design, EDA structure, narrative arc definition, project coordination |
| **Aryan Goel** | 26040826 | Analyst | Data enrichment, cross-dataset ABS joins, per-capita metric creation, README |
| **Thi Phuong Nhi Nguyen** | 25969039 | Analyst | Visualisations design, design system implementation, colour accessibility |
| **Faisal Shoaib** | 25747888 | Artist | Presentation design & delivery, final report writing, Gestalt principles, slide layout |
| **Yan Hao** | 25976440 | Developer | Streamlit dashboard build, session state, deployment, data caching |
| **Aishwarya Pandey** | 25524822 | Artist / Orator | Miro board, persona development, presentation script, slide design |
| **Yuxiang Wang** | 25509050 | Analyst | What-if modelling, data dictionary, ACMA enforcement analysis |

---

## 📌 Table of Contents

1. [Problem Statement](#1-problem-statement)
2. [Dataset Description](#2-dataset-description)
3. [Data Dictionary](#3-data-dictionary)
4. [Data Cleaning & Processing](#4-data-cleaning--processing)
5. [Dashboard Walkthrough](#5-dashboard-walkthrough)
6. [Team & Project Planning](#6-team--project-planning)
7. [Narrative Design](#7-narrative-design)
8. [Advanced Features](#8-advanced-features)
9. [Key Insights](#9-key-insights)
10. [Technologies Used](#10-technologies-used)
11. [How to Run](#11-how-to-run)
12. [Limitations & Future Work](#12-limitations--future-work)
13. [Data Provenance & Credits](#13-data-provenance--credits)

---

## 1. Problem Statement

### What problem are we solving?

Australia loses **$28 million to scams every single month**, and online enforcement is almost entirely lacking. While complaints about scam phone calls are **68% lower** since ACMA started enforcing the rules, financial losses have not been reduced — instead, scammers have completely shifted to unregulated digital platforms such as social media, online marketplaces, and search advertising.

### Why it matters

- **$290.9M** was lost by Australians in just 11 months of 2025
- Only **175,104 reports** were made — a small fraction of all scams that occur (most go unreported)
- Investment scams make up **45.4% of reported losses** yet represent just 3.5% of all reports — equating to **$33,000+ per successful attack**
- While older adults (65+) and young adults (18–24) are adversely affected, too few resources are invested in digital literacy programs

### Who is affected

| Stakeholder | How Affected |
|---|---|
| General public | Direct financial harm and psychological distress |
| Young adults (18–24) | Targeted via social media; highest exposure per digital hour |
| Elderly (65+) | Highest average loss per report; targeted via phone and email |
| Small businesses | Investment and impersonation scams drain working capital |
| Government / ACMA | Policy failure in online channel regulation |
| National Anti Scam Centre | Overwhelmed complaint volumes; resource allocation gaps |

### Central Question

> *"Is Australia losing the war on scams — and who is being left most vulnerable?"*

---

## 2. Dataset Description

### Why this dataset meets the "Rich Data" requirement

| Criterion | Evidence |
|---|---|
| **Real-world & recent** | All data from 2024–2026; no synthetic records |
| **Temporal variable** | `month` — datetime64, monthly from Jan 2025 to Nov 2025 |
| **Spatial variable** | `state` — Australian state/territory (ISO-style abbreviations + lat/long centroids) |
| **Multi-dataset enrichment** | 4 datasets joined via state codes and financial year keys |
| **Scale** | 7+ purposeful visualisations across scam type, geography, age, enforcement |

### Datasets Used

| # | Dataset Name | Custodian | Date Range | Role |
|---|---|---|---|---|
| 1 | [Scamwatch Public Dashboard Export](https://www.scamwatch.gov.au/research-and-resources/scam-statistics) | ACCC / Scamwatch | Jan 2025–Nov 2025 | Primary: scam reports, losses, demographics |
| 2 | [Regional Population by Age and Sex 2024 (Cat. 3235.0)](https://www.abs.gov.au/statistics/people/population/regional-population-age-and-sex/latest-release) | Australian Bureau of Statistics | 30 June 2024 | Enrichment: per-capita normalisation |
| 3 | [Action on Scams, Spam & Telemarketing Oct–Dec 2025](https://www.acma.gov.au/action-scams-spam-and-telemarketing) | ACMA | FY 2022–23 to YTD 2025–26 | Enforcement gap analysis |
| 4 | [How Australians Communicate 2025 Survey](https://www.acma.gov.au/how-australians-communicate) | ACMA | 2025 | Contact channel behaviour |

### Dataset Enrichment (Cross-Join)

```
ABS Population (state, young_pop, elderly_pop, total_pop)
    × Scamwatch (state, age, loss)
    = per-capita risk scores: loss_per_100k, rpt_per_100k_young, loss_per_100k_eld
```

This enrichment was essential for the **"So What"** — raw loss numbers favour large states (NSW, VIC) by default. Per-capita normalisation revealed that **WA and NT are disproportionately exposed** relative to their population size, a finding invisible in raw figures.

---

## 3. Data Dictionary

### Scamwatch Dataset (Primary)

| Column | Type | Description |
|---|---|---|
| `month` | `datetime64[ns]` | First day of reporting month (YYYY-MM-01) |
| `state` | `str` | State abbreviation: NSW, VIC, QLD, WA, SA, TAS, ACT, NT, INTL, UNK |
| `contact_mode` | `str` | Delivery channel: Email \| Phone call \| Text message \| Online \| Mail \| In person |
| `age_group` | `str (ordinal)` | Victim age bracket: Under 18 → 65 and over \| Unspecified |
| `gender` | `str` | Victim gender: Male \| Female \| Other \| Unspecified |
| `scam_type` | `str` | ACCC taxonomy Level 3 scam category |
| `amount_lost` | `float64` | Dollar value of reported loss; 0.0 if no financial loss reported |
| `reports` | `int64` | Aggregated report count for this group-month-state cell |

### ABS Population 2024 (Enrichment)

| Column | Type | Description |
|---|---|---|
| `state` | `str` | State abbreviation (ABS Table 3, persons, 30 June 2024) |
| `young_pop` | `int64` | Sum of 15–19 and 20–24 age bands — proxy for 18–24 scam cohort |
| `elderly_pop` | `int64` | Sum of 65–69 through 85+ age bands — proxy for 65+ scam cohort |
| `mid_pop` | `int64` | Sum of 25–64 working-age population |
| `total_pop` | `int64` | Total estimated resident population at 30 June 2024 |

### ACMA Enforcement Data

| Column | Type | Description |
|---|---|---|
| `fy` | `str` | Financial year label; YTD flag for current year (Oct–Dec 2025) |
| `anti_scam` | `int64` | Finalised ACMA anti-scam investigations |
| `spam` | `int64` | Finalised ACMA spam investigations |
| `telemarket` | `int64` | Finalised ACMA telemarketing investigations |
| `scam_call` | `int64` | Consumer complaints about scam phone calls received |
| `scam_sms` | `int64` | Consumer complaints about scam SMS received |

### Derived & Enriched Metrics

| Column | Type | Description |
|---|---|---|
| `loss_per_100k` | `float64` | Dollar losses per 100,000 total population (state-level) |
| `rpt_per_100k` | `float64` | Report count per 100,000 total population (state-level) |
| `avg_loss` | `float64` | Mean dollar loss per report within demographic segment |
| `loss_per_100k_young` | `float64` | Dollar losses per 100,000 young (15–24) population |
| `loss_per_100k_eld` | `float64` | Dollar losses per 100,000 elderly (65+) population |
| `prevented` | `float64` | What-if model: losses prevented by digital literacy program at target rate |
| `bcr` | `float64` | Benefit-cost ratio = total prevented losses ÷ program investment |
| `hazard_score` | `float64` | Composite state risk score (0–100): 40% total loss + 25% loss/capita + 20% avg loss/report + 15% reports |

---

## 4. Data Cleaning & Processing

### Scamwatch Cleaning

- **Column renaming:** Verbose column names standardised (e.g., `Scam___Contact_Mode` → `contact_mode`)
- **Currency parsing:** Custom `money_to_float()` strips `$`, commas, and whitespace; returns `0.0` for missing/non-numeric
- **State standardisation:** `standardise_state_name()` maps full names to abbreviations; overseas records tagged `INTL`, unknowns tagged `UNK`
- **Datetime parsing:** `month` converted to `datetime64[ns]` via `pd.to_datetime()`
- **No-loss records kept:** Reports with no financial loss are retained as scam exposure volume
- **Groupby aggregation:** Records with the same `(month, state, age_group, scam_type, contact_mode)` are aggregated and summed

### ABS Population Cleaning

- Reading Cat. 3235.0 Table 3 (State/Territory by single year of age) using `openpyxl`
- Age band aggregation: `young_pop` (15–24), `mid_pop` (25–64), `elderly_pop` (65+)
- ABS state names aligned with Scamwatch abbreviations

### Feature Engineering

```python
# Per-capita normalisation (cross-dataset join)
by_state['loss_per_100k'] = by_state['losses'] / by_state['total_pop'] * 100_000

# Composite Hazard Score (0–100)
hazard_score = (
    0.40 * normalised_total_loss +
    0.25 * normalised_loss_per_capita +
    0.20 * normalised_avg_loss_per_report +
    0.15 * normalised_total_reports
)

# What-if model
model['prevented'] = model['losses'] * model['reduction_rate']
model['bcr'] = model['prevented'].sum() / program_budget
```

### Libraries Used

`pandas` · `numpy` · `matplotlib` · `matplotlib.gridspec` · `matplotlib.ticker` · `re` · `pathlib` · `streamlit` · `plotly` · `openpyxl`

---

## 5. Dashboard Walkthrough

> All screenshots taken from the live Streamlit dashboard — Group 19, May 2026.

### Act 1: The Anomaly — Overview

Act 1 sets the scene for an executive audience not technically inclined in the field of scams. The KPI cards display: Total Reports, Total $ Lost, Months Covered, and Average Loss/Report. The sidebar allows filtering by state, age group, scam type, and time window. All charts update dynamically with every selection.

![Act 1 — The Anomaly: Overview](outputs/dash_act1_overview.png)

> **Figure 1:** Act 1 — The Anomaly: Overview dashboard showing **126,649 reports** and **$257.9M in losses**. Investment scams account for 45.9% of all losses (donut chart, right). The "Current lens" banner updates in real-time as filters change — **Advanced Feature 1** in action.

---

### Act 2: The Investigation — Victim Profile

The Victim Profile tab shows which age groups are being targeted and to what degree. The diverging bar chart shows report count and average loss per age group. The Risk Matrix plots volume (number of reports) against severity (average loss) to cleanly separate two very different types of targets.

![Act 2 — Victim Profile](outputs/dash_act2_victim.png)

> **Figure 2:** Act 2 — Victim Profile: Reports vs Average Loss by Age Group and Risk Matrix. **45–54 has the highest average loss per report; 65+ contributes the largest total loss** — two different problems requiring two different policy interventions.

---

### Act 2: The Investigation — Geographic Risk

The Geographic Risk tab shows losses by state in raw numbers and per-capita exposure. The key finding: while NSW has high raw losses, **WA has the highest loss per person** — raw totals alone would mislead resource allocation.

![Act 2 — Geographic Risk](outputs/dash_act2_geographic.png)

> **Figure 3:** Act 2 — Geographic Risk: Raw Losses by State vs Per-Capita Loss Exposure. WA leads at **$1.4M per 100k residents** — only visible after the Scamwatch × ABS population enrichment join.

---

### Act 2: The Investigation — Contact Channels

The Contact Channels tab shows channel losses and a heatmap of channel share by scam type. Under all filter configurations, **online is the largest loss channel** — policy must focus on online platforms, not just telcos. Relationship scams flow 81% online; job scams 79% online.

![Act 2 — Contact Channels](outputs/dash_act2_contact.png)

> **Figure 4:** Act 2 — Contact Channels: Losses by Channel and Channel Share Heatmap. Hover any heatmap cell to see exact % loss share — **Advanced Feature 3** (contextual tooltips) in action.

---

### Act 2: The Investigation — Risk Score

The Risk Score tab shows a composite score for each scam type based on report volume, total losses, and average loss per report. **Investment scams rank 87.5/100** — this balances scale, severity, and frequency, offering a policy-neutral prioritisation framework not available in raw Scamwatch data.

![Act 2 — Risk Score](outputs/dash_act2_riskscore.png)

> **Figure 5:** Act 2 — Scam Risk Score: Composite priority ranking by scam type. Investment scams score 87.5 despite only 4,161 reports because each incident costs **$28,450 on average**.

---

### Act 2: The Investigation — Compare Segments

The Compare Segments tab allows any two states, age groups, or scam types to be compared side-by-side. Loss difference, report difference, and average loss difference are all calculated dynamically.

![Act 2 — Compare Segments](outputs/dash_act2_compare.png)

> **Figure 6:** Act 2 — Compare Segments: NSW vs VIC comparison showing **$41.1M loss difference** and 7,965 more reports. Average loss/report is $728 higher in NSW — a different scam-type mix requiring a different intervention.

---

### Act 3: What Next — The What-If Simulator

Act 3 is the policy decision engine. Users select a policy scenario, primary target focus, and use sliders for estimated loss reduction (%) and program budget ($M). **Projected Outcomes** (Losses Prevented, Net Benefit, BCR) update in real-time. The auto-generated Executive Summary combines the Detective's Verdict and four Recommended Actions, refreshing with every filter and slider change.

![Act 3 — What-If Simulator](outputs/dash_act3_whatif.png)

> **Figure 7:** Act 3 — What-If Simulator: Policy scenario modelling with BCR and auto-generated executive summary. Under the Conservative scenario: **$33.3M prevented, BCR 1.1×, Net Benefit $3.3M** — **Advanced Feature 2** (What-If parameterisation) in action.

---

## 6. Team & Project Planning

### Team Members

![Group 19 — Team Members and Roles](outputs/team_members.png)

> **Figure 8:** Group 19 — 7 members across Architect, Analyst, Developer, and Artist/Orator roles.

### Dataset Selection Vote

A democratic vote was used to choose the dataset. **63% voted for the Scamwatch topic** ("Target prevention at the channels and scam types causing the most harm"), demonstrating strong group alignment on the chosen subject.

![Dataset Selection Vote](outputs/dataset_vote.png)

> **Figure 9:** Dataset selection vote — Scamwatch topic won with **63% of team votes**.

### OCEAN Persona Board

Each team member determined their MDSI role according to their personality type using the OCEAN framework. Roles assigned: Architect (Ishaan), Analyst (Aryan, Nhi, Yuxiang), Developer (Yan), Artist/Orator (Faisal, Aishwarya).

![OCEAN Persona Board](outputs/ocean_personas.png)

> **Figure 10:** Miro Board — OCEAN Persona sticky notes for all team members.

### Project Gantt Chart

The project ran from April to May 2026 with milestones including: Dataset Confirmation, Role Allocation, Data Review, Miro Board, Data Cleaning & Preparation, Advanced Feature Planning, Presentation Slides & Script, Final Presentation, and Video Walkthrough & Documentation.

![Project Gantt Chart](outputs/gantt_chart.png)

> **Figure 11:** Gantt chart showing project milestones from April 11 to May 22, 2026.

---

## 7. Narrative Design

### Narrative Arc: The Detective

> *"Start with a data anomaly (the 'crime') and uncover the 'culprit' (the variable)."*

The dashboard follows the Detective arc across three acts:

| Act | Chapter | Narrative Beat |
|---|---|---|
| **Act 1** | The Anomaly | $257.9M stolen. Investment scams = 45.9% of losses despite only 3.5% of reports. Something is systemically broken. |
| **Act 2 — Tab 1** | Victim Profile | 45–54 highest avg loss per report; 65+ largest total loss. Two different problems, two different solutions. |
| **Act 2 — Tab 2** | Geographic Risk | NSW leads raw totals. But WA loses $1.4M per 100k residents. Raw totals mislead resource allocation. |
| **Act 2 — Tab 3** | Contact Channels | Online is the #1 loss channel. Relationship scams 81% online; job scams 79%. Phone regulation worked; the web is ungoverned. |
| **Act 2 — Tab 4** | Risk Score | Investment scams score 87.5/100 composite risk despite only 4,161 reports. Frequency alone is not the right metric. |
| **Act 2 — Tab 5** | Compare Segments | NSW vs VIC: $41.1M more losses, $728 higher avg loss/report in NSW — different mix, different fix. |
| **Act 3** | The Verdict | The unregulated online pipeline is the culprit. Conservative response prevents $33.3M at BCR 1.1×. Mandate platform obligations now. |

### Stakeholder

**ACMA Commissioner + National Anti-Scam Centre of Australia.** The story is designed to persuade a regulatory executive to turn attention to enforcement of online platforms and invest in targeted digital literacy programs.

### User Stories

#### Story 1 — The ACMA Commissioner

> *"As a Commissioner on the ACMA, I would like to have a single-screen view of losses by category and whether our enforcement efforts are hitting the mark, to demonstrate data-driven policy leadership."*

- **Acceptance Criteria:** Dashboard loads in <5 seconds; all Act 1 KPIs displayed without scrolling; enforcement gap clearly identified
- **Definition of Done:** Commissioner can present without further analysis

#### Story 2 — The NASC Triage Officer

> *"As a National Anti-Scam Centre analyst, I want to know how many victims are in each state and age group — so I can focus outreach campaigns and victim support resources efficiently."*

- **Acceptance Criteria:** Top 3 vulnerable segments clearly identified and ranked
- **Definition of Done:** Officer can create a resource allocation brief without further analysis

---

## 8. Advanced Features

### Feature 1: Context-Aware Filtering

Multi-select filters for State, Age Group, Scam Type, and Months Covered, plus a time-range slider, are wired to Streamlit session state. All selections propagate simultaneously across:
- KPI cards (Total Reports, Total $ Lost, Avg Loss/Report, Months Covered)
- All charts across every Act 2 sub-tab
- The "Current Lens" narrative banner in plain English
- The Auto-Generated Executive Summary in Act 3
- The "Download Filtered Data" CSV export button

```python
filtered_df = scam_aus[
    (scam_aus['state'].isin(selected_states)) &
    (scam_aus['age_group'].isin(selected_ages)) &
    (scam_aus['scam_type'].isin(selected_types)) &
    (scam_aus['month'].between(date_start, date_end))
]
# All 7 visuals + KPI cards + narrative text receive filtered_df
```

### Feature 2: What-If Parameterisation

Act 3 implements a full **Policy Intervention Simulator** with:
- **Policy Scenario** dropdown (Conservative / Moderate / Aggressive response)
- **Primary Target Focus** dropdown (High-loss scam types / Young adults / Elderly / All segments)
- **Estimated Loss Reduction (%)** slider
- **Program Budget ($M)** slider

All Projected Outcomes (Losses Prevented, Net Benefit, BCR) recalculate on every slider change. The BCR callout dynamically changes colour — green for positive return, red for negative.

```python
model['prevented'] = model['losses'] * (reduction_pct / 100)
model['residual']  = model['losses'] - model['prevented']
bcr = model['prevented'].sum() / (program_budget * 1_000_000)
```

### Feature 3: Auto-Generated Executive Summary

In Act 3, the "Detective's Verdict" section is dynamically generated from the current filter state and simulator inputs. It combines:
- Latest statistics for the current lens
- The highest-risk age group under current filters
- The selected policy package result and BCR
- Four Recommended Actions — all updated whenever filters or sliders change

### Feature 4: Composite Risk Score *(Bonus)*

The Risk Score tab calculates a score for each scam type (report volume 40%, total loss 35%, average loss per report 25%) scaled to 0–100. Presented as both a horizontal bar chart and an interactive data table — a policy-neutral, multi-dimensional prioritisation tool not available in the raw Scamwatch data.

---

## 9. Key Insights

1. **Investment scams are the dominant financial threat** — 3.5% of total reports generate 45.4% of total losses ($132M). Scammers are shifting from numerous low-cost attacks to fewer, higher-value ones that evade detection.

2. **Phone enforcement works — online enforcement does not** — Scam phone call complaints dropped 68% from 2022–23 to 2024–25 following ACMA's carrier-level intervention. There is no parallel decline in online investment scam losses.

3. **WA and NT are disproportionately exposed** — After per-capita normalisation using ABS 2024 population data, WA and NT rank highest in `loss_per_100k` yet receive fewer dedicated NASC resources than NSW and VIC.

4. **Age 45–54 loses the most per incident** — Average loss of $2,047 vs $1,980 for 65+, suggesting investment scammers specifically target mid-career professionals.

5. **A $50M digital literacy investment has a 4.3× BCR** — Targeted age-group campaigns (20–35% reduction rates) could prevent $214M in annual losses — a net benefit of $164M on a $50M outlay.

---

## 10. Technologies Used

| Technology | Purpose |
|---|---|
| **Python 3.11** | Core data processing and analysis |
| **Pandas** | Data loading, cleaning, transformation, joins |
| **NumPy** | Numerical operations, array handling |
| **Matplotlib** | Static publication-quality visualisations (notebook) |
| **Plotly** | Interactive charts in Streamlit dashboard |
| **Streamlit** | Interactive web dashboard; session state; callbacks |
| **openpyxl** | Reading ABS `.xlsx` population data |
| **GitHub** | Version control, collaboration, deployment source |
| **Jupyter Notebook** | EDA, prototyping, and narrative documentation |

### Design System

- **Colour palette:** WCAG-AA compliant (`#1A3A5C` navy, `#C0392B` red, `#16A085` teal, `#E67E22` orange)
- **Typography:** System sans-serif; axis labels 9pt; titles 13pt bold
- **Layout:** Wide-screen responsive (`layout="wide"`); tab-based chapter navigation
- **Theme:** Dark dashboard background; high contrast for presentations and accessibility

---

## 11. How to Run

### Prerequisites
- Python 3.9+ installed
- Git installed

### Step 1: Clone the repository
```bash
git clone https://github.com/16aryan/Scam_Project_Collaborator_Part3_Data_Visualisation
cd Scam_Project_Collaborator_Part3_Data_Visualisation
```

### Step 2: Install dependencies
```bash
pip install -r requirements.txt
```

### Step 3: Place data files
Ensure the following files are in the `data/` directory:
```
data/
├── Scamwatch903 Public Scams Dashboard - Export.csv
└── 32350DS0003_2024.xlsx
```

### Step 4: Run the Streamlit dashboard
```bash
streamlit run streamlit_app_group19_advanced.py
```

The dashboard will open at `http://localhost:8501`

---

## 12. Limitations & Future Work

### Current Limitations

| Limitation | Detail |
|---|---|
| **Underreporting dark figure** | Scamwatch captures only reported scams — ACCC estimates actual losses are 3–5× higher |
| **ABS proxy cohort** | ABS 15–24 age band used as proxy for Scamwatch's 18–24 — introduces ~12% population overcount |
| **11-month window** | Data runs Jan–Nov 2025 only; December seasonal spike not captured |
| **ACMA enforcement granularity** | Enforcement data is quarterly and aggregated — cannot attribute specific interventions to loss changes |
| **No victim recurrence data** | Cannot distinguish first-time from repeat victims |
| **Gender breakdown gaps** | ~30% of Scamwatch records have `Unspecified` gender — limits gender analysis reliability |

### Future Work

- **Machine learning scam classification:** Train an NLP model on scam description text to predict scam type and estimated loss
- **Real-time data pipeline:** Connect directly to Scamwatch API for live dashboard updates
- **Victim recovery tracking:** Integrate with AFCA data to measure how much money is actually recovered
- **Expanded geographic detail:** Drill down from state to SA3/LGA level using ABS correspondence files
- **Longitudinal comparison:** Extend to 2024 data for year-on-year trend analysis
- **Social media ad volume correlation:** Join with Meta/Google ad transparency data to test the investment scam → paid ad volume hypothesis

---

## 13. Data Provenance & Credits

| Dataset | Custodian | Licence | Accessed |
|---|---|---|---|
| [Scamwatch Public Dashboard Export](https://www.scamwatch.gov.au/research-and-resources/scam-statistics) | ACCC / Scamwatch | CC BY 4.0 | May 2026 |
| [Regional Population by Age and Sex 2024 (Cat. 3235.0)](https://www.abs.gov.au/statistics/people/population/regional-population-age-and-sex/latest-release) | Australian Bureau of Statistics | CC BY 4.0 | May 2026 |
| [Action on Scams, Spam & Telemarketing Oct–Dec 2025](https://www.acma.gov.au/action-scams-spam-and-telemarketing) | ACMA | CC BY 4.0 | May 2026 |
| [How Australians Communicate 2025 Survey](https://www.acma.gov.au/how-australians-communicate) | ACMA | CC BY 4.0 | May 2026 |

---

<div align="center">

**Group 19 · MDSI Data Visualisation & Narratives · UTS · Autumn 2026**

*"Every $1 invested in targeted digital literacy prevents $4.30 in scam losses."*

</div>

