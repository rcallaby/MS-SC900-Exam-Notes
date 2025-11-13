# Identity Protection & Identity Governance with Microsoft Entra — Exam-focused guide (for **MS-SC900**)

Below is an exhaustive, exam-oriented study guide covering **Identity Protection** and **Identity Governance** in Microsoft Entra (Entra ID). All statements are drawn from official Microsoft documentation and Microsoft product pages; citations follow the most important sections so you can verify details directly.

---

# Quick overview — what you must know

* **Identity Protection** detects risky sign-ins and risky users, produces risk signals, and enables automated responses (block, require password reset, require MFA) that integrate with Conditional Access. ([Microsoft Learn][1])
* **Identity Governance** is the set of capabilities that let you govern who has access to what and for how long — main components are Privileged Identity Management (PIM), Entitlement Management (access packages), and Access Reviews. ([Microsoft Learn][2])
* Many advanced Identity Protection & Governance features require **Microsoft Entra ID Premium P2** (or equivalent suites); PIM and Identity Protection are P2 features. Always confirm licensing for features you plan to use. ([Microsoft Learn][3])

---

# Part A — Identity Protection (concepts, signals, actions)

## What is Identity Protection?

Microsoft Entra Identity Protection analyzes signals from sign-ins and user behavior using machine learning to detect likely compromises, then surfaces **risk detections** and **risk levels** (low/medium/high) for sign-ins and for user accounts. These signals feed automated remediation and Conditional Access decisions. ([Microsoft Learn][1])

## Key signals / detections

* **Sign-in risk detections**: e.g., impossible travel, anonymous IP addresses, leaked credentials, atypical travel, atypical properties of the sign-in.
* **User risk**: aggregated risk score for an account based on past detections and ongoing activity.
  (Full detection types and definitions are documented in Identity Protection references.) ([Microsoft Learn][1])

## Risk policies & remediation options

* **Two main risk policies** (configurable in Identity Protection):

  * **Sign-in risk policy** — respond to risky sign-ins (require MFA, block, etc.).
  * **User risk policy** — respond when a user is flagged as risky (e.g., force password reset, block).
* Policies can be set to **require MFA**, **block access**, or **require password change**; they may also be configured in **report-only** before enforcement. ([Microsoft Learn][4])

## Integration with Conditional Access

* Identity Protection provides risk signals that Conditional Access can use as conditions in policies. A common pattern: Conditional Access requires MFA when sign-in risk is medium or higher. Use Conditional Access for composable, granular enforcement combining device state, user group, location, and risk signals. ([Microsoft Learn][5])

## Investigation & response

* **Dashboards and reports**: Identity Protection provides dashboards for risky users, risky sign-ins, and detections. Roles like Global Reader / Reports Reader allow viewing these reports.
* **Recommended response flow**: triage detection → check sign-in logs and device state → apply remediation (password reset, block, require MFA) → monitor. Identity Protection can be paired with SIEM ingestion for advanced investigation. ([Microsoft Learn][6])

## Best practices (exam-relevant)

* Start in **report-only** mode to validate risk policies. ([Microsoft Learn][4])
* Protect administrative accounts with stronger policies and require MFA; exclude emergency/break-glass accounts from automated blocks but monitor them.
* Use **passwordless** and phishing-resistant auth where possible to reduce risky sign-ins. (Passwordless reduces attack surface for credential-based compromises.)

---

# Part B — Identity Governance (PIM, Entitlement Management, Access Reviews)

## Privileged Identity Management (PIM) — core concepts

* **Purpose:** Reduce standing privileged access by using just-in-time (JIT) activation, approval workflows, time-bound assignments, and auditing for privileged roles (Entra, Azure, Microsoft 365). PIM helps implement least-privilege and JIT admin access. ([Microsoft Learn][2])
* **Key concepts:**

  * **Eligible vs Active assignments:** eligible users must activate to become active for a limited time.
  * **Activation controls:** MFA, approval, justification, ticket number, time limit.
  * **Approval workflows & notifications:** configure approvers for activation requests.
  * **Access reviews & alerts:** PIM supports recurring reviews and alerts for high-risk activity.
