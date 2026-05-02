# 02 - Healthy Workload (No Incident Expected)

This example demonstrates that Court does **not** create incidents when a workload completes successfully.

---

## Goal

Validate that:

- A healthy workload does not generate an incident
- No `Suit` is created in the system
- No issue is created in the configured repository

---

## Prerequisites

- Court installed and validated (see example 01)
- A fork of this repository

---

## Step 0 - Update repository annotation

Before applying the manifest, update the annotation:

```yaml
court.dev/repository: https://github.com/ghdrope/court-examples
```

Replace with your own fork:

```yaml
court.dev/repository: https://github.com/ghdrope/court-examples
```

---

## Step 1 - Apply the workload

```bash
kubectl apply -f healthy-pod.yaml
```

---

## Step 2 - Watch pod lifecycle

```bash
kubectl get pods --watch
```

You should observe the pod transitioning through states such as:

- Pending
- Running
- Completed

### ✅ Expected behavior

The pod should complete successfully:

```txt
STATUS: Completed
```

---

## Step 3 - Verify no suit was created

Check database (Archive / PostgreSQL). Access the PostgreSQL container:

```bash
kubectl exec -n court -it statefulset/archive -- psql -U postgres -d archive
```

Run:

```sql
SELECT * FROM suits;
```

### ✅ Expected result

- No rows returned
- No Suit created

---

## Step 4 - Verify no issues was created

Go to your fork of the repository and check:

- Issues tab

### ✅ Expected result

- No issues created

---

## Step 5 - Cleanup

Remove the pod created in this example:

```bash
kubectl delete -f healthy-pod.yaml
```

or:

```bash
kubectl delete pod hello-world
```

---

## What just happened

- The workload executed successfully
- No failure signals were detected
- Officer did not create an `IncidentReport`
- Court did not create a `Suit`
- No issues was generated
