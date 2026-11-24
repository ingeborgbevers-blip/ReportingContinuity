# Reporting Continuity: From a Completed Task to a Usable Outcome

![Power BI reporting continuity analysis showing action completeness and overdue exceptions](Effective%20reporting.png)

## Overview

A task marked **completed** can leave the next person without enough information to act.

This Power BI case study explores how a fictional SME warehouse maintains its stock exception reporting when work passes between people. It follows the process through refreshing the report, checking figures, reviewing exceptions, recording next actions and releasing the report.

The study continues the earlier **Recurring Stock Discrepancies** analysis. It uses the same warehouse, products, bins and count observations, then adds fictional workflow, availability, learning and capability records.

The earlier study asked what explained the stock differences. This study asks whether the reporting process gives people the access, preparation, support and ownership needed to investigate and follow them through.

**Workflow:** Synthetic CSV tables → Power Query preparation → Power BI modelling and DAX → process and case-level investigation → practical recommendations.

The project is relevant to SME owners, warehouse and operations managers, reporting leads, ERP users and people responsible for training or handovers.

**All data is synthetic.** Findings describe designed scenarios, not real employees or measured business improvements.

---

## The Problem

Completion, correctness and follow-through are different checks.

A refresh can finish while loading a previous count. A briefing can be recorded while an access barrier remains unresolved. A reporting step can be marked completed while individual exceptions lack a clear action, owner or deadline.

The analysis investigates:

- Whether released reports use the intended stock-count cycle.
- What happens when the usual analyst is unavailable.
- How briefings, guided practice and observed assessments relate to recorded work.
- Whether instructions and support help people handle changes and exceptions.
- Whether reporting findings receive usable next actions.
- How release responsibility is covered when the regular owner is unavailable.

The purpose is to identify where the process needs attention, rather than infer employee capability from a completion tick or an absence alone.

---

## The Approach

### Dataset and Scope

The source package contains:

| Area | Scope |
|---|---|
| Warehouse | WH01, the same fictional warehouse as the stock study |
| Stock context | 24 products, three bins and 864 original count observations |
| Reporting period | 12 Monday report runs, 10 August to 26 October 2026 |
| Process | Five expected steps per run, giving 60 work-step records |
| People | Six fictional employees |
| Exception cases | 119 cases, one for each discrepant original count observation |
| Learning evidence | 12 learning-session records and 18 capability assessments |
| Operational extraction cut-off | 26 October 2026, 17:00 |

The stock context remains unchanged from the first study. This is a separate Power BI report; importing the earlier Power BI file is not required.

### Stage 1: Report Freshness and Cover

The first stage compares the intended count cycle with the source cycle actually loaded, alongside release deadlines and actual release times.

The investigation then follows refresh work performed by Priya and Jo, examining guide versions, support minutes, recorded barriers and learning dates.

### Stage 2: Readiness, Capability and Follow-Through

The second stage examines whether known barriers were resolved, what independent assessments demonstrated and how work changed when an exception occurred.

It then follows the process beyond refresh to action recording and release. Individual exception records provide a second check on what a completed process step actually confirms.

---

## The Solution

### Power Query and Data Model

The model keeps records at their original grains:

| Table | Grain and purpose |
|---|---|
| ReportRuns | One scheduled report instance; target/source cycles and release dates |
| WorkSteps | One expected process step per run, including incomplete or unstarted work |
| ProcessSteps | One process step, with sequence and expected evidence |
| People | One fictional person and role |
| Availability | One person/report-window availability record |
| LearningSessions | One person/task learning event |
| CapabilityAssessments | One observed person/task assessment |
| ExceptionCases | One discrepant original count, with investigation and action details |
| StockCounts | One original item/bin/count-cycle observation |
| CountCycles | One original stock-count cycle |
| Products and Bins | Shared stock dimensions from the first study |

Helper names are resolved separately from the assigned operator's name. This preserves the distinction between performing a task and supporting someone else.

Process names are sorted using `ProcessSteps[step_order]`:

1. Refresh report.
2. Check figures.
3. Review exceptions.
4. Record next actions.
5. Release report.

### Power BI Measures and Interpretation

| Measure | Definition |
|---|---|
| Completed work steps | Work-step status is `Completed`; a finish timestamp alone is insufficient |
| On-time report releases | A nonblank release time on or before its deadline |
| Current-cycle report releases | A released report whose source cycle matches the intended cycle |
| Independent completed steps | Completed work with no recorded helper or support minutes |
| Active labour minutes | Operator active minutes plus additional helper minutes |
| Cases with complete action details | Nonblank next action, named owner and deadline |
| Action details complete % | Cases with complete details divided by all exception cases in scope |
| Open overdue actions | Deadline before the fixed cut-off and no recorded action completion |
| Open cases without an action deadline | No recorded deadline and no action completion |
| Late completed actions | Recorded action completion after its deadline |

