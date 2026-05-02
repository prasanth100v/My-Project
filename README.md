* shift left security  --  integrating security testing earliest phases of the software development lifecycle (SDLC) (Fixing bugs during development) developers identify vulnerabilities during coding and design, it reduces costs and speeds up releases.

* (SBOM)  --  Software Bill of Materials (SBOM) is a list of everything inside your software—all libraries, dependencies, versions, and components. (in a machine-readable format) (helping improve security, transparency, and vulnerability management.)

## 🔐 DevSecOps Security Pipeline (Shift-Left)
| 🔢 **Step**            | 🛠 **Tool** | 🎯 **Purpose**       | 🧠 **What It Does**                                                          | 💡 **Why It Matters**           |
| ---------------------- | ----------- | -------------------- | ----------------------------------------------------------------------------- | -------------------------------- |
| 🔍 **Secret Scan**     | Gitleaks    | Find exposed secrets | 👉 Scans code for API keys, passwords, tokens                                | Prevents credential leaks        |
| 🛡 **Dependency Scan** | Trivy       | Find vulnerabilities | 👉 Detects CVEs ( Common Vulnerabilities and Exposures) in packages & images  | Reduces attack surface           |
| 📦 **SBOM Generation** | Syft        | List all components  | 👉 Generates Software Bill of Materials (SBOM) (libraries, dependencies, versions) | Improves security & vulnerability management |



