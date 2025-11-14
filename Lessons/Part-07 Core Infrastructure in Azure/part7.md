# Expert guide — **Azure Core Infrastructure** (for **MS-SC-900** prep)

## Quick roadmap — what this guide covers

1. Azure management hierarchy & resource organization (regions, subscriptions, management groups, resource groups, ARM). ([Microsoft Learn][1])
2. Identity & access control fundamentals (Entra/ Azure AD, RBAC, conditional access concepts). ([Microsoft Learn][2])
3. Network core components and protections (VNets, subnets, NSGs, Azure Firewall, DDoS, WAF, Bastion). ([Microsoft Learn][3])
4. Secrets and keys (Azure Key Vault). ([Microsoft Learn][4])
5. Compute, scale, availability (VMs, scale sets, regions, availability zones). ([Microsoft Learn][5])
6. Monitoring, detection and cloud security posture (Azure Monitor, Microsoft Defender for Cloud). ([Microsoft Learn][6])
7. Security best practices and how these map to MS-SC-900 skills measured. ([Microsoft Learn][4])

---

# 1 — Azure management hierarchy & resource organization (core facts)

**What to know (exam style):**

* **Hierarchy**: *Management groups → Subscriptions → Resource groups → Resources*. Management groups group subscriptions for centralized policy and access control. Subscriptions are billing and quota boundaries. Resource groups are logical containers for lifecycle and RBAC scoping. ARM (Azure Resource Manager) is the management layer and provides declarative templates and the resource model. ([Microsoft Learn][7])
* **Why it matters for security/compliance**: policies (Azure Policy) and RBAC apply at these scopes; correct scoping is essential for least privilege, consistent tagging, cost attribution, and enforcing security baselines. (See the Cloud Adoption Framework guidance on organizing resources.) ([Microsoft Learn][8])

**Study tips / must-remember phrases**

* “Policy at management group level affects all child subscriptions.” ([Microsoft Learn][7])
* ARM templates / Bicep are the way to deploy repeatable infrastructure (declarative). Know the concept, not syntax.

---

# 2 — Identity & Access (Entra / Azure AD & access control)

**Core concepts**

* **Entra ID (Azure AD)**: Microsoft’s cloud identity service for authentication, single sign-on, user/group/account lifecycle, app registration, and identity protection features. Distinguish Entra/Azure AD (cloud directory service) from on-prem Active Directory (AD DS). Entra ID provides identity objects (users, groups, service principals, managed identities). ([Microsoft Learn][2])
* **RBAC**: Role-based access control gives fine-grained permissions across Azure resources. Roles (built-in, custom) are assigned at a scope (management group, subscription, resource group, resource). Principle of least privilege is enforced via RBAC. ([Microsoft Learn][9])
* **Conditional Access / Identity protection**: Policies evaluate risk signals and require MFA, block access, or restrict sessions. Know the purpose (protect access based on conditions) rather than implementation details. ([Microsoft Learn][2])
* **Managed identities & service principals**: Use non-user identities for apps and services to access resources (instead of embedding secrets). Understand the difference: service principal = app identity; managed identity = platform-managed identity tied to a resource. ([Microsoft Learn][2])

**Examable tasks / concepts**

* Given a scenario, identify whether to use RBAC role assignment, conditional access, or Azure Policy to meet the requirement. (RBAC controls *who* can do *what*; Policy controls *what* resources can be and enforces configurations; Conditional Access controls *how and when* identities can authenticate.)

---

# 3 — Network core components & protections

**Foundational elements**

* **Virtual Network (VNet)**: The fundamental unit for network isolation — contains subnets, route tables, and network interfaces for resources. VNets are logically isolated and can be peered. ([Microsoft Learn][3])
* **Subnets**: Segmentation within a VNet; apply NSGs and service endpoints at subnet or NIC level. ([Microsoft Learn][10])
* **Network Security Group (NSG)**: Stateful filtering of traffic with allow/deny rules. Typically used to filter traffic to/from subnets and NICs. Know that NSGs are not a firewall replacement for all needs — they are ACL-style controls. ([Microsoft Learn][11])