Labour measures describe person-minutes of effort. They are not elapsed time. Likewise, action-detail completeness is not the same as action completion.

For capability comparisons, use the latest assessment for each person/task at or before the relevant reporting date. A later assessment must not be used to describe an earlier person's readiness. No assessment means unknown, rather than incapable.

---

## Dashboard Highlights

The final five-page report follows the evidence in this order:

1. **The report was refreshed. Was the right count used?** Compare target/source cycles, release timing and analyst availability.
2. **A briefing was recorded. The first refresh still encountered a gap.** Connect refresh outcomes with guide versions, practice and practical support.
3. **Effective cover: a readiness check needs a response.** Examine a known access barrier and capability under changing task conditions.
4. **Effective cover: the refresh is only the first handover.** Review action-recording status and release responsibility.
5. **Effective reporting: does completed leave someone able to act?** Inspect individual exception records for action details and overdue follow-up.

The emphasis moves from report-level outcomes to the evidence behind them, then to the person responsible for the next action.

---

## Key Findings

### 1. A Completed Refresh Did Not Guarantee Current Data

Across 12 reporting cycles:

- Eight reports were released on time.
- Two released reports used the previous stock-count cycle.
- Three reports were released late.
- The final report remained unreleased and overdue at the extraction cut-off.

Timing and source freshness are separate dimensions. The eight on-time releases, three late releases and one unreleased run account for all 12 scheduled runs.

### 2. The First Cover Attempts Encountered Access and Documentation Gaps

Priya's refresh on 17 August encountered unavailable access to the current source folder. Jo's refresh on 31 August encountered an undocumented workbook query path.

Both used older extracts after attending briefings with guide G01. These records identify practical barriers; they do not establish that either employee lacked the ability to perform the task.

Following guided practice with revised guide G02, all six subsequent refreshes performed by these employees used the intended cycle. Four still had support recorded.

Practice, revised guidance, support and experience occurred together. Their individual effects cannot be isolated from these records.

### 3. A Readiness Check Identified a Barrier Before Cover Was Needed

Priya's backup refresh check on 13 August was recorded as `Blocked by access`.

Four days later, the same barrier remained during the refresh. The next investigation is whether resolution was assigned and whether successful access was retested before cover was required.

### 4. Independent Assessment Did Not Cover Every Later Situation

Jo completed an observed refresh independently on a saved report copy on 29 September.

On 12 October, a changed source column required Priya's support. The assessment on 13 October recorded completion with support.

This illustrates why assessment context and changing task conditions matter. It does not prove a general loss of capability or explain later assignment decisions.

### 5. Action Recording and Release Had Separate Gaps

The action-recording step was incomplete in three runs. One of those reports, CC08, was nevertheless released on time.

Casey was the regular release owner and completed the previous eleven releases. The final run's recorded barrier was `Release owner unavailable after review`.

The first finding raises questions about completion criteria and action review. The second raises questions about alternative release arrangements. Neither should automatically be treated as a training problem.

### 6. Case-Level Detail Challenged the Completion Tick

At 26 October 2026, 17:00:

| Exception follow-up measure | Result |
|---|---:|
| Total exception cases | 119 |
| Cases with complete action details | 88 |
| Action details complete | 73.95% |
| Open cases without an action deadline | 31 |
| Open overdue actions with recorded deadlines | 7 |

The 31 cases without deadlines are a separate group from the seven open overdue actions. A missing deadline must not be interpreted as an action being on time.

In CC01, `Record next actions` was marked completed, but four of fourteen cases lacked complete action details. Even where details were complete, some actions remained overdue.

> The completion criteria and the review of individual records need to be checked together.

---

## Operational Recommendations

### 1. Verify the Source After Refresh

Check the intended count cycle against the cycle actually loaded. Include this in the refresh checklist and release review.

### 2. Close Known Readiness Gaps

Give each access or documentation barrier an owner and deadline. Confirm resolution through a successful retest by the person providing cover.

### 3. Match Practice to the Work

Use guided practice and observed tasks alongside instructions. Include changed sources, failed checks and escalation routes where relevant. Keep the assessment date and context visible.

### 4. Define What Completed Means

Agree the evidence required before marking each process step completed. For action recording, check that individual cases contain a clear action, owner and deadline.

### 5. Make Release Cover Explicit

Define an authorised alternative when the regular release owner is unavailable, including what that person must verify before release.

### 6. Monitor Follow-Through

Review overdue actions and missing action information separately. Confirm what happened after completion, and inspect later cycles for recurring issues.

