## 🧠 **Helm CLI Cheatsheet**

### 🔍 **Working with Repositories**

| Command                      | Description                              |
| ---------------------------- | ---------------------------------------- |
| `helm repo add <name> <url>` | Add a new chart repository               |
| `helm repo list`             | List all added repositories              |
| `helm repo update`           | Update local cache of chart repositories |
| `helm search repo <keyword>` | Search for charts in added repos         |

---

### 📦 **Installing and Managing Charts**

| Command                              | Description                              |
| ------------------------------------ | ---------------------------------------- |
| `helm install <release> <chart>`     | Install a chart as a release             |
| `helm upgrade <release> <chart>`     | Upgrade a release                        |
| `helm uninstall <release>`           | Uninstall/delete a release               |
| `helm rollback <release> <revision>` | Roll back to a previous release revision |

---

### 📄 **Release Info and Status**

| Command                       | Description                                    |
| ----------------------------- | ---------------------------------------------- |
| `helm list`                   | List releases (use `--all-namespaces` for all) |
| `helm status <release>`       | Show status of a release                       |
| `helm get all <release>`      | Show full release info                         |
| `helm get values <release>`   | Show values used in the release                |
| `helm get manifest <release>` | Show generated Kubernetes manifest             |

---

### ⚙️ **Template and Linting**

| Command                 | Description                     |
| ----------------------- | ------------------------------- |
| `helm lint <chart>`     | Lint a chart (check for issues) |
| `helm template <chart>` | Render chart templates locally  |

---

### 🧪 **Debugging and Testing**

| Command                          | Description                          |
| -------------------------------- | ------------------------------------ |
| `helm test <release>`            | Run tests for a release              |
| `helm upgrade --dry-run --debug` | Simulate upgrade (no actual changes) |

---

### 📁 **Chart Management**

| Command                    | Description                           |
| -------------------------- | ------------------------------------- |
| `helm create <chart-name>` | Create a new chart scaffold           |
| `helm package <chart-dir>` | Package a chart directory into `.tgz` |
| `helm show values <chart>` | Show default values of a chart        |