**Perimeter & advanced protections**

* **Azure Firewall**: Managed, stateful network firewall as a service (policy, rules, filtering, FQDN). Use for central egress/ingress control in hub-and-spoke architectures. ([Microsoft Learn][4])
* **DDoS Protection**: Azure provides basic DDoS protection automatically; *DDoS Protection Standard* is a paid tier that gives adaptive tuning, telemetry, and mitigation for VNets. Understand the purpose and that Standard is used to protect public endpoints. ([Microsoft Learn][4])
* **Web Application Firewall (WAF)**: Protects web apps (Application Gateway WAF or Azure Front Door WAF) against typical web attacks (OWASP rules). ([Microsoft Learn][4])
* **Azure Bastion**: Managed PaaS that provides secure RDP/SSH to VMs over the Azure portal without exposing VM ports to the internet. Useful detail for secure jump-host scenarios. ([Microsoft Learn][4])

**How it maps to exam questions**

* Be able to *choose* the right control to meet a requirement (e.g., protect web app from SQLi/XSS → WAF; require secure admin access → Bastion; protect public endpoints from volumetric attacks → DDoS Standard).

---

# 4 — Secrets & Keys: Azure Key Vault

**Essentials**

* **Purpose**: secure storage for secrets, keys, and certificates. Supports HSM-backed keys (managed HSM or Key Vault with standard HSM protection). Use Key Vault to avoid storing secrets in code/config. ([Microsoft Learn][4])
* **Access control**: integrate with RBAC and access policies; use managed identities for apps to retrieve secrets without credentials. Know the difference between access control via Azure RBAC and legacy Key Vault access policies. ([Microsoft Learn][7])

---

# 5 — Compute, scaling, availability

**Key facts**

* **VMs** are basic compute; **VM Scale Sets** provide autoscaling and orchestration for similar workloads. Understand use cases for scale sets (stateless, autoscaling). ([Microsoft Learn][5])
* **Regions & Availability Zones**: Regions are geographic; availability zones are physically separate datacenter locations within a region for high availability. Use paired regions and zone-aware services for resilience. ([Microsoft Learn][1])

**Exam focus**

* Recognize resilience options (region failover, zone redundancy) and when to recommend scale sets vs. individual VMs.

---

# 6 — Monitoring, detection, and posture management

**Azure Monitor**

* Collects metrics, logs, and traces across Azure and on-prem; supports alerts, dashboards, and automated responses. Core for observability and incident detection. ([Microsoft Learn][6])

**Microsoft Defender for Cloud (formerly Security Center)**

* CSPM + workload protection: collects telemetry, provides recommendations, threat detection, cross-cloud support, and can enable additional plans for DBs, containers, etc. Understand that Defender for Cloud offers both posture management (CSPM) and workload protection (EDR, runtime protections) depending on plans. ([Microsoft Learn][12])

**What to memorize**

* Defender for Cloud = posture + runtime protection; Azure Monitor = telemetry and alerting pipeline. Know their role separation and how they integrate.

---

# 7 — Security best practices — concise checklist (what examiners expect you to know)

* **Least privilege**: use RBAC roles scoped to the lowest required scope. ([Microsoft Learn][9])
* **Identity protection**: enable MFA & conditional access for administrative roles. ([Microsoft Learn][2])
* **Network segmentation**: use VNets, subnets, NSGs; centralize egress/ingress via Azure Firewall or WAF where appropriate. ([Microsoft Learn][3])
* **Secrets handling**: store secrets/keys in Key Vault; use managed identities. ([Microsoft Learn][7])
* **Visibility & response**: enable Azure Monitor + Defender for Cloud; ship logs to Log Analytics/workspace for retention/analysis. ([Microsoft Learn][6])
* **Resilience**: design with regions/zones and scale sets where required. ([Microsoft Learn][1])

---

# 8 — How MS-SC-900 typically tests these topics (question formats & examples)