* **Typical uses:** emergency access, limiting Global Admins, temporary elevated access for break-fix scenarios. ([Microsoft Learn][3])

## PIM capabilities exam checklist

* Ability to convert permanent role assignments to eligible assignments.
* Configure activation rules (MFA, approval, duration).
* View audit logs and activation history.
* Configure alerting for suspicious behavior while privileged. ([Microsoft Learn][7])

## Entitlement Management & Access Packages

* **Purpose:** Automate lifecycle of user access to groups, applications, and sites via **access packages** that users can request. Useful for onboarding contractors, cross-organization collaboration, or project-based access with expiration and automated revocation. ([Microsoft Learn][8])
* **Components:** catalogs, access packages, policies (who can request — internal, external, connected orgs), approval flows, lifecycle/expiration, and connectors to Azure AD groups and application assignments. ([Microsoft Learn][9])
* **Use cases:** temporary contractor access, cross-company collaboration (B2B) with automatic expiration, delegated access management by non-admins.

## Access Reviews

* **Purpose:** Periodic attestation of continued need for access — reviewers (owners, managers, or selected users) are prompted to approve or deny access; results can automatically remove access. Access Reviews help meet compliance and least-privilege objectives. ([Microsoft Learn][10])
* **Capabilities:** recurring schedules (weekly/monthly/quarterly/annually), reviewer selection, insights & recommendations, auto-apply results (remove access if denied), integration with entitlement management and PIM. ([Microsoft Learn][10])

## Governance best practices (exam-relevant)

* Use **access packages** for external collaborators and contractors rather than manual guest invites. ([Microsoft Learn][8])
* Configure **access reviews** on critical groups, privileged roles, and access packages to reduce stale access. ([Microsoft Learn][10])
* Use **PIM** to ensure privileged roles are eligible and just-in-time where possible; schedule regular privileged access reviews. ([Microsoft Learn][2])

---

# Part C — How Identity Protection & Governance work together (scenarios)

* **Scenario: compromised user detected**

  1. Identity Protection marks sign-in or user as risky. ([Microsoft Learn][1])
  2. Conditional Access evaluates the risk signal and may require MFA or block the sign-in. ([Microsoft Learn][5])
  3. If user is an admin, PIM can be used to ensure no standing admin privileges are present (or to limit activation). Admin activations are logged and reviewed. ([Microsoft Learn][2])

* **Scenario: contractor access for 90 days**

  1. Create an access package in Entitlement Management that includes groups/apps and sets a 90-day expiration and an approval workflow. ([Microsoft Learn][9])
  2. Create access reviews to run every 30 days for the package. ([Microsoft Learn][10])

---

# Licensing & feature availability (concise)

* **Microsoft Entra ID P2** is required for full Identity Protection and advanced governance (PIM, Identity Protection, entitlement management advanced features). Always confirm current licensing pages for precise SKU boundaries. ([Microsoft Learn][3])

---

# Audit, logging & monitoring

* **Sign-in logs and audit logs**: use the Microsoft Entra admin center to view sign-in details and remediation steps. Export logs to a SIEM (e.g., Microsoft Sentinel) for correlation, long-term retention, and advanced alerting. Identity Protection reports provide quick triage dashboards. ([Microsoft Learn][6])

---

# Practical configuration checklist (hands-on labs you should know)

1. Enable Identity Protection and review default detections (P2 required). ([Microsoft Learn][1])
2. Create a **Sign-in risk** policy in Identity Protection (start report-only → require MFA → block if needed). ([Microsoft Learn][4])
3. Create Conditional Access policy that uses **sign-in risk** and **user risk** signals. Validate in report-only mode first. ([Microsoft Learn][5])
4. Enable **Privileged Identity Management**; convert a test Global Admin to eligible, configure activation settings and approval workflow. ([Microsoft Learn][3])
5. Create an **Access Package** in Entitlement Management that includes a group and app, configure external user rules and expiration. ([Microsoft Learn][9])
6. Configure an **Access Review** for that group to run quarterly and auto-remove denied access. ([Microsoft Learn][10])

