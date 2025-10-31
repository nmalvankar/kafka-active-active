# Streams for Apache Kafka Active-Active Setup with MirrorMaker 2

This is a guide for setting up an active-active, bidirectionally replicated Kafka environment using the Streams for Apache Kafka Operator and MirrorMaker 2.

We will refer to the two Kafka clusters as `cluster-a` and `cluster-b`.

---

You must perform these steps on **both** OpenShift clusters. In this example, we will be deploying 2 instances of Kafka in 2 different namespaces on the same OpenShift cluster.

### 1. Install the Streams for Apache Kafka Operator

Install the Streams for Apache Kafka Operator on both namespaces. This operator manages the Kafka cluster lifecycle.

Lets start by creating 2 namespaces for our Kafka clusters
```
oc new-project cluster-a

oc new-project cluster-b
```

### 2. Deploy the 2 Kafka cluster instances

```
oc apply -f cluter-a/kafka.yaml -n cluster-a
oc apply -f cluter-b/kafka.yaml -n cluster-b
```

### 3. Establish cross-cluster trust

```
oc get secret cluster-a-cluster-ca-cert -n cluster-a -o jsonpath='{.data.ca\.crt}' | base64 -d > cluster-a.crt
oc get secret cluster-b-cluster-ca-cert -n cluster-b -o jsonpath='{.data.ca\.crt}' | base64 -d > cluster-b.crt
```

```
oc create secret generic cluster-a-cluster-ca-cert -n cluster-b --from-file=ca.crt=./cluster-a.crt
oc create secret generic cluster-b-cluster-ca-cert -n cluster-a --from-file=ca.crt=./cluster-b.crt
```

### 4. Deploy MirrorMaker2 on both Kafka clusters

```
oc apply -f cluster-a/mirrormaker2.yaml -n cluster-a
oc apply -f cluster-b/mirrormaker2.yaml -n cluster-b
```

### 5. Install the Streams for Apache Kafka Console operator

Install the Streams for Apache Kafka Console operator for verification

### 6. Deploy Kafka Console instance

Deploy the Kafka Console instance in one of the namespaces
```
oc apply -f kafka-console/console.yaml -n cluster-a
```
