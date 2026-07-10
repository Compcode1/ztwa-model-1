[ YOUR MASTER FRAMEWORK MODEL ]
                       
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
The Universal Framework Mechanics
This model strips away the specific project names and exposes the absolute engineering core that applies to almost all modern cloud automation architecture:

The Inbound Claim (Step 1 & 2): The external platform runs its execution script and sends a dynamic signature. The identity platform doesn't check a password; it checks the path of where the call came from against a pre-configured rule (the trust anchor).

The Cryptographic Pivot (Step 3 & 4): Once the path is validated, the identity plane converts that external identity into an internal token. This token acts as a temporary, single-use key.

The Isolated Execution (Step 5): The runner uses that key to punch straight into the data resource. Because access is governed entirely by Role-Based Access Control (RBAC) rules instead of permanent account keys, the security boundary is flawless.
