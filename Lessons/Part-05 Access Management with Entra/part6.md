# Comprehensive guide — **Access Management with Microsoft Entra**

## At-a-glance: what you must know for SC-900

* **Microsoft Entra ID** is Microsoft’s cloud identity service (Azure AD was renamed to Microsoft Entra ID). Understand identity types, authentication, single sign-on, and hybrid identity. ([Microsoft Learn][1])
* **Access control in Entra** is achieved using Authentication methods (including passwordless), Conditional Access policies (zero-trust policy engine), role-based access (RBAC), and governance features (PIM, entitlement management, access reviews). ([Microsoft Learn][2])
* **Identity protection & reporting**: Entra ID Protection detects risky sign-ins and user risk; its signals can feed Conditional Access. ([Microsoft Learn][3])
* **Privileged access management**: PIM implements just-in-time (JIT) elevation, approval workflows, time-bound role activation and access review of privileged roles. ([Microsoft Learn][4])
* **Entitlement management / identity governance**: Access packages, workflows, and automated provisioning for guests and employees at scale. ([Microsoft Learn][5])

(Official SC-900 learning resources and the exam outline are on Microsoft Learn — use them alongside these notes). ([Microsoft Learn][6])

---

# 1) Core identity concepts and terminology

* **Identity** = principal that needs authentication (user, service principal, managed identity, guest). Know the difference between:

  * *User identities* (workforce, guest/B2B, consumer/B2C),
  * *Service principals* / app registrations (used by apps to access resources), and
  * *Managed identities* (for Azure resources). ([Microsoft Learn][1])
* **Authentication vs Authorization**: authentication verifies who you are; authorization determines what you can do (roles, permissions). Understand OAuth2 / OpenID Connect basics (flows used for SSO & delegated access).
* **Directory/tenant**: the Entra tenant stores identities; trust boundaries and consent operate at tenant level.

**Exam tip:** be prepared to identify identity types and explain why one is chosen (example: managed identity for VM to call Key Vault without credentials).

---

# 2) Microsoft Entra product family (short)

* **Microsoft Entra ID** — cloud directory and identity provider (what used to be Azure AD). ([Microsoft Learn][1])
* **Entra Permissions Management** — CIEM for cloud permissions (broader cloud posture).
* **Entra Verified ID** — decentralized identity (verifiable credentials).
  *(For SC-900, most questions focus on Entra ID + governance features.)*

---

# 3) Authentication methods (what to know)

* **Password + MFA**: traditional username/password plus Azure MFA (push, OTP, phone call, OATH).
* **Passwordless**: Windows Hello for Business, Microsoft Authenticator (phone-sign in / passkeys), FIDO2 security keys — recommended for stronger, phishing-resistant auth. Microsoft now heavily promotes passwordless options. ([Microsoft Learn][2])
* **Hybrid sign-in options** (when you have on-prem AD):

  * **Password Hash Synchronization (PHS)** — sync a hash of on-prem password to Entra for cloud auth. ([Microsoft Learn][7])
  * **Pass-Through Authentication (PTA)** — passes auth to on-prem AD.
  * **Federation (AD FS)** — full federation for sign-ins.
* **Self-service password reset (SSPR)**, password protection (custom banned lists), and risk-based authentication are important management capabilities.

**Exam tip:** know what each hybrid option does and a basic scenario where each is appropriate (e.g., PHS for simpler deployments, federation for complex SSO requirements).

---

# 4) Single Sign-On (SSO) & Application access

* **SSO models**: SAML, OpenID Connect, OAuth2 are supported; Entra provides SSO integrations (enterprise applications).
* **Application objects vs Service principals**: app object is the global app definition; service principal is the tenant-specific identity used to grant access. Understand difference and lifecycle.

**Exam tip:** you may be asked to recognize which protocol an app uses or why you'd register an app vs create a service principal.

---

# 5) Conditional Access (the Zero-Trust policy engine)

* **What it does:** Conditional Access evaluates signals (user, device, location, application, risk) and enforces policies (require MFA, block access, require compliant device, require app protection). Microsoft calls it the Zero-Trust policy engine. ([Microsoft Learn][8])
* **Common signals & controls:** user or group membership, risk level from Identity Protection, device state (joined/compliant), application, sign-in risk, location (named locations / trusted IPs). Controls include block, require MFA, require device compliance, require hybrid Azure AD joined, Terms of Use, session controls (limited access via Defender for Cloud Apps).
* **Policy planning & order:** policies should be tested (report-only), and you must consider policy precedence and exclusions (e.g., emergency access accounts).

