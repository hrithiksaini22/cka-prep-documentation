In Kubernetes, **Priority Classes** are a way to assign importance to Pods — they help the **scheduler** and **kubelet** make decisions during resource shortages (like evictions). Understanding them is important for **CKA prep** and managing workloads in production.

---

## 🚨 What Are Priority Classes?

A **PriorityClass** is a **non-namespaced object** that defines the **priority value** of a Pod.

### Why it's useful:

* Pods with **higher priority** are scheduled first.
* Lower-priority pods are **evicted first** during node pressure (e.g., out of memory).
* You can control eviction behavior in multi-tenant or critical workloads.

---

## 🛠️ Create a PriorityClass

### Example YAML:

```yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 1000000
globalDefault: false
description: "This priority class is for high-priority workloads."
```

* `value`: Higher value = higher priority.
* `globalDefault`: If set to `true`, pods without an explicit `priorityClassName` will use this.
* `description`: Optional, for documentation.

---

## 📦 Using a PriorityClass in a Pod or Deployment

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: high-priority-pod
spec:
  priorityClassName: high-priority
  containers:
  - name: nginx
    image: nginx
```

---

## 🧠 Important Notes for 2025 CKA

| Feature           | Detail                                                              |
| ----------------- | ------------------------------------------------------------------- |
| ✅ Available since | Kubernetes 1.11                                                     |
| 🎯 Applies to     | All Pods                                                            |
| 🚫 Cannot         | Lower priority of system-reserved components                        |
| 📊 Range          | Values can be positive or negative; higher number = higher priority |
| 🧹 Eviction       | Lower priority pods are evicted first under pressure (e.g., memory) |

---

## 📌 Default Priority Classes in Kubernetes

Some clusters (like GKE, AKS) include default priority classes:

* `system-cluster-critical` (value: 2000000000)
* `system-node-critical` (value: 2000001000)

These are reserved for **Kubernetes system components**.

---

## 🔍 Check Priority Classes

```bash
kubectl get priorityclasses
kubectl describe priorityclass high-priority
```

---

## ✅ Use Cases in CKA

* Ensuring your critical pods aren't evicted before less important ones.
* Creating a global default priority.
* Handling graceful eviction of non-critical workloads.

---

Would you like a hands-on YAML exercise with multiple pods and priority classes to test eviction behavior?
