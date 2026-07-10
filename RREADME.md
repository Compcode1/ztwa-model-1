# Zero-Trust Workload Architecture (ZTWA) - Model 1

A cryptographic, secretless machine-to-machine trust framework. Disables legacy static passwords by leveraging OpenID Connect (OIDC) identity federation and cloud-native scoped RBAC to authorize automated runners directly to target data plane resources under a least-privilege paradigm.

---

## 1. Sequence Flow Map

  (Source Node)              (Identity Plane)              (Target Resource)
 GitHub Actions             Microsoft Entra ID               Azure Storage
  Execution VM               App Registration                Container Data
       │                            │                              │
       │  1. Request OIDC Token     │                              │
       ├───────────────────────────>│                              │
       │                            │                              │
       │  2. Validate Subject Claim │                              │
       │     & Evaluate Trust (FIC) │                              │
       │<───────────────────────────┤                              │
       │                            │                              │
       │  3. Issue Short-Lived      │                              │
       │     Access Token (AT)      │                              │
       ├───────────────────────────>│                              │
       │                            │                              │
       │  4. Present AT via RBAC    │                              │
       │     (--auth-mode login)    │                              │
       ├────────────────────────────┼─────────────────────────────>│
       │                            │                              │
       │                            │  5. Transact Data Payload    │
       │                            │     (Zero-Trust Write)       │
       │<───────────────────────────┼──────────────────────────────┤
       │                            │                              │

---

## 2. Universal Framework Mechanics

* **The Inbound Claim (Steps 1 & 2):** The external platform runs its execution script and sends a dynamic signature. The identity platform doesn't check a password; it checks the path of where the call came from against a pre-configured rule (the trust anchor).
* **The Cryptographic Pivot (Steps 3 & 4):** Once the path is validated, the identity plane converts that external identity into an internal token. This token acts as a temporary, single-use key.
* **The Isolated Execution (Step 5):** The runner uses that key to punch straight into the data resource. Because access is governed entirely by Role-Based Access Control (RBAC) rules instead of permanent account keys, the security boundary is flawless.