**Exam tip:** understand typical Conditional Access scenarios (e.g., block legacy auth, require MFA for admin roles, require compliant device for accessing sensitive apps).

---

# 6) Identity Protection (risk detection) & how it integrates

* **Identity Protection** uses signals and machine learning to detect risky sign-ins and user compromise (leaked credentials, atypical travel, anonymous IP, impossible travel). Administrators can set policies to block or require MFA on risky sign-ins or for risky users. These signals feed Conditional Access. ([Microsoft Learn][3])

**Exam tip:** expect scenario questions where you must pick actions based on detected risk (e.g., require password reset, block sign-in, enforce MFA).

---

# 7) Role-based Access Control (RBAC) & administrative roles

* **Built-in roles** (Global Admin, Security Admin, User Administrator, etc.) control who can manage resources.
* **Custom roles** can be created for granular permission sets.
* **Administrative units** for scoped admin delegation.
* **Least privilege**: best practice is to minimize role assignments (don’t assign Global Admin broadly).

**Exam tip:** know definitions of common roles and why you’d prefer PIM (just-in-time elevation) over permanent assignments.

---

# 8) Privileged Identity Management (PIM)

* **Purpose:** limit standing admin access by requiring activation (approval, MFA, justification), set time-bound access, collect justification and review, and produce access reports. PIM also supports alerts and access reviews for privileged roles. ([Microsoft Learn][4])
* **Capabilities to know:** eligible vs active assignments, approval workflows, time limits, MFA enforcement, Just-in-Time access, access review scheduling, alerts & audit logs.

**Exam tip:** expect to choose PIM to enforce JIT admin activation or to perform periodic privileged access reviews.

---

# 9) Identity governance & entitlement management

* **Access packages:** bundle resources (groups, apps, SharePoint sites) into packages users can request; include lifecycle policies, expiration, and request/approval workflows. ([Microsoft Learn][9])
* **Access reviews:** periodic reviews of group and role memberships to keep access current.
* **Entitlement management** automates governance across external users (B2B) and internal users, with connected organizations and catalogues for requests.

**Exam tip:** be ready to match governance needs (e.g., periodic contractor access) to entitlement management features.

---

# 10) External identities — B2B & B2C

* **B2B collaboration:** invite external users as guests to your tenant; you can apply Conditional Access and governance to guest access.
* **B2C (Azure AD B2C / Entra customer identity):** for consumer-facing apps; separate considerations for identity providers (social, local accounts).

**Exam tip:** differentiate when a business should use B2B collaboration vs B2C.

---

# 11) Device identity and conditional access (hybrid/device posture)

* **Device joining types:** Azure AD joined, Hybrid Azure AD joined (on-prem AD + Entra), Azure AD registered. Device compliance is commonly enforced via Intune/endpoint manager and used as a Conditional Access signal.
* **Use cases:** require compliant device to access corporate email or block unmanaged devices.

---

# 12) Monitoring, logging & compliance

* **Audit logs and sign-in logs** in Entra for investigations.
* **Integrations**: send logs to SIEM (e.g., Microsoft Sentinel) for correlation.
* **Reporting**: conditional access insights, PIM audit reports, risky sign-in reports from Identity Protection.

---

# 13) Licensing considerations (what features require P1/P2)

* Many governance/security features are split by license tiers (Entra ID Free, Microsoft Entra ID P1, P2). Examples:

  * **PIM** and **Identity Protection** and advanced entitlement management capabilities typically require **P2**.
  * Conditional Access baseline features may require P1 for some advanced controls. (Always check current SKUs in Microsoft documentation for exact licensing boundaries.)
    *(Because licensing names/entitlements change, double-check the current Microsoft licensing page when planning deployments.)*

**Exam tip:** know roughly which features are premium (P1/P2) versus included in free tier; the exam tests the concept rather than exact SKU pricing.

---

# 14) Zero Trust and access management

* **Zero Trust principles**: verify explicitly, least privilege access, assume breach. Entra and Conditional Access are core building blocks for Zero Trust identity controls. ([Microsoft Learn][8])

