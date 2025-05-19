Introduction to Helm:

    Problem in Kubernetes:

        Managing complex applications involves multiple Kubernetes objects (e.g., deployments, services, volumes, secrets).

        Each object typically has its own YAML file, making it tedious to manage, update, or delete multiple objects.

        Manual updates across multiple files can lead to errors and inconsistencies.

What is Helm?

    Helm is a package manager for Kubernetes that simplifies the deployment, management, and maintenance of applications in Kubernetes.

    It treats multiple Kubernetes objects as part of a single package, called a Helm chart.

How Helm Simplifies Kubernetes Management:

    Packaging Objects as a Single Chart:

        Helm groups related Kubernetes objects (e.g., deployment, service, secrets) into a single package called a chart.

        Instead of managing each object individually, Helm manages them as a unit.

    Centralized Configuration (values.yaml):

        Custom settings (e.g., storage size, admin passwords) can be specified in a single values.yaml file.

        This avoids editing multiple YAML files and centralizes application configuration.

    Single Command Deployment:

        Install all objects of an application with a single command, e.g., helm install wordpress.

        Helm automatically deploys all objects defined in the chart without requiring individual kubectl apply commands.

    Upgrade and Rollback Management:

        Helm keeps track of application revisions, allowing users to upgrade or rollback the application using a single command.

    Uninstallation Simplified:

        Helm tracks all objects associated with a particular release, enabling complete removal with a single command, e.g., helm uninstall wordpress.

Analogy - Game Installer:

    Game Installation: Instead of downloading and placing each game file manually, we use a game installer that organizes everything for us.

    Helm: Similar to a game installer, Helm installs, updates, and manages Kubernetes objects as a single package (a chart), without manual intervention.

Roles of Helm:

    Package Manager: Simplifies application deployment through Helm charts.

    Release Manager: Manages application versions, allowing rollbacks and upgrades.



Helm Versions and History:

    Helm 1.0: Released in February 2016.

    Helm 2.0: Released in November 2016.

    Helm 3.0: Released in November 2019.

    Kubernetes advancements facilitated improvements in Helm, especially in Helm 3.

Key Differences Between Helm 2 and Helm 3:

    Tiller Component:

        Helm 2: Required Tiller as a middleman for performing actions. Tiller had high privileges, raising security concerns.

        Helm 3: Removed Tiller. Helm directly interacts with Kubernetes, reducing complexity and security risks.

    Role-Based Access Control (RBAC):

        Helm 2: Managed permissions through Tiller, which was less secure.

        Helm 3: Utilizes Kubernetes' native RBAC, allowing fine-tuned control over user actions.

    Three-Way Strategic Merge Patch:

        Helm 2: Compared the current chart with the previous chart to handle rollbacks. Manual changes made outside Helm were not detected.

        Helm 3: Compares three elements — the current chart, previous chart, and live state of Kubernetes objects.

            Allows Helm 3 to identify manual changes and handle rollbacks or upgrades more accurately.

    Revisions and Rollbacks:

        Each deployment creates a revision (e.g., Revision 1, Revision 2, etc.).

        Rollbacks in Helm 3 consider manual changes and revert accordingly, unlike Helm 2, which only compared chart states.

    Upgrade Handling:

        Helm 2: Overwrites changes without checking the live state.

        Helm 3: Considers the live state to preserve custom changes made outside of Helm during upgrades.
