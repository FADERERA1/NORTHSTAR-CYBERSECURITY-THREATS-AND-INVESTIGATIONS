# NORTHSTAR CYBERSECURITY THREATS AND INVESTIGATIONS

## Cybersecurity Log Analysis and Threat Investigation

## Executive Summary

This three-week Group A SQL portfolio project from Tech Studio Academy analyzes fictional Northstar Services security records using MySQL. The supplied dataset contains **238 login events** from **September 1–30, 2026**. The analysis covers login outcomes, IP reputation, employee and device behavior, and security alerts. Findings identify patterns for follow-up; they do not prove an attack or account compromise.

## Project Objectives

- Measure login volume, success and failure rates, and activity over time.
- Identify accounts, IP addresses, devices, and departments with notable failed-login activity.
- Examine successful logins associated with high-risk IP addresses.
- Review alert severity, status, and resolution times.
- Compare department and company failure rates and build a reusable daily security summary.

## Dataset Overview

| Source table | Records | What it contains |
| --- | ---: | --- |
| `employees` | 18 | Employee names, departments, roles, and employment status |
| `devices` | 20 | Employee-linked devices and operating systems |
| `login_logs` | 238 | Login timestamps, outcomes, IPs, employees, and devices |
| `security_alerts` | 15 | Alert type, severity, status, and timestamps |
| `ip_reputation` | 12 | IP risk level, country, and reputation notes |

The data is fictional training data. The supplied README notes that some data-quality issues are intentional.

## Key Business Questions

- What share of login attempts succeeded or failed, and when did activity occur?
- Which days, hours, accounts, and IP addresses had the most failed logins?
- Were successful logins associated with IPs marked high risk?
- Which departments and devices had the highest failure counts or rates?
- How many accounts used multiple devices or logged in on weekends/outside 09:00–17:00?
- How many alerts were high or critical, unresolved, or associated with longer resolution times?
- How did failed login volume change from one day to the next?

## Summary Metrics

### Login Outcomes

| Measure | Result |
| --- | ---: |
| Total login attempts | 238 |
| Successful | 159 (66.8%) |
| Failed | 79 (33.2%) |
| Reporting period | September 1–30, 2026 |

### Failed Login Patterns

| Measure | Result |
| --- | --- |
| Highest failure count by weekday | Friday and Saturday: 14 each |
| Highest failure count by hour | 08:00: 14 failed attempts |
| Highest failed-login IP | `185.220.101.7`: 18 failures |
| Next highest failed-login IP | `45.95.147.22`: 12 failures |
| Two consecutive failed events followed by a success | 16 matching event sequences |

The sequence count applies the SQL script's method: each successful login whose two immediately preceding employee events were failures counts as a match.

### Department Login Rates

| Department | Attempts | Failed | Failure rate |
| --- | ---: | ---: | ---: |
| Finance | 47 | 23 | 48.9% |
| HR | 45 | 18 | 40.0% |
| IT | 58 | 21 | 36.2% |
| Sales | 39 | 12 | 30.8% |
| Operations | 33 | 4 | 12.1% |
| Marketing | 16 | 1 | 6.3% |

Finance had both the most failures and the highest failure rate in this sample. IT had the highest login volume. These are descriptive comparisons, not evidence that a department caused the failures.

### IP Reputation

| IP risk level | Attempts | Successful | Failed |
| --- | ---: | ---: | ---: |
| High Risk | 42 | 2 | 40 |
| Medium Risk | 11 | 2 | 9 |
| Low Risk | 131 | 103 | 28 |
| Trusted | 53 | 51 | 2 |
| Missing/unmatched IP reputation | 1 | 1 | 0 |

Two successful logins were associated with high-risk IPs. The high-risk group had 40 failed attempts out of 42 total attempts. One login record had no matching IP reputation entry because its IP value was blank.

### Employee, Device, and Time Patterns

| Measure | Result |
| --- | ---: |
| Employees who used more than one device | 2 of 18 |
| Logins on weekends or outside 09:00–17:00 | 153 of 238 (64.3%) |
| Failed attempts by the two highest-count accounts | Amara Okafor: 12; Fatima Sani: 12 |

The working-hours measure follows the project query's rule: weekend activity or a time before 09:00 or at/after 17:00.

### Security Alerts

| Measure | Result |
| --- | ---: |
| Total alerts | 15 |
| Critical | 5 |
| High | 4 |
| Medium / Low | 3 / 3 |
| Resolved | 7 |
| Open / Investigating | 4 / 4 |
| High or Critical and still Open/Investigating | 6 |
| Resolved alerts with usable timestamps | 7; average 303 minutes (5 h 3 min), median 140 minutes (2 h 20 min) |

## Major Findings and Practical Follow-up

1. **Failed logins were 33.2% of recorded attempts.** The sample contains 79 failures among 238 logins. Review the failed-login patterns alongside user and device context.
2. **High-risk IP activity merits review.** The dataset records 42 attempts from high-risk IPs, including two successful logins. This is an investigation lead, not proof of unauthorized access.
3. **Failure rates vary by department.** Finance had a 48.9% failure rate (23 of 47), while IT had the largest volume (58 attempts). Compare the underlying records and login volume before drawing operational conclusions.
4. **Six high/critical alerts remain unresolved.** Prioritize checking their current status and supporting events.
5. **Failed-login activity peaked on September 14 and 25.** Each date had seven failures; September 25 rose by six compared with the prior calendar day.
6. **Some logins occurred outside the stated workday/week.** 153 records (64.3%) met the project’s weekend/out-of-hours definition. Check expected schedules and time-zone assumptions before treating this as suspicious.

## SQL Skills Demonstrated

- Filtering, sorting, grouping, and aggregation with `WHERE`, `ORDER BY`, `GROUP BY`, `COUNT`, `SUM`, `MIN`, and `MAX`
- Conditional aggregation with `CASE` and boolean conditions
- Multi-table `INNER JOIN` and `LEFT JOIN` analysis
- Common table expressions (CTEs)
- Window functions: `LAG` for event sequences and daily comparisons; `DENSE_RANK` for ranking employee accounts
- Date/time functions for weekday, hour, date, and alert-resolution calculations
- Percentage calculations using `ROUND` and `NULLIF`
- Creating and querying the `daily_security_summary` view

## Method and Data Notes

Summary values above were calculated from the supplied CSV files using the project’s stated table relationships and analysis definitions. They are real counts from the provided fictional dataset, not exported results from a MySQL session.

The SQL script contains a company failure-rate condition written as `' Failed'` with a leading space. This does not match the dataset value `'Failed'` and would make that company-level count incorrect. The department-level counts above were recalculated from the CSV status values. Review query filters and joins against the imported tables before using SQL output.

## Repository Contents

| File | Description |
| --- | --- |
| `SQL PROJECT FILE NTERN.sql` | SQL exploration and investigation queries |
| `October Intern - Group A/` | Dataset CSVs and supporting project files |

## How to Run

1. Import the five CSV files as `employees`, `devices`, `login_logs`, `security_alerts`, and `ip_reputation` in MySQL.
2. Confirm database/table names and timestamp types.
3. Review and correct the SQL filters and joins against the imported data.
4. Run the analysis queries and compare the returned results with the summary above.

## Limitations

- Northstar Services and all records are fictional training data.
- IP risk labels and unusual patterns are indicators for investigation, not confirmation of malicious activity.
- The 30-day period is a limited sample and may not represent normal long-term behavior.
- Missing IP information and any field, join, or status inconsistencies can affect conclusions.

## Project Credit

Tech Studio Academy — Group A SQL Portfolio Project.