MS-SC-900 is foundational. Expect:

* **Conceptual multiple choice** (identify which service meets a requirement).
* **Scenario questions** (pick configuration or service to meet security/compliance requirement).
* **Match services to capabilities** (e.g., which service stores TLS certs? → Key Vault).

**Example practice items (short)**

1. *Which service provides centralized identity for Azure and supports conditional access?* — **Entra ID (Azure AD)**. ([Microsoft Learn][2])
2. *You need to restrict RDP access to VMs without exposing RDP port to the internet — which service?* — **Azure Bastion**. ([Microsoft Learn][4])
3. *Which service offers a managed, stateful network firewall for central network policy?* — **Azure Firewall**. ([Microsoft Learn][4])

---

# 9 — Verified primary study sources (start here — official docs & learn)

* MS-SC-900 study guide (skills measured): Microsoft Learn certification page. ([Microsoft Learn][4])
* Describe core architectural components of Azure (Learn module). ([Microsoft Learn][1])
* Azure Resource Manager & resource organization. ([Microsoft Learn][7])
* Entra ID / Azure AD overview & B2C docs. ([Microsoft Learn][2])
* Azure Networking: Virtual Network, NSGs docs. ([Microsoft Learn][3])
* Azure Key Vault & security docs. ([Microsoft Learn][7])
* Azure Monitor & Defender for Cloud. ([Microsoft Learn][6])

> I used these Microsoft Learn / docs pages as the authoritative sources and cross-checked the “core infrastructure” items listed in the MS-SC-900 skills measured page. ([Microsoft Learn][4])

---

# 10 — Condensed study checklist (printable)

* [ ] Understand management scopes: mgmt groups, subscriptions, resource groups, resources. ([Microsoft Learn][7])
* [ ] Be able to explain Azure AD (Entra) features and identity protection basics. ([Microsoft Learn][2])
* [ ] Know RBAC vs. policies vs. conditional access (when to use each). ([Microsoft Learn][9])
* [ ] Explain VNets, subnets, NSGs; and Azure Firewall, DDoS, WAF, Bastion roles. ([Microsoft Learn][3])
* [ ] Know Key Vault purpose and managed identities. ([Microsoft Learn][7])
* [ ] Be able to describe Azure Monitor and Defender for Cloud (CSPM + workload protection). ([Microsoft Learn][6])

---

## Final notes & how I checked this

* I started from Microsoft’s **MS-SC-900 skills measured / study guide** and then cross-checked each technical area against the appropriate Microsoft Learn documentation and product overviews (Azure networking, RBAC, Entra ID, Key Vault, Defender for Cloud, Azure Monitor, ARM). The most load-bearing exam topics listed on the MS-SC-900 study guide (including **core infrastructure security services**) are explicitly described in the linked Learn docs. ([Microsoft Learn][4])


[1]: https://learn.microsoft.com/en-us/training/modules/describe-core-architectural-components-of-azure/ "Describe the core architectural components of Azure"
[2]: https://learn.microsoft.com/en-us/shows/windows-azure-active-directory/ "Windows Azure Active Directory"
[3]: https://learn.microsoft.com/en-us/azure/virtual-network/ "Virtual Network documentation - Azure"
[4]: https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/sc-900 "Study guide for Exam SC-900"
[5]: https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview "Azure Virtual Machine Scale Sets overview"
[6]: https://learn.microsoft.com/en-us/azure/azure-monitor/ "Azure Monitor documentation"
[7]: https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview "What is Azure Resource Manager?"
[8]: https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-setup-guide/organize-resources "Organize your Azure resources effectively - Cloud Adoption ..."
[9]: https://learn.microsoft.com/en-us/azure/role-based-access-control/ "Azure RBAC documentation"
[10]: https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-subnet "Add, change, or delete a virtual network subnet"
[11]: https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview "Azure network security groups overview"
[12]: https://learn.microsoft.com/en-us/azure/defender-for-cloud/ "Microsoft Defender for Cloud documentation"
