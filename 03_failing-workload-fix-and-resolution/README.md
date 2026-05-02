# 03 - Failing Workload, Issue Creation and Automated Resolution

This example demonstrates the full Court workflow:

- Detect a failure
- Generate an incident
- Create a GitHub issue
- Apply a fix
- Automatically resolve the issue

---

## Goal

Validate that Court:

- detects a failing workload
- creates an incident and a `Suit`
- opens an issue in the configured repository
- automatically closes the issue once the problem is resolved

---

## Prerequisites

- Court installed and validated (see example 01)
- A fork of this repository

---

## Step 0 - Update reposiroy annotation

Before applying the manifest, update:

```txt
court.dev/repository: https://github.com/ghdrope/court-examples
```

Replace with your fork:

```txt
court.dev/repository: https://github.com/<your-username>/court-examples
```

---

## Step 1 - Apply the failing workload

```bash
kubectl apply -f api-worker-pod.yaml
```

---

## Step 2 - Watch pod lifecycle

```bash
kubectl get pods --watch
```

You should observe:

- Pending -> Running -> Error

### ❌ Expected failure

The pod will eventually reach:

```txt
STATUS: Error
```

This is expected.

---

## Step 3 - Check generated issue

Go to your forked repository and open:

- **Issues tab**

### ✅ Expected result

A new issue should be created automatically describing the failure, including:

- logs
- Kubernetes events
- runtime context
- resolution instructions

---

## Step 4 - Apply a fix using an AI agent

### Option A - GitHub Copilot (if available)

Assign the issue to Copilot and request a fix.

### Option B - Use an AI agent in your IDE

1. Create a branch from the issue:

    ```bash
    git checkout -b fix/api-worker-failure
    ```

2. Open the issue and copy its content

3. Paste it into your AI agent (Copilot Chat, Codex, etc.)

### 🧠 Expected fix

The agent should identify that the containers exit with failure:

```txt
exit 1
```

And replace with:

```txt
exit 0
```

---

## Step 5 - Apply the fix

After updating the manifest:

```bash
kubectl apply -f api-worker-pod.yaml --force
```

---

## Step 6 - Verify recovery

Watch the pod again:

```bash
kubectl get pods --watch
```

### ✅ Expected result

- Pod transitions to `Running` or completes successfully

---

## Step 7 - Verify issue resolution

Go back to your repository:

- Refresh the Issues tab

### ✅ Expected result

- The issue is automatically closed

---

## Step 8 - Cleanup

Delete the pod:

```bash
kubectl delete -f api-worker-pod.yaml
```

Or:

```bash
kubectl delete pod api-worker
```

---

## Watch just happened

- Officer detected the failure and created an `IncidentReport`
- Court created a `Suit` and opened a GitHub issue
- You applied a fix
- The workload recovered
- Court reconciled the state and close the issue
