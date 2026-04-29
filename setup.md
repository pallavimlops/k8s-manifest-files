# Setup Guide — Kubernetes Local Environment (Windows)

## Step 1 — Install Docker Desktop

1. Go to https://www.docker.com/products/docker-desktop/
2. Click **Download for Windows**
3. Run the installer (`Docker Desktop Installer.exe`)
4. During install — make sure **Use WSL 2** is checked
5. Restart your computer when asked
6. After restart — open Docker Desktop from Start menu
7. Wait until the whale icon in the taskbar stops animating


---

## Step 2 — Install Minikube

1. Go to https://minikube.sigs.k8s.io/docs/start/
2. Download the **Windows installer**
3. Run the installer
4. Open a new terminal and verify:
```bash
minikube version
```

---

## Step 4 — Start Minikube

```bash
# Set docker as default driver (do this once)
minikube config set driver docker

# Start the cluster
minikube start
```

You should see:
```
Done! kubectl is now configured to use "minikube"
```

---

## Step 5 — Verify Everything Works

```bash
minikube status    # should show Running
kubectl get nodes  # should show 1 node Ready
```

---

## Daily Usage

```bash
# Start cluster (do this every time after PC restart)
minikube start

# Stop cluster when done
minikube stop


