# CompTIA Network+ N10-009 — Objective 3.1: Life Cycle Management

### 1. Hardware and Software Aging Categories
* **End of Life (EOL):** The milestone where a vendor halts the release of new software versions or feature enhancements for a product.
* **In-Bracket Definition:** `(A product status where a manufacturer stops providing new software versions or feature enhancements but may still provide security patches)`
* **End of Support (EOS):** A critical operational state where a manufacturer cuts off all updates, technical support, and security patches. Newly found security bugs remain unfixed forever, creating severe infrastructure vulnerabilities.
* **In-Bracket Definition:** `(A critical status where a product receives absolutely no updates, patches, or technical support of any kind)`

### 2. Patching, OS Updates, and Configurations
* **Update Types:** Software health relies on recurring monthly vendor patches, quarterly Service Packs, and immediate, out-of-band updates triggered to block active zero-day exploits.
* **OS Hardening:** Lifecycle maintenance requires administrators to manually push configuration updates to user accounts (enforcing password complexity and minimum length limits), adjust interface permissions, rewrite host firewalls, and update antimalware definition files.

### 3. Firmware Management
* **Definition:** The specialized internal software governing purpose-built hardware appliances (such as printers, modems, or IoT smart thermostats).
* **In-Bracket Definition:** `(The permanent software programmed into a hardware device's read-only memory that controls its basic operations)`
* **Rollback Rules:** Because a firmware failure can completely brick a device, engineers must keep a secure local archive of previous firmware binaries to execute a fallback downgrade if a new update fails.

### 4. Decommissioning Process
* **Definition:** The strict, structured retirement of outdated infrastructure components (laptops, servers, switches).
* **In-Bracket Definition:** `(The structured retirement and destruction process used to ensure old hardware doesn't leak corporate data)`
* **Data Protection:** Standard trash disposal is forbidden because it exposes data to scavengers. Drives must undergo data sanitization or physical destruction. If regulatory compliance mandates data retention, the assets must be moved to a secure offsite storage facility instead.

### 5. Change Management and Process Tracking
* **Change Management:** A central control framework requiring formal tracking, evaluation, and documentation before any engineer modifies production switches, firewalls, or routers.
* **In-Bracket Definition:** `(A strict approval framework that ensures every network change is planned, tested, and can be reversed if something breaks)`
* **Core Elements:** Every change submission must define a designated maintenance window, a step-by-step installation script, and a functional rollback plan.
* **Process Tracking:** Uses ticket tracking workflows to record user service requests from creation, triage, remediation, through final closure to build a clear infrastructure audit trail.






