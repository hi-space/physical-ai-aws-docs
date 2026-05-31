---
description: Reference architecture for running real-time inference using Cosmos NIM microservices in Amazon EKS
---

# Cosmos NIM + EKS

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

Operating Cosmos NIM microservices on Amazon EKS provides **enterprise-grade orchestration**, **auto-scaling**, and **simplified operations**.

If you need **high availability**, **elastic scaling**, and **cloud-native integration** in production, we recommend this approach.

Cosmos NIM packages the Cosmos model together with an **optimized inference engine**. This reduces manual configuration complexity.

**Advantages**

* **Enterprise-Grade Orchestration**: Leverages Kubernetes' declarative configuration, automatic rollout/rollback, Pod restart-based self-healing, and service discovery.
* **High Availability**: Multi-pod deployments eliminate single points of failure. Node distribution across availability zones withstands AZ failures. Rolling updates maintain availability during deployments.
* **Simplified Operations**: EKS's managed control plane reduces operational burden. Upgrades and patches become easier. Integration with AWS services provides consistent monitoring and security.
