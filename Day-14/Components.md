Helm Components - Overview

Helm Command Line Utility:

A command line tool installed on the local system.

Used for performing Helm actions such as installing a chart, upgrading, rollback, etc.

Charts:

A collection of files containing all the instructions Helm needs to deploy objects in a Kubernetes cluster.

Acts as a package for Kubernetes applications.

Publicly available charts can be downloaded from repositories like Bitnami, AppsCode, etc.

Artifact Hub (artifacthub.io) serves as a centralized repository for all available charts.

Release:

A single installation of an application using a Helm chart.

Each release can have multiple revisions, acting as snapshots of the application state.

Allows for multiple releases from the same chart (e.g., my-site and my-second-site can be based on the same chart but tracked independently).

Values.yaml File:

The configuration file where customizable values for a chart are defined.

Simplifies application customization by providing a single location to manage settings like storage size, admin passwords, etc.

Metadata (Stored as Kubernetes Secrets):

Helm stores metadata about releases, charts used, and revision states as Kubernetes secrets.

Ensures that release data is accessible to all team members working on the cluster.

Examples Used in the Lesson:

Hello World Application:

Simple NGINX-based web server with a service to expose it.

Consists of two objects: Deployment and Service.

WordPress Application:

More complex chart involving multiple objects (Deployment, PVC, PV, Secrets, etc.).

Key Takeaways:

Helm treats a collection of Kubernetes objects as a single application, making management easier.

It stores configuration data in the values.yaml file, making deployments more consistent and manageable.

Repositories like Artifact Hub centralize available charts for easier access and verification.

Metadata storage in Kubernetes ensures team-wide access and persistent storage of release data.


Helm Components - Overview

    Helm Command Line Utility:

        A command line tool installed on the local system.

        Used for performing Helm actions such as installing a chart, upgrading, rollback, etc.

    Commands:

        Install a chart: helm install <release-name> <chart-name>

        Upgrade a release: helm upgrade <release-name> <chart-name>

        Rollback to a previous revision: helm rollback <release-name> <revision-number>

        Uninstall a release: helm uninstall <release-name>

    Charts:

        A collection of files containing all the instructions Helm needs to deploy objects in a Kubernetes cluster.

        Acts as a package for Kubernetes applications.

        Publicly available charts can be downloaded from repositories like Bitnami, AppsCode, etc.

        Artifact Hub (artifacthub.io) serves as a centralized repository for all available charts.

    Example Chart Installation:
    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm repo update
    helm install my-wordpress bitnami/wordpress

    Release:

        A single installation of an application using a Helm chart.

        Each release can have multiple revisions, acting as snapshots of the application state.

        Allows for multiple releases from the same chart (e.g., my-site and my-second-site can be based on the same chart but tracked independently).

    Example Commands:
    helm ls
    helm get values <release-name>
    helm get manifest <release-name>

    Values.yaml File:

        The configuration file where customizable values for a chart are defined.

        Simplifies application customization by providing a single location to manage settings like storage size, admin passwords, etc.

        This file is heavily used in templating for customizing deployments.

    Example values.yaml:
    replicaCount: 3
    image:
      repository: nginx
      tag: latest
    service:
      type: NodePort
      port: 80

    Metadata (Stored as Kubernetes Secrets):

        Helm stores metadata about releases, charts used, and revision states as Kubernetes secrets.

        Ensures that release data is accessible to all team members working on the cluster.

    Command to View Metadata:
    kubectl get secrets -n <namespace>

Examples Used in the Lesson:

    Hello World Application:

        Simple NGINX-based web server with a service to expose it.

        Consists of two objects: Deployment and Service.

    Deployment Template (templates/deployment.yaml):
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: {{ .Values.name }}
    spec:
      replicas: {{ .Values.replicaCount }}
      selector:
        matchLabels:
          app: {{ .Values.name }}
      template:
        metadata:
          labels:
            app: {{ .Values.name }}
        spec:
          containers:
          - name: nginx
            image: {{ .Values.image.repository }}:{{ .Values.image.tag }}

    WordPress Application:

        More complex chart involving multiple objects (Deployment, PVC, PV, Secrets, etc.).

        Demonstrates the use of dependencies (e.g., MariaDB as a separate chart).

Chart.yaml File Structure:

    API Version: Specifies Helm API version (V1 for Helm 2, V2 for Helm 3).

    App Version: Specifies the version of the application being deployed (e.g., WordPress version).

    Chart Version: Version of the chart itself, independent of the app version.

    Name: Name of the chart (e.g., WordPress).

    Description: Description of the chart's purpose.

    Type: Application (default) or Library (for building charts).

    Dependencies: Other charts required for the current chart (e.g., MariaDB).

    Keywords: Tags to help with searching the chart in repositories.

    Maintainers: Information about the maintainers of the chart.

    Home and Icon: URLs for the project's homepage and icon.

Example chart.yaml:
apiVersion: v2
name: wordpress
version: 1.0.0
appVersion: 5.7.2
description: A WordPress application
keywords:
  - wordpress
  - blog
  - cms
maintainers:
  - name: John Doe
    email: john@example.com
dependencies:
  - name: mariadb
    version: 9.3.1
    repository: https://charts.bitnami.com/bitnami
Chart Directory Structure:

    templates/ - Contains template files for Kubernetes objects.

    values.yaml - Configuration file for defining custom values.

    chart.yaml - Contains metadata about the chart.

    LICENSE - Chart license information.

    README.md - Human-readable information about the chart.

    charts/ - Directory for dependency charts.


