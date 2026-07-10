# Zero-Trust Workload Architecture (ZTWA) - Model 1

A cryptographic, secretless machine-to-machine trust framework. Disables legacy static passwords by leveraging OpenID Connect (OIDC) identity federation and cloud-native scoped RBAC to authorize automated runners directly to target data plane resources under a least-privilege paradigm.

---

## 1. Sequence Flow Map

```text
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