---

# Exam mapping & likely question types

* **Recognize** risk detections and pick the correct remediation (require MFA / password reset / block). ([Microsoft Learn][1])
* **Match** governance features to scenarios (e.g., “temporary contractor access” → Access Package/Entitlement Management; “reduce standing admin privilege” → PIM). ([Microsoft Learn][8])
* **Identify** which capabilities require P2 licensing (Identity Protection and PIM are P2-level). ([Microsoft Learn][3])

---

# Sample practice questions (short — answers below)

1. Which Entra service produces a **sign-in risk** level that Conditional Access can evaluate?
2. You need to ensure Global Admins are not permanently assigned and must request elevation for admin tasks. Which Entra feature should you use?
3. A contractor needs access for a project that lasts 60 days and must expire automatically after. Which capability do you use?
4. Which tool would you use for periodic attestation of group membership and application access?

**Answers:** 1) Identity Protection (sign-in risk). ([Microsoft Learn][1])
2) Privileged Identity Management (PIM). ([Microsoft Learn][2])
3) Entitlement Management — Access Package with 60-day expiration. ([Microsoft Learn][9])
4) Access Reviews. ([Microsoft Learn][10])

---

# Further reading & authoritative study links

* Microsoft Entra Identity Protection docs (concepts, detection list, risk policies). ([Microsoft Learn][1])
* Privileged Identity Management (PIM) overview and configuration. ([Microsoft Learn][2])
* Entitlement Management / Access Packages. ([Microsoft Learn][8])
* Access Reviews overview and how to create them. ([Microsoft Learn][10])
* Conditional Access overview and how to use Identity Protection signals. ([Microsoft Learn][5])
* Microsoft Entra licensing and plan pages (verify P1/P2 entitlements before deploying). ([Microsoft][11])

---

# Final exam-prep tips (concise)

* Understand **what each feature does** (Identity Protection vs Conditional Access vs PIM vs Entitlement Management vs Access Reviews) and **when to choose it**. ([Microsoft Learn][1])
* Know the **signal → policy → action** flow: Identity Protection → Conditional Access → enforcement/remediation. ([Microsoft Learn][5])
* Practice in a lab tenant (enable report-only modes first), and be comfortable mapping features to the scenario-based questions the SC-900 uses.

---


[1]: https://learn.microsoft.com/en-us/entra/id-protection/ "Microsoft Entra ID Protection documentation"
[2]: https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/ "Privileged Identity Management documentation"
[3]: https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/pim-getting-started "Start using Privileged Identity Management"
[4]: https://learn.microsoft.com/en-us/entra/id-protection/howto-identity-protection-configure-risk-policies "Microsoft Entra ID Protection - Risk policies"
[5]: https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview "Microsoft Entra Conditional Access: Zero Trust Policy Engine"
[6]: https://learn.microsoft.com/en-us/entra/id-protection/howto-identity-protection-investigate-risk "Investigate risk with Microsoft Entra ID Protection"
[7]: https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/pim-configure "What is Microsoft Entra Privileged Identity Management?"
[8]: https://learn.microsoft.com/en-us/entra/id-governance/entitlement-management-overview "What is entitlement management?"
[9]: https://learn.microsoft.com/en-us/entra/id-governance/entitlement-management-access-package-create "Create an access package in entitlement management"
[10]: https://learn.microsoft.com/en-us/entra/id-governance/access-reviews-overview "What are access reviews? - Microsoft Entra"
[11]: https://www.microsoft.com/en-us/security/business/microsoft-entra-pricing "Microsoft Entra Plans and Pricing | Microsoft Security"
