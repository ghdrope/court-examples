# 01 - Installation Validation

This example validates that Court is correctly installed and running in your cluster.

---

## Goal

Ensure that all components of Court are up and healthy after installation.

---

## Prerequisites

- Kubernetes cluster running
- Court installed via Helm (see main repository README)

---

## Step 1 - Check Officer

Retrieve logs from the Officer component:

```bash
kubectl logs -n court deploy/officer
```

You should see logs similar to:

```bash
{"msg":"starting officer"}
{"msg":"controller manager initialized"}
{"msg":"starting controller manager loop"}
{"msg":"Starting workers"}
```

### ✅ Expected Signal

When you see:

```txt
"Starting workers"
```

It means the Officer controller is fully initialized and running.

---

## Step 2 - Check Court Worker

Retrieve logs from the Court component:

```bash
kubectl logs -n court deploy/court
```

You should see logs similar to:

```bash
{"msg":"configuration loaded"}
{"msg":"court worker started"}
```

### ✅ Expected Signal

When you see:

```txt
"court worker started"
```

It means the Court worker is connected and processing events.

---

## Final Validation

If both conditions are met:

- Officer reached "Starting workers"
- Court reached "court worker started"

Court is running and healthy.
