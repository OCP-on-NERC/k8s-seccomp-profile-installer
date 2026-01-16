# k8s-seccomp-profile-installer

Use a privileged DaemonSet to install custom seccomp profiles on all current and future nodes in an OpenShift cluster.

## How it works

A DaemonSet runs on every node and copies seccomp profile JSON files from a ConfigMap to `/var/lib/kubelet/seccomp` on each node's host filesystem. This makes the profiles available for use by pods via `securityContext.seccompProfile.localhostProfile`.

## Installation

### 1. Create the namespace

```bash
oc create namespace seccomp-profile-installer
```

### 2. Apply the solution

```bash
oc apply -k k8s-seccomp-profile-installer/overlays/nerc-ocp-prod/
```

### 3. Verify the installation

Check that pods are running on all nodes:

```bash
oc get pods -n seccomp-profile-installer -o wide
```

Verify the profile exists on a node:

```bash
oc debug node/<node-name> -- chroot /host ls -la /var/lib/kubelet/seccomp/
```
