# Kafka for Streams Active-Active Setup with MirrorMaker 2

This guide details the process for setting up an active-active, bidirectionally replicated Kafka environment using the Strimzi Kafka Operator and MirrorMaker 2.

We will refer to the two Kafka clusters as `cluster-a` and `cluster-b`.

## Prerequisites

* Two separate OpenShift clusters
* `kubectl` or `oc` configured with separate contexts for each cluster.

---

## 🚀 Step 1: Install Kafka on Both Clusters

You must perform these steps on **both** OpenShift clusters.

### 1. Install the Strimzi Operator

Deploy the Strimzi Kafka Operator, which manages the Kafka cluster lifecycle.

```bash
# Set your context to the OpenShift cluster-a
kubectl config use-context cluster-a

