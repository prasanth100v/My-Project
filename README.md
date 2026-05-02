* shift left security  --  integrating security testing earliest phases of the software development lifecycle (`SDLC`) (`Fixing bugs during development`) developers identify vulnerabilities during coding and design, it reduces costs and speeds up releases.

* (SBOM)  --  Software Bill of Materials (`SBOM`) is a list of everything inside your software — `all libraries`, `dependencies`, `versions`, and `components`. (in a machine-readable format) (helping improve security, transparency, and vulnerability management.)

## 🔐 DevSecOps Security Pipeline (Shift-Left)
| 🔢 **Step**            | 🛠 **Tool** | 🎯 **Purpose**       | 🧠 **What It Does**                                                          | 💡 **Why It Matters**           |
| ---------------------- | ----------- | -------------------- | ----------------------------------------------------------------------------- | -------------------------------- |
| 🔍 **Secret Scan**     | Gitleaks    | Find exposed secrets | 👉 Scans code for API keys, passwords, tokens                                | Prevents credential leaks        |
| 🛡 **Dependency Scan** | Trivy       | Find vulnerabilities | 👉 Detects CVEs ( Common Vulnerabilities and Exposures) in packages & images  | Reduces attack surface           |
| 📦 **SBOM Generation** | Syft        | List all components  | 👉 Generates Software Bill of Materials (SBOM) (libraries, dependencies, versions) | Improves security & vulnerability management |

* Anchore -- is a `container security platform` that uses `Syft` for `SBOM` generation and `Grype` for `vulnerability scanning`, enabling policy-based security enforcement in `CI/CD pipelines` .

## ⚙️ Anchore Components

| 🧩 **Component**      | 📖 **Purpose**        | 🧠 **How It Works**                                  | 💡 **Why It Matters**           |
| --------------------- | --------------------- | ------------------------------------------------------ | ------------------------------- |
| 🔍 **Anchore Engine** | Full platform         | 👉 Image analysis + enforces security policies       | Blocks insecure images in CI/CD |
| 📦 **Syft**           | SBOM generator        | 👉 Extracts all packages from container image         | Visibility into dependencies    |
| 🛡 **Grype**          | Vulnerability scanner | 👉 Matches SBOM packages with CVE databases            | Detects known vulnerabilities   |


* 🔐 Why did you choose shift-left security?
    * Catch issues early → `cheaper to fix`
    * Reduces `production vulnerabilities`
    * Improves `developer accountability`

* 📜 What is SBOM and why did you generate it?
    * List of all dependencies
    * Helps in vulnerability tracking

* 🐳 How do you secure Docker images?
    * Minimal base images (`Alpine`)
    * Scan images (Trivy)
    * Remove unused packages
    * Use `non-root user`

* ☁️ Why use Amazon EKS instead of ECS?
    * Kubernetes flexibility
    * Better ecosystem (`Helm`, `GitOps`, `service mesh`)