---

## Data Limitations

- All people, activities and outcomes are fictional, designed for this portfolio exercise.
- Patterns are illustrative and do not estimate how common these issues are in real businesses.
- The dataset is not a SAP Business One or Produmex export. Application-specific permissions and approval mechanisms are not represented.
- Learning attendance, supported practice, assessment and live work provide different evidence. They must not be treated as interchangeable measures of competence.
- Unrecorded training or support cannot be assumed absent in a real-world application of this approach.
- The available records do not establish every underlying cause, escalation decision or reason for later task assignments.
- Case status and action completeness describe the fixed extraction cut-off. They are not a complete history of status changes for each reporting date.
- A closed case or completed action does not establish that the underlying stock-process issue has been permanently resolved.
- The study does not isolate training effects or measure financial savings, productivity uplift or implemented improvement.
- Stock values copied from the earlier study must not be appended to those originals as additional observations.

The analysis identifies where further investigation and targeted checks would be useful. It does not demonstrate that the recommendations have already produced benefits.

---

## Demo

Start with the accompanying five-page case study PDF, then follow the report-level findings into the work, assessment and exception records.

The preview above uses `Week 2.png`, showing action completeness and overdue exceptions.

Companion source materials are supplied in the **StraightLine November Dataset 2: Reporting Continuity** package:

- `Reporting_Continuity.xlsx` and equivalent CSV tables in `data/`.
- `START_HERE.md`, including modelling guidance and definitions.
- `POWER_BI_START.md`, suggesting a first investigation page.
- A field dictionary.
- Separate reference answers and validation results.

Reference answers are intended for checking the designed scenarios after exploring the records.

---

## How to Run

### Requirements

- A PDF reader to view the finished case study.
- Power BI Desktop to reproduce or extend the analysis.
- The synthetic source workbook or CSV tables from the reporting continuity dataset package.

No SQL database is required for this workflow.

### Steps

1. Download the case study and source package.
2. Import `ReportRuns`, `WorkSteps`, `ProcessSteps` and `People` into a new Power BI file.
3. Add `Availability`, `LearningSessions`, `CapabilityAssessments` and `ExceptionCases` for the supporting evidence.
4. Import the included `StockCounts`, `CountCycles`, `Products` and `Bins` when stock context is required.
5. Set IDs to text, minutes to whole numbers and timestamps to local **Date/Time**. Do not introduce a timezone conversion.
6. Relate `ReportRuns` to `WorkSteps` and `ExceptionCases` by `run_id`, and `ProcessSteps` to `WorkSteps` by `step_id`.
7. Resolve person names for their specific roles. A helper lookup must use `support_person_id`, rather than the assigned operator relationship.
8. Sort `step_name` by `step_order` and create the completion, release and action-detail measures.
9. Use the fixed extraction cut-off, rather than today's date, for the historical overdue analysis.
10. Reconcile the results to the case study and inspect individual records before interpreting totals.

### Modelling Precautions

Use one active filtering route where CountCycles, ReportRuns, StockCounts and ExceptionCases create alternative paths. Do not force a many-to-many relationship to make an ambiguous model work.

People appears in several roles: operator, helper, investigator, action owner, facilitator and assessor. Use role-specific dimensions or targeted lookups/merges rather than trying to activate every relationship to one People table.

Do not flatten work steps onto every exception case and then sum minutes: that multiplies the work records. Assessments are time-varying observations, so use the latest relevant assessment instead of joining all historical assessments to each work step.

---

## Suggested Business Use

This case study demonstrates how managers could review:

- Reporting continuity during absence.
- Whether a refresh loads the expected period or count.
- Access readiness and usable process instructions.
- Practical learning and assessment evidence.
- Support needs when a familiar task changes.
- Clear ownership of exceptions and release responsibilities.
- Whether completed work leaves someone able to take the next action.

The same investigation approach can be adapted to other recurring operational or management reports after validating their source records and business definitions.

> A reporting process earns confidence through the handovers the team can rely on.

---

## Tools Used

- Power BI Desktop.
- Power Query.
- DAX.
- Synthetic CSV and Excel source tables.
- PDF report export.

GitHub is used to publish the portfolio documentation and supporting materials.

---

## Project Status

Completed as a portfolio case study for **StraightLine Data & Training**, November 2026.

Potential future development includes testing the approach against permitted operational records, adding a full action-status history, documenting escalation and release authorisation, and measuring recurrence after an implemented process change.

No measured improvement is claimed in this version.

**StraightLine Data & Training**  
Clarity. Insight. Confidence.  
[www.straightlinedata.co.uk](https://www.straightlinedata.co.uk/)
