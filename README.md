# 🚀 Bootstrapping a [K8s](https://kubernetes.io/) Cluster on [Raspberry Pi](https://www.raspberrypi.com/) with [`ansible`](https://ansible.com/)

🧠 This project automates the provisioning of a lightweight [`k3s`](https://k3s.io/) cluster
on [Raspberry Pi](https://www.raspberrypi.com/) devices using [`ansible`](https://ansible.com/). It sets up a
control‑plane node, one or more worker nodes, and optionally a kiosk node for displaying dashboards. The playbook
handles everything from system configuration to cluster bootstrapping:

- 🛠️ Base system configuration (locales, timezone, hostname, kernel flags)
- 📦 Package updates and cleanup
- 🐧 Installation of [`k3s`](https://k3s.io/) on the control-plane node with minimal components
- 🔑 Secure token retrieval and distribution for worker nodes
- 🤝 Automatic joining of workers to the cluster
- 🖥️ Optional kiosk node with X11 + Chromium in fullscreen mode

## 📋 Prerequisites

Before running the playbook, make sure you have:

- 💻 A control machine with [`ansible`](https://ansible.com/) installed
- 🔐 Your SSH private key available locally (e.g. `~/.ssh/id_ed25519`)
- 🔑 SSH access to all nodes (public key added to `~/.ssh/authorized_keys`)
- 🗂️ A valid inventory file (`inventory/hosts.ini`) with IP addresses and group names
- 🍓 [Raspberry Pi OS](https://www.raspberrypi.com/software/operating-systems/) already installed and reachable via
  network
- 🐍 Python 3 installed on each [Raspberry Pi](https://www.raspberrypi.com) (usually pre-installed
  with [Raspberry Pi OS](https://www.raspberrypi.com/software/operating-systems/))
- 🌐 Internet access on the Pis to fetch [`k3s`](https://k3s.io/) and updates

## ⚙️ Usage

Copy the example inventory file `inventory/hosts.example.ini` to `inventory/hosts.ini` and customize it according to
your setup..

```bash
# 🖥️ Control Plane Setup
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory/hosts.ini -l k62s-control-plane playbooks/k62s.yaml

# 🧑‍💻 Worker Node Setup
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory/hosts.ini -l k62s-worker-1 playbooks/k62s.yaml
```

## 🖥️ Optional: Kiosk Node Setup

This project also supports provisioning a kiosk node — a Raspberry Pi that automatically boots into a minimal X11
session and displays a fullscreen Chromium browser. This is ideal for dashboards such as [Grafana](http://grafana.com/),
status boards, or home‑lab monitoring screens.

The kiosk role:

- 🚫 Disables the default `getty` login on `tty1`
- 🪟 Installs a lightweight X11 environment with Openbox
- 🌐 Launches Chromium in kiosk mode
- 🖱️ Hides the mouse cursor and disables screen blanking
- 🔁 Runs as a systemd service for automatic startup and recovery

To provision the kiosk node, target the `kiosk` group in your inventory:

```bash
# 🖥️ Kiosk Node Setup
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory/hosts.ini -l kiosk playbooks/k62s.yaml
```

## 🔭 What’s Next?

After successfully setting up your [Kubernetes](https://kubernetes.io/) cluster, it’s time to start tinkering! You
could:

- 🧪 Deploy sample apps and test workloads
- 📦 Explore [Helm](https://helm.sh/) charts or GitOps workflows
- 🚀 [FluxCD](https://fluxcd.io/) or [ArgoCD](https://argoproj.github.io/cd/) for continuous deployment
- 🏡 Start a Home Lab project like [k62s-gitops](https://github.com/mzellho/k62s-gitops)

Your cluster is lightweight, modular, and ready for experimentation — so go ahead and make it yours!
