# UAE ERP SaaS — Master Product Specification

Version: 1.0 | Baseline date: 2026-11-03 (Asia/Kolkata) | Status: documentation baseline for human review

Authority: MASTER PROMPT 00 supplied by the product owner. This specification is the primary reference for subsequent requirements, architecture, module, phase, compliance and implementation documentation. It does not authorise application development or certify regulatory compliance.

## 1. Evidence, terminology and governance

### 1.1 Repository inspection

The complete available workspace, `D:\Project\ERP`, was inspected on 2026-11-03 using a recursive file inventory and a hidden-item directory listing. It was empty and was not a Git repository. No `AGENTS.md` was present in the workspace or at `D:\Project` or `D:\`. Consequently, no README, product specifications, architecture documentation, ADRs, roadmap, project status documents or application code were available to read. There are no existing architecture decisions to confirm or supersede. Missing evidence is a limitation, not approval of an implementation design. The supplied master prompt is the only existing product baseline reviewed.

### 1.2 Requirement status

| Label | Meaning |
| --- | --- |
| C — Confirmed | Explicit product-owner requirement from MASTER PROMPT 00. |
| P — Proposed | Concrete specification detail proposed here; requires human acceptance before becoming a binding implementation decision. |
| R — Recommended | Practice recommended for design or verification; not a selected architecture. |
| A — Assumption | Working premise that must be validated. |
| D — Pending | Unresolved decision with an owner and resolution gate in section 20. |
| E — Evidence | Official-source observation; does not establish applicability to every customer. |

Identifiers are stable references. Unless labelled C, detailed expectations below are P. Acceptance of this documentation task does not silently approve items explicitly marked pending. Later specifications must cite relevant identifiers, add measurable acceptance tests, and preserve unresolved decisions.

### 1.3 Vocabulary and boundaries

| Term | Definition |
| --- | --- |
| Organisation / tenant | Customer isolation boundary containing memberships, configuration, subscriptions and business data. These terms are synonymous here. |
| Company | Business/legal entity managed inside a tenant; multiple companies must be supported. Exact legal-entity/branch mapping remains D-01. |
| User | Authenticated person; membership grants access to a tenant and separately authorised companies. Global identity versus tenant-local accounts is D-02. |
| Platform operator | SaaS provider staff operating the service; not automatically a member of every customer company. |
| Module | Business capability registered in a configurable catalogue with availability, dependencies and entitlements. |
| Plan | Commercial packaging of modules, limits and pricing; a bundle must not remove the ability to buy supported modules individually. |
| Entitlement | Effective permission for a tenant to use a subscribed capability, subject also to user permissions and company scope. |
| Subscription invoice | Invoice issued by the SaaS provider to its subscribing customer. |
| ERP customer invoice | Invoice issued by a tenant's company to that company's customer. Separate ownership, numbering, payment allocation and lifecycle from subscription invoices. |

GOV-01 (C): Preserve the approved phase order and names. A change proposal must describe rationale, scope, dependencies, regulatory impact and migration implications and obtain human approval before changing the roadmap. Record future architecture selections in ADRs; this document does not create ADRs by implication.

GOV-02 (P): New detailed documents should reference this master rather than copy it. Conflicting requirements must be logged with both sources and an explicit resolution status. No Prompt 01 work is included.

## 2. Product overview and vision

PRD-01 (C): Deliver a commercially viable, secure, scalable, modular ERP SaaS primarily for businesses operating in the UAE. Customers register an organisation, manage companies and users, select and subscribe to modules, change subscriptions and use subscribed capabilities from a unified platform.

PRD-02 (C): Support small businesses, SMEs and larger organisations, with a path to enterprise expansion and future modules without major architectural changes.

Vision: give UAE businesses one coherent operational record across customer relationships, sales, finance, purchasing, people and projects, with module choice aligned to business need and traceable local regulatory support.

PRD-03 (P): The unified experience must provide explicit active-tenant and active-company context, consistent navigation, search and permission feedback, and access to subscription administration for authorised administrators. Shared records must have an identified owning domain; modules must not create conflicting customer, supplier or company masters.

## 3. Business objectives and success measures

| ID / status | Objective | Proposed evidence of success; numerical targets pending D-14 |
| --- | --- | --- |
| BUS-01 C | Commercial viability through modular subscriptions | Trial-to-paid conversion where trials exist, recurring revenue, churn and payment recovery tracked separately from ERP sales. |
| BUS-02 C | Central administration and connected operations | Organisation onboarding, company creation, invitation and module activation can be completed through documented workflows. |
| BUS-03 C | UAE relevance | Bilingual/RTL workflows, local fiscal outputs and provider integrations pass their applicable release checks. |
| BUS-04 C | Secure, reliable growth | Isolation/security evidence, restore drills and capacity measurements support the selected service tier. |
| BUS-05 C | Extensible product | A later module can register permissions, entitlements, dependencies and navigation using documented contracts. |

Metrics must use tenant-safe, privacy-reviewed collection. Business targets and release dates are not yet approved.

## 4. Target market and customer segments

Primary geography is the UAE. Expansion outside the UAE is future scope, not an assertion of other-country compliance.

| Segment | Primary needs | Product implication |
| --- | --- | --- |
| Small business / owner-managed | Simple onboarding, customers, quotations, invoices and cash visibility | Guided setup, clear module costs, understandable workflows and exports. |
| SME / growing multi-department business | Accounting, purchasing, inventory, HR and controlled approvals | Shared master data, role separation, reconciliation and configurable approval policies. |
| Larger / multi-company organisation | Company-scoped access, governance, scale, integration and audit evidence | Explicit company boundaries, extensible permissions and enterprise readiness in Phase 11. |

A-01: Initial customers are general commercial businesses. Priority industries, first customer cohort and free-zone/mainland coverage are D-03. Manufacturing, healthcare and other specialised regulated verticals are not implied.

## 5. Commercial model and module subscription strategy

COM-01 (C): Modules have configurable pricing, availability and dependency rules. Support monthly and annual billing, optional configured trials, discounts/promotions, subscription history, payment status, subscription invoices, grace periods and suspension policies. Do not hardcode commercial policies in application logic.

COM-02 (P): Catalogue entries require a stable module identifier, versioned commercial terms, effective dates, currency, billing interval, dependency list and activation availability. Historical invoices and subscription agreements retain the accepted price and policy version after catalogue changes. Packaging, per-seat/per-company/usage charging and included limits are D-04; no price is established here.

COM-03 (P): Before confirmation, present selected modules, prerequisite modules, billing period, recurring amount, applicable tax, discounts, renewal/effective date and consequences of change. Dependencies must be validated for missing prerequisites and cycles. Removing a prerequisite must block or explicitly include dependent-module changes; no silent loss of capability.

COM-04 (P): Distinguish commercial subscription state, payment state and effective entitlements. Proposed lifecycle: trial (if configured), active, past due, grace, suspended and cancelled/expired. Future-dated changes require effective-time evaluation. Payment success must be verified through authenticated provider evidence rather than a browser redirect. Payment success, failure, retry and refund must not cause duplicate invoices or entitlements. Activation on payment versus other approved commercial terms is D-05.

COM-05 (C/P): Support upgrades, downgrades and module purchases. Proration, immediate versus renewal changes, refund rules, trial conversion, cancellation notice, auto-renewal and promotion stacking are configurable policies pending D-05. Every transition records actor, reason, prior/new terms and effective date.

COM-06 (P): Deactivation must preserve business records and audit history. Define allowed read/export access and treatment of queued jobs, integrations, open transactions and retention before implementing suspension. Blocking a paid feature must not silently destroy legally retained records. Retention/legal hold can override scheduled deletion. A dependency or compliance requirement is not waived by unpaid subscription status; customer communication and access policy require review.

COM-07 (C): SaaS subscription billing remains separate from ERP customer invoicing. Provider revenue/payment reconciliation must never appear as a tenant company's customer receivable merely because that company subscribes. Provider subscription billing may itself have UAE tax/e-invoicing obligations; validate independently (REG-05).

COM-08 (P): Core administration remains available as required to resolve billing, manage authorised access and request export. Which core features are included commercially, entitlement scope per tenant/company and limits are D-04. Module permission checks alone cannot substitute for entitlement checks.

## 6. High-level module catalogue

Catalogue rows identify capability domains, not approved SKU boundaries or detailed breakdowns for Phases 04–12. Packaging/dependencies require D-04 and subsequent phase specifications.

| Domain | First roadmap phase | High-level responsibility | Boundary / dependency consideration |
| --- | --- | --- | --- |
| Core platform | 01 | Tenants, companies, users, roles, audit history | Required administration foundation for all modules. |
| Subscription engine | 02 | Catalogue, plans, entitlements, provider billing and changes | Owns commercial access, never tenant sales ledgers. |
| CRM and sales | 03 | Leads, customers, pipeline, quotations and invoicing | Company-owned business records; quotation conversion preserves traceability. |
| UAE e-invoicing | 04 | Structured invoice exchange/reporting integration and status | Builds on Phase 03 invoice identity; ASP integration and legal scope pending. |
| Accounting and VAT | 05 | Financial books, controls, tax evidence and reporting | Owns accounting postings; invoice-to-ledger transition needs explicit design. |
| Procurement and inventory | 06 | Purchasing, suppliers and stock management | Financial and stock valuation boundaries must reconcile with accounting. |
| HRMS and payroll | 07 | Employee administration and payroll processing | Restricted personal/pay data; jurisdiction-specific rules need validation. |
| Projects, expenses and fixed assets | 08 | Project activity/costs, expense processing and asset lifecycle | Posting and approval contracts with finance; detailed decomposition deferred. |
| Banking and advanced finance | 09 | Banking connectivity, reconciliation and advanced financial capabilities | Bank coverage and regulated activity boundaries remain pending. |
| Reporting and integrations | 10 | Cross-module analytics and external interoperability | Tenant/company permission enforcement also applies to aggregates and exports. |
| Enterprise readiness | 11 | Enterprise governance, capacity and operational readiness | Features such as SSO require explicit selection in phase specification. |
| Production deployment | 12 | Operational launch and production acceptance | Release gate, not automatic activation of every future capability. |

## 7. Complete approved development roadmap

All 13 phases below are confirmed (C), including the listed Phase 00–03 module breakdown. No dates, durations or Phase 04–12 module breakdowns are approved by this master.

| Phase | Approved name | Approved breakdown |
| --- | --- | --- |
| 00 | FOUNDATION | 1. Project Structure; 2. Development Environment; 3. Core Configuration; 4. CI/CD Foundation |
| 01 | CORE PLATFORM | 1. Tenant Management; 2. Company Management; 3. User Management; 4. Role & Permissions; 5. Audit Logging |
| 02 | SUBSCRIPTION ENGINE | 1. Module Registry; 2. Subscription Plans; 3. Module Entitlements; 4. Subscription Billing; 5. Upgrade & Downgrade |
| 03 | CRM, SALES & INVOICING | 1. CRM; 2. Lead Management; 3. Customer Management; 4. Sales Pipeline; 5. Quotations; 6. Invoicing |
| 04 | UAE E-INVOICING | Detailed module breakdown reserved for the Phase 04 specification. |
| 05 | ACCOUNTING & VAT | Detailed module breakdown reserved for the Phase 05 specification. |
| 06 | PROCUREMENT & INVENTORY | Detailed module breakdown reserved for the Phase 06 specification. |
| 07 | HRMS & PAYROLL | Detailed module breakdown reserved for the Phase 07 specification. |
| 08 | PROJECTS, EXPENSES & FIXED ASSETS | Detailed module breakdown reserved for the Phase 08 specification. |
| 09 | BANKING & ADVANCED FINANCE | Detailed module breakdown reserved for the Phase 09 specification. |
| 10 | REPORTING & INTEGRATIONS | Detailed module breakdown reserved for the Phase 10 specification. |
| 11 | ENTERPRISE READINESS | Detailed module breakdown reserved for the Phase 11 specification. |
| 12 | PRODUCTION DEPLOYMENT | Detailed module breakdown reserved for the Phase 12 specification. |

ROAD-01 (P): Each phase specification must define entry conditions, detailed scope, dependency contracts, acceptance evidence, security/regulatory checks and an explicit human completion decision before the next phase. Foundational security, testing, tenant isolation and auditability apply from the first affected phase; Phase 11 is not permission to defer them.

ROAD-02 (D): Phase 03 invoices precede Phase 05 accounting/VAT. Define a legally reviewed minimum tax-document contract in Phase 03 and the later accounting handoff without implementing Phase 05 early. Until validated, draft/pro forma use and restrictions on live tax invoices must be explicit. Phase 04 relies on that same contract. This is an unresolved sequencing dependency, not a roadmap change.

ROAD-03 (D): Subscription billing in Phase 02 can create provider tax obligations before Phase 04/05. Select an appropriate compliant provider/manual operating boundary before live charges. Technical completion of a phase does not authorise commercial release.

## 8. High-level functional scope

| ID / status | Required product outcome | Proposed acceptance boundary |
| --- | --- | --- |
| FUN-01 C/P | Organisation registration and lifecycle | Verified administrator establishes tenant; partial onboarding is recoverable; duplicate actions do not create duplicate tenants. Provisioning, suspension and closure have auditable states. |
| FUN-02 C/P | Company administration | Manage legal/display names, address, licensing authority/number/expiry, emirate, tax registration status and identifiers where applicable, fiscal settings and base currency. Branch/legal entity distinctions remain pending. |
| FUN-03 C/P | Users, memberships and permissions | Invite, accept, revoke and deactivate memberships; assign roles within authorised company scope; prevent privilege escalation and accidental removal of all administrative recovery paths. |
| FUN-04 C/P | CRM, leads and sales pipeline | Track owners, lead progression, activities and conversion to customer/opportunity without losing history. Stages and duplicate handling require module specifications. |
| FUN-05 C/P | Quotations and invoices | Version quotations, record acceptance, convert with source references; distinguish draft, issued, credited/cancelled and payment states. Issued financial documents require controlled corrections, not silent edits. |
| FUN-06 C/P | Accounting and VAT | Trace source documents to balanced postings and reports; define chart of accounts, periods, receivables/payables, corrections, close controls and tax treatment in Phase 05. No posting to a closed period without authorised procedure. |
| FUN-07 C/P | Procurement and inventory | Trace purchasing/receipts/supplier documents and stock movements; validate quantities and valuation consistency; negative-stock and valuation policies are pending phase decisions. |
| FUN-08 C/P | HR and payroll | Secure employee records, approved payroll runs, payslips and payment output/reconciliation; validate leave, termination, benefits and applicable WPS rules before release. |
| FUN-09 C/P | Projects, expenses and fixed assets | Record accountable ownership, approvals and finance references; define cost capture, expense reimbursement and depreciation policy in Phase 08. |
| FUN-10 C/P | Banking and finance | Import/connect statements, match and reconcile with reviewed exceptions; bank connectivity and payment initiation are separately authorised capabilities. |
| FUN-11 C/P | Reporting and analytics | Reconcile reports to their source records and show company, period, currency and as-of context; exports must enforce the same access rules as screens. |
| FUN-12 P | Import, export and lifecycle | Validate imports with row-level errors and duplicate protection; authorise exports and log sensitive downloads. Define export formats and closure/deletion process before customer onboarding. |

Workflow example (P): authorised company user creates quotation → authorised issue action creates immutable invoice identity → Phase 04 connector tracks external submission separately → Phase 05 records accounting posting and reconciliation. Sales status, provider acknowledgement and ledger posting are distinct states; failures remain visible and recoverable.

## 9. Non-functional requirements and financial integrity

| ID / status | Expectation | Verification required in later delivery |
| --- | --- | --- |
| NFR-01 C/P | Security and isolation | Negative tests across tenant/company identifiers, list/detail endpoints, exports, files, tasks and reports; deny by default. |
| NFR-02 C/P | Reliability and transactional integrity | Atomic business-critical state changes; duplicate/retry/concurrency scenarios cannot double-post, double-charge or corrupt stock. External failures have reconciliation paths. |
| NFR-03 C/P | Financial precision | Decimal arithmetic for monetary values; currency/unit precision, calculation order, tax basis, FX precision and rounding stage/mode explicitly specified and versioned before financial workflows. Binary floating-point money is prohibited in the proposed design. |
| NFR-04 C/P | Maintainability and modularity | Document ownership/contracts, isolate domain rules, review migrations and preserve compatibility for persisted financial history. |
| NFR-05 C/P | Responsive, usable bilingual experience | Primary flows work at agreed desktop/mobile viewports, keyboard access, readable validation and English/Arabic layouts; proposed accessibility target WCAG 2.2 AA awaits D-14. |
| NFR-06 C/P | Performance and scale | Define tenant counts, concurrent users, record volumes and p95 response targets per workflow; load-test against the approved profile. No unsupported capacity claim. |
| NFR-07 C/P | Recovery and availability | Approve service availability, RPO, RTO, backup retention and restore test cadence before production; demonstrate restore, including tenant access correctness. |
| NFR-08 C/P | Observability and auditability | Correlation across request/task/provider event; actionable alerts; no secrets or unnecessary personal/financial payloads in operational logs. |
| NFR-09 P | Time and repeatability | Store unambiguous timestamps; use explicit business dates/timezones for periods, payroll and renewals; scheduled operations cannot depend on server local time. |

FIN-01 (P): Money acceptance cases must cover line versus document rounding, discounts before/after tax, inclusive/exclusive prices, credit notes, partial payments, currencies and FX differences. For illustration only, AED 100.00 at an applicable 5% tax rate yields AED 5.00 tax and AED 105.00 gross before discounts. This example does not classify any real supply or establish a universal rate.

FIN-02 (P): Posted records preserve source, actor, company, effective date and policy version. Corrections use traceable reversal/adjustment workflows. Number uniqueness, sequence scope/gap treatment, posting locks and reconciliation tolerances require explicit approved rules.

## 10. Multi-tenancy and company principles

TEN-01 (C): Each tenant has independent company information, memberships/users, module subscriptions, business records, configuration, financial records and audit history. Another tenant must never access those records except through explicitly authorised platform-level functionality.

TEN-02 (C/P): Enforce isolation in backend authorisation and database-access paths. Resolve trusted tenant context from authenticated, active membership; never trust a submitted tenant ID by itself. Reads, writes, relationships, bulk operations, raw queries and administrative operations must remain tenant-scoped. Referencing a record in another tenant must fail without leaking its contents.

TEN-03 (P): Company-level authorisation is an additional boundary inside a tenant. Shared tenant data and company-owned data must be explicitly classified. Membership of a tenant is insufficient for access to every company. Cross-company reports require explicit grants; intercompany accounting and consolidation are pending enterprise scope.

TEN-04 (D): Shared tables with tenant keys, schema-per-tenant, database-per-tenant, row-level security and any combination require an ADR with isolation, connection pooling, migration, backup/restore, cost and operational evidence. None is selected here. A shared-table option must consider database constraints preventing cross-tenant relationships; application filtering alone is not an adequate evidence package.

TEN-05 (P): The boundary extends to cache keys, Redis use, background jobs, file/object storage, search indexes, exports, logs, analytics and provider credentials. Jobs must carry validated scope, recheck permissions/entitlements as appropriate, and never inherit mutable request-global tenant state.

TEN-06 (P): Tenant suspension/closure, membership revocation, restores and data imports must preserve isolation. Platform aggregate metrics require a defined lawful purpose and minimised data. Support access follows SEC-03.

## 11. Security principles

SEC-01 (C/P): Require authentication, active membership, company scope, action permission and applicable entitlement on protected operations. UI hiding is only usability. Service accounts and API integrations have bounded scopes and revocable credentials.

SEC-02 (P): Protect sessions and credentials; require transport encryption, secure secret storage, secure cookies, CSRF protection for browser flows, input validation and safe rendering. Define password reset, invitation expiry, session revocation, login abuse controls and MFA policy before onboarding. MFA for privileged operators is recommended (R).

SEC-03 (P): Platform support access requires an explicit approved purpose, scope, time limit and audit record. Impersonation/break-glass capabilities are not default privileges; decide consent, approval, emergency procedure and tenant visibility under D-08.

SEC-04 (C/P): Audit sensitive changes: permissions, memberships, company/tax settings, subscription terms, payment events, financial issue/post/reversal, payroll approvals, integrations and support access. Capture actor/service, tenant/company, action, target, timestamp, outcome and safe before/after changes. Protect audit records from ordinary modification; audit retention is a separate approved policy.

SEC-05 (P): Classify personal, payroll, financial and secret data; minimise collection; enforce restricted exports and encryption at rest through the selected storage design. Threat modelling, dependency/security checks and remediation gates precede production. Do not claim ISO, SOC, PCI or government certification without separate evidence.

## 12. UAE localisation

LOC-01 (C/P): AED is supported as the primary UAE currency. Multi-currency operations and FX sources are pending D-10; do not imply AED is the only currency the future architecture can handle.

LOC-02 (C/P): English and Arabic must cover navigation, forms, validation, critical workflows and agreed business outputs, including applicable PDFs and notification emails. Arabic uses RTL layouts with correct mixed-direction identifiers, email, account numbers and amounts. Preserve entered legal names; translated UI must not alter legal data. Translation ownership and review belong to D-10.

LOC-03 (C/P): Use `Asia/Dubai` as the proposed UAE business timezone, distinct from authoring timezone. Support locale-aware number/date display and unambiguous machine exchange. Define preferred numeral style, calendar display, date input and bilingual document templates under D-10; never use ambiguous display strings as financial storage.

LOC-04 (C/P): Support licensing authority, emirate, business identifiers and tax registration details where relevant. Do not assume every free zone has the same tax or employment regime. Record legal entity, branch and registration relationships only after D-01/D-09 validation.

LOC-05 (C): Consider UAE VAT, e-invoicing, corporate tax, payroll/employment, emirate-specific requirements and local payments/banking. Language support does not itself establish legal acceptability of an invoice or filing.

## 13. Regulatory considerations and evidence register

Verification date: 2026-11-03. Sources below are official public sources reviewed for high-level product relevance. Scope is not an exhaustive consolidated legal opinion. Rules must be rechecked at detailed specification and again before release. A qualified UAE reviewer must determine customer/jurisdiction applicability, effective dates, exemptions and output validity. No compliance certification is claimed.

| ID | Official evidence (E) | Product consequence (P) | Outstanding validation / gate |
| --- | --- | --- | --- |
| REG-01 VAT | FTA [VAT registration guidance](https://www.tax.gov.ae/en/taxes/Vat/vat.topics/registration.for.vat.aspx) describes resident mandatory registration at AED 375,000 and voluntary registration at AED 187,500, with time tests and non-resident distinctions. | Model tax registration status and effective-dated tax treatment; avoid universal threshold/rate assumptions. | Before live tax invoicing: current invoice/credit-note fields, deadlines, rounding, rates, zero-rated/exempt/reverse-charge cases, designated zones, currencies, tax groups, return mappings and record retention. |
| REG-02 E-invoicing | MoF [official e-invoicing portal](https://mof.gov.ae/en/about-us/initiatives/einvoicing/) distinguishes structured invoices from PDFs and describes PINT AE exchange through ASPs. Its legislation list includes Decisions 243/2025, 244/2025 and 2026 amendments including 66, 197, 168 and Cabinet Decision 173. | Keep structured payload, transmission/reporting evidence and external statuses distinct from a rendered invoice. Integrate through a selected ASP subject to validation. | Consolidate operative texts, exclusions, current timeline, schema/mandatory fields, credit notes, inbound processing, failure/retry duties, archival obligations and provider certification before Phase 04 acceptance. Merely listing an amendment is not verification of its detailed effects. |
| REG-03 Corporate tax | FTA [record retention announcement](https://tax.gov.ae/en/media.centre/News/pr.28082025.aspx) states at least seven years after the relevant tax period for relevant taxable/exempt-person records. | Preserve traceable financial history and configurable legally reviewed retention with holds. | Define corporate-tax support boundary, adjustments, free-zone/entity treatment and filing expectations in Phase 05; do not equate accounting profit with taxable income. |
| REG-04 Employment/payroll | UAE Government [payment of wages guidance](https://u.ae/en/information-and-services/jobs/employment-in-the-private-sector/payment-of-wages) refers to Resolution 340/2026 and WPS with exclusions; CBUAE [UAEWPS overview](https://centralbank.ae/en/our-operations/payments-and-settlements/uae-wages-protection-system-uaewps) describes payment-agent flows. | Use versioned payroll rules and selected bank/agent output validation; track output acceptance and actual payment separately. | Determine mainland/free-zone regime, current WPS format/timing/exemptions, leave, overtime, end-of-service, pension and other applicable employment rules before Phase 07 release. Do not reuse older penalty/grace schedules without verification. |
| REG-05 Provider billing | Same tax/e-invoicing authorities as REG-01/02, applied to the SaaS provider's own activity. | Keep provider billing compliance review separate from customer module capabilities. | Before Phase 02 live billing, validate provider legal/tax status and chosen billing operating model. |
| REG-06 Privacy | UAE Government [data protection guidance](https://u.ae/en/about-the-uae/digital-uae/data/data-protection-laws) identifies Federal Decree-Law 45/2021 and data-processing/privacy obligations. | Define controller/processor responsibilities, data inventory, access controls, retention and rights-request handling. | Before collecting customer data: applicability/exemptions, DIFC/ADGM or other applicable regime, lawful basis, processor contracts, breach handling and cross-border transfers. No blanket UAE-only hosting obligation is asserted. |
| REG-07 VAT retention | FTA [invoice retention FAQ](https://tax.gov.ae/en/faq.aspx?keyword=How+long+must+a+taxable+person+retain+VAT+invoices+for%3F) describes a minimum five-year period. | Retention cannot be one global delete-after-five-years setting; corporate-tax overlap and special cases can require longer. | Validate current consolidated legislation, record categories, period start, extensions, real estate and legal holds before adopting any schedule. |

Regulatory timing warning (E/P): MoF's [10 May 2026 announcement](https://mof.gov.ae/en/news/ministry-of-finance-announces-targeted-amendments-to-einvoicing-system-decisions/) reports an ASP appointment extension to 30 October 2026 for the large-revenue cohort and a 1 January 2027 implementation date. Older 2025 guidance contains an earlier appointment date. The current portal lists subsequent amendments; this master deliberately does not reproduce a definitive cohort timetable or resolve the exact AED 50 million boundary from a news summary. D-09 requires reading the consolidated legal texts. As of this baseline date, regulatory deadlines may precede product readiness; no release date or customer eligibility promise is authorised.

REG-GATE (P): Maintain a future compliance matrix linking legal source/version, applicable entity/transaction, product requirement, test/output evidence, reviewer and revalidation date. Unverified items block claims of applicable compliance and relevant live workflows; they do not authorise changing the roadmap.

## 14. Technical direction and architectural consistency

The following intended direction is confirmed (C); component introduction, versions and detailed architecture remain pending design review. No dependencies have been installed and no application structure has been generated.

| Area | Intended direction | Decision boundary |
| --- | --- | --- |
| Backend | Python and Django | Supported versions, domain boundaries and tenancy ADR pending. |
| Database | PostgreSQL | Isolation model, constraints, extensions and backup architecture pending. |
| Frontend | Django Templates, CSS and JavaScript | No SPA framework replacement is approved. |
| API | Django REST Framework where appropriate | Add only for defined integration/product needs; not every screen needs an API. |
| Background processing | Celery | Introduce when durable asynchronous work is specified; task safety/tenant scope required. |
| Broker/cache | Redis | Roles, persistence assumptions and separation require design; cache is not the financial source of truth. |
| Application server | Gunicorn | Deployment configuration selected later. |
| Reverse proxy | Nginx | TLS termination, static/media handling and trusted proxy rules selected later. |
| Containers | Docker | Development/production image and runtime policies selected in phase specifications. |
| Testing | pytest and pytest-django | Apply meaningful tests with each affected capability. |
| CI/CD | GitHub Actions | Repository/hosting and credentials not yet configured. |

ARCH-01 (R): Start with a modular monolith aligned to domain ownership and documented interfaces. Avoid premature microservices and unnecessary infrastructure complexity (C). This recommendation is not an approved tenancy or deployment ADR.

ARCH-02 (P): Module boundaries must prevent circular dependencies and duplicate ownership of invoices/parties/ledger entries. Shared identifiers and versioned contracts should permit future modules without rewriting core administration. Physical code layout and shared-party design are pending, not implemented.

ARCH-03: No conflict with existing architecture could be assessed because none exists in the available workspace. If prior materials are later supplied, review them before implementation and record discrepancies. Do not treat absence as evidence that prior decisions never existed elsewhere.

## 15. Integration strategy

INT-01 (C/P): Plan for UAE e-invoicing, local payments/banking and future business integrations. Provider selection, supported banks, data contracts, costs, sandboxes and production access are D-11. Do not assume direct FTA, bank or government API access exists.

INT-02 (P): Each connector requires scoped credentials, tenant/company ownership, versioned payload mapping, timeouts, bounded retries, idempotency, external correlation IDs, reconciliation and visible exception handling. Authenticate callbacks and reject replay/duplicate delivery. External acknowledgements cannot be inferred from HTTP transport success alone.

INT-03 (P): Preserve source business state independently of connector state; avoid holding database transactions open during remote calls. Reliable dispatch pattern selection is an architecture decision. Provider outages must not lose or duplicate financial events.

INT-04 (P): Approve data minimisation, transfers, logging and retention per integration. Prefer hosted/tokenised payment collection; raw card-data handling is excluded unless explicitly approved with its own scope. Public APIs, partner ecosystems and event delivery contracts require Phase 10 decisions; earlier required connectors remain within their relevant phases.

## 16. Deployment philosophy

DEP-01 (C/P): Use a simple, maintainable deployment consistent with intended Docker/Gunicorn/Nginx/PostgreSQL direction. Introduce Celery/Redis and other components only when justified. Hosting provider, region, managed versus self-managed services and topology are D-12. Kubernetes/microservices are not presumed requirements.

DEP-02 (P): Separate development, test/staging and production data/secrets. CI gates should cover relevant tests, migration checks, dependency/security review and reproducible builds. Production promotion requires approval, configuration validation and a rollback/recovery plan that accounts for irreversible data changes.

DEP-03 (P): Before Phase 12 acceptance, demonstrate backups/restores, monitoring/alerts, incident response, privileged access control, operational runbooks, tenant-safe onboarding/offboarding and approved service objectives. Use synthetic or appropriately sanitised test data; production data copying needs explicit controls.

## 17. Scalability strategy

SCALE-01 (C/P): Design for growth without promising unmeasured scale. Establish D-14 capacity profiles covering tenants, companies, concurrent sessions, invoices, ledger/stock volume, payroll sizes and integrations before capacity claims.

SCALE-02 (R): Optimise indexed tenant/company access paths, pagination, bounded exports and query counts; use asynchronous processing for justified long-running tasks. Measure before adding replicas, partitioning, dedicated tenant deployments or service extraction.

SCALE-03 (P): Prevent noisy-neighbour exhaustion with reviewed limits, task quotas and fair scheduling. Cache isolation and invalidation must preserve permission/entitlement changes. Consistency requirements for financial reporting take precedence over unreviewed caching or replica lag.

## 18. Product limitations and exclusions

LIM-01 (C): This task creates documentation only: no models, views, interfaces, APIs, migrations, package installation, infrastructure deployment or unrelated code changes. Prompt 01 is not started.

LIM-02 (P): Detailed Phase 04–12 module decomposition, prices, tax/payroll calculation rules, hosting selections, service targets and launch dates are reserved for approved follow-up specifications.

LIM-03 (P): Native mobile apps, offline operation, manufacturing, industry-specific regulated workflows, non-UAE localisation, automatic tax filing, unrestricted payment initiation, custody/lending, intercompany consolidation, enterprise SSO and custom customer code are not committed features. They may be proposed through controlled scope decisions; Phase 09/11 names do not automatically include all such capabilities.

LIM-04: No legal advice, regulatory accreditation, ASP status, bank partnership, security certification, production readiness or operational compliance is established by this specification. Reports/exports must be described according to validated scope.

## 19. Conflicts, dependencies and assumptions

| Reference | Finding | Disposition |
| --- | --- | --- |
| CF-01 | No repository history/documents available. | Record evidence limitation; no architecture silently replaced. Reconcile later-provided materials before implementation. |
| CF-02 | Phase 03/04 financial documents precede Phase 05 VAT/accounting. | D-06 defines invoice/tax contract and posting handoff; preserve roadmap. |
| CF-03 | Phase 02 provider billing precedes ERP compliance modules. | REG-05/D-07 validation before live charges; no phase reorder. |
| CF-04 | Commercial module deactivation can conflict with legal retention and active obligations. | D-05/D-09 define access, workflow and retention treatment. Never equate cancellation with data deletion. |
| CF-05 | Official summaries contain older dates; later e-invoicing amendments exist. | D-09 requires consolidated legal validation; do not adopt outdated dates or infer applicability. |
| A-02 | A tenant may administer multiple companies under a common commercial account. | Confirm billing/legal-entity mapping in D-01/D-04; this is not approval of cross-company data access. |
| A-03 | Initial delivery is browser-based, with general UAE business workflows. | Consistent with template frontend direction; validate target browser and customer cohort in D-03/D-14. |

## 20. Pending decisions and resolution gates

Owners below are proposed accountable roles, not evidence that a reviewer has already approved an item.

| ID | Decision / required evidence | Proposed owner | Resolve before |
| --- | --- | --- | --- |
| D-01 | Tenant/company/legal entity/branch hierarchy; tax groups; company limits and billing entity relationship | Product + UAE finance/legal reviewer | Core data architecture / Phase 01 detailed specification |
| D-02 | Identity scope, memberships, invitation/recovery and company-scoped role model | Technical architect + security | Phase 01 implementation |
| D-03 | Initial industries, customer cohort, mainland/free-zone coverage and onboarding eligibility | Product owner | Scope commitment / first onboarding |
| D-04 | SKU/dependency matrix; pricing units; tenant/company entitlements; core inclusion; limits and bundles | Product + commercial owner | Phase 02 detailed specification |
| D-05 | Proration, trials, renewals, refunds, discounts, grace/suspension, read/export and queued-work rules | Commercial + finance/legal | Phase 02 live billing |
| D-06 | Phase 03 minimum invoice/tax contract; Phase 04 requirements; Phase 05 posting handoff and rounding | Product + finance + technical architect | Phase 03 design / live document issuance |
| D-07 | Provider subscription billing legal/tax operating model and payment provider | Provider finance/legal | First live charge |
| D-08 | Tenancy ADR; platform support/break-glass; MFA; audit/secret management and revocation | Technical architect + security | Core implementation / access to customer data |
| D-09 | Consolidated UAE compliance matrix: tax, current e-invoicing texts/timeline, payroll, privacy, retention/holds, entity exceptions | Qualified UAE regulatory/legal reviewers | Affected detailed specification and live release |
| D-10 | Locale templates, numeral/date preferences, currencies, FX source/rules and fiscal calendar | Product + finance + localisation | Relevant workflow specifications |
| D-11 | ASP/payment/bank providers; contracts, APIs, onboarding, sandbox acceptance and support | Integration lead + commercial/legal | Connector commitment / live integration |
| D-12 | Hosting region/provider, data transfers, storage, backup architecture and operational responsibility | Technical + security/legal + operations | Deployment architecture / customer data storage |
| D-13 | Detailed later-phase scope, dependency contracts, acceptance gates and enterprise feature selection | Product owner + domain leads | Each Phase 04–12 specification |
| D-14 | Commercial success targets, workload/capacity profiles, response targets, accessibility/browser criteria, availability/RPO/RTO and recovery cadence | Product + engineering + operations | Relevant acceptance plans; all production objectives before Phase 12 |

## 21. Review record and master acceptance

This baseline was prepared from the supplied prompt and actual empty-workspace inspection. On 2026-11-03, the Product Architect/Business Analyst agent and Technical Architect agent independently inspected the workspace, reviewed the prompt, and then read this saved specification. Both final reviews reported no blocking defects. The product review confirmed all 20 requested coverage areas and the exact roadmap; the architecture review confirmed pending tenancy choices, intended stack consistency and sequencing dependencies. Their review-record and licensing-reference corrections were incorporated. The primary agent verified 13 consecutive phase rows (00–12), checked completeness and official-source attribution, and confirmed that this is the only workspace file. Regulatory source review supports the evidence register only; no independent UAE compliance agent or qualified legal sign-off was performed, and D-09 remains pending.

| Master-task criterion | Evidence / disposition |
| --- | --- |
| Repository inspected; available documentation reviewed | Section 1: full available workspace empty; requested legacy documents absent. |
| Master product specification created | This file; no duplicate product documents created. |
| Complete 13-phase roadmap preserved | Section 7; exact Phase 00–03 breakdown; later breakdowns deferred. |
| Commercial model and UAE requirements defined | Sections 5, 12–13. |
| Multi-tenancy and technical consistency addressed | Sections 10 and 14; no unsupported architecture selection. |
| Assumptions, decisions and conflicts explicit | Status legend and sections 19–20. |
| Completeness reviewed | Actual-file product/business and architecture reviews completed on 2026-11-03; primary documentation review incorporated findings. No blocking defects reported. |
| Documentation-only boundary respected | Only this Markdown specification is created by this task. |

Human review of proposed details, architecture ADRs and legal validations remains outstanding and does not prevent completion of this documentation baseline.