---

# 15) Practical configuration & secure deployment checklist

(Useful to memorize as exam scenarios or for practical understanding)

* Enable **MFA** and enforce for admins.
* Block **legacy authentication** (IMAP/POP/basic auth).
* Protect **break-glass**/emergency accounts (don’t put them behind risky policies; ensure JIT access).
* Implement **Conditional Access** policies incrementally (report-only → pilot → enforce).
* Deploy **passwordless** options where possible (Authenticator, FIDO2). ([Microsoft Learn][2])
* Use **PIM** for privileged roles and schedule **access reviews**. ([Microsoft Learn][4])

---

# 16) How Microsoft Entra features map to SC-900 exam objectives

From Microsoft’s SC-900 skill outline, study these Entra topics closely:

* Describe Entra functions & identity types. ([Microsoft Learn][6])
* Authentication capabilities (MFA, passwordless, SSPR). ([Microsoft Learn][2])
* Access management capabilities (Conditional Access, RBAC). ([Microsoft Learn][8])
* Identity protection & governance (Identity Protection, PIM, entitlement management, access reviews). ([Microsoft Learn][3])

---

# 17) Study resources (official & reliable)

* Microsoft Entra ID docs (start here). ([Microsoft Learn][1])
* Conditional Access docs & conceptual overview. ([Microsoft Learn][8])
* Authentication methods & passwordless guidance. ([Microsoft Learn][2])
* PIM & Identity Protection docs. ([Microsoft Learn][4])
* Microsoft Learn SC-900 learning path and exam study guide (official). ([Microsoft Learn][6])

Use Microsoft Learn modules (hands-on sandbox work), the official docs, and instructor-led videos linked from Microsoft Learn as your core study materials.

---

# 18) Quick exam-style practice questions (brief)

1. **Q:** Which Entra feature enforces policies like “require MFA when sign-in risk is high”?
   **A:** Conditional Access fed by Entra Identity Protection signals. ([Microsoft Learn][8])

2. **Q:** You want to limit Global Admin standing access and require approval each time an admin is needed. Which feature?
   **A:** Privileged Identity Management (PIM). ([Microsoft Learn][4])

3. **Q:** Which method synchronizes a hash of the on-prem password to Entra for cloud authentication?
   **A:** Password Hash Synchronization (PHS). ([Microsoft Learn][7])

4. **Q:** You need to give contractors temporary access to a set of groups and apps with automated expiration. Which feature?
   **A:** Entitlement management / access packages. ([Microsoft Learn][5])

(If you want, I’ll generate 30+ practice questions and detailed answer explanations mapped to the SC-900 objectives.)

---

# 19) Final study strategy (how to use these notes)

1. Walk the official SC-900 learning path on Microsoft Learn (watch module videos, complete sandboxes). ([Microsoft Learn][10])
2. Read the Entra docs for each major feature above (Conditional Access, Identity Protection, PIM, entitlement management, authentication). ([Microsoft Learn][8])
3. Practice scenario questions (I can generate more) and hands-on lab tasks: configure a Conditional Access policy in a dev tenant, set up SSPR, configure PIM.
4. Memorize mappings (feature → problem solved) — exam items are often scenario based.

[1]: https://learn.microsoft.com/en-us/entra/identity "Microsoft Entra ID documentation"
[2]: https://learn.microsoft.com/en-us/entra/identity/authentication/concept-authentication-methods "Authentication methods and features - Microsoft Entra ID"
[3]: https://learn.microsoft.com/en-us/entra/id-protection/overview-identity-protection "What is Microsoft Entra ID Protection?"
[4]: https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/ "Privileged Identity Management documentation"
[5]: https://learn.microsoft.com/en-us/entra/id-governance/entitlement-management-overview "What is entitlement management?"
[6]: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/sc-900 "Study guide for Exam SC-900"
[7]: https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/whatis-phs "What is password hash synchronization with ..."
[8]: https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview"Microsoft Entra Conditional Access: Zero Trust Policy Engine"
[9]: https://learn.microsoft.com/en-us/entra/id-governance/entitlement-management-access-package-first "Manage access to resources in entitlement management"
[10]: https://learn.microsoft.com/en-us/training/paths/describe-capabilities-of-microsoft-identity-access/ "Introduction to Microsoft Entra - Training"
