---
title: "Week 9 Worklog"
date: "`r Sys.Date()`"
weight: 109
chapter: false
pre: " <b> 1.9. </b> "
---



# Week 9 Report

### Objectives
* Exhaustive well architecture framework to apply to project
* Write a document about the API references to prepare do a frontend project
* Fix bug relate to format json datetime in cache
* Logic automation to collect data customer in every 10 minutes
* Hot fix bug with client provider

### Tasks to be carried out this week:
| Day | Task                                                                                                                                            | Start Date | Completion Date | Reference Material |
| :--- |:------------------------------------------------------------------------------------------------------------------------------------------------|:-----------|:----------------| :--- |
| 2 | - **Architecture Review:** <br> - Review project against AWS Well-Architected Framework <br> - Identify security & reliability gaps             | 3/11/2025  | 3/11/2025       | AWS Well-Architected Tool |
| 3 | - **Documentation:** <br> - Write API reference document <br> - Define request/response schemas for Frontend preparation                        | 4/11/2025  | 4/11/2025       | Internal Wiki / Swagger |
| 4 | - **Feature Implementation:** <br> - Logic automation to collect customer data <br> - Configure scheduler (Cron/Lambda) for 10-minute intervals | 5/11/2025  | 5/11/2025       | Python Boto3 Docs |
| 5 | - **Bug Fix (Cache):** <br> - Investigate JSON serialization error <br> - Fix JSON datetime format issue in Redis cache                         | 6/11/2025  | 6/11/2025       | StackOverflow / Library Docs |
| 6 | - **Maintenance:** <br> - Diagnose Client Provider connectivity issue <br> - Deploy Hotfix to production                                        | 7/11/2025  | 7/11/2025       | System Logs / CloudWatch |

### Week 9 Achievements:

* **Infrastructure Optimization:**
  * Conducted an exhaustive review of the project using the **AWS Well-Architected Framework**.
  * Mapped out improvements for data resiliency and security compliance.

* **Documentation & Frontend Handoff:**
  * Successfully authored and finalized the **API Reference Document**.
  * Provided clear endpoints and data structures to facilitate the Frontend integration phase.

* **Backend Automation:**
  * Developed and deployed the data collection logic.
  * Established a reliable automation cycle to fetch customer data every **10 minutes**.

* **Critical Bug Resolutions:**
  * **Cache Layer:** Resolved the `JSON datetime` formatting bug, ensuring smooth data serialization in the cache.
  * **Client Provider:** Identified and applied a **hotfix** for the client provider connectivity issue, restoring service stability.