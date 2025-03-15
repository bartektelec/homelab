# 🏡 homelab

This repo hosts the desired state of my homelab kubernetes cluster. The cluster runs ArgoCD application that pulls the most recent changes and applies them to the cluster automatically.

## ❓Why Homelab

When I moved in to a new apartament I really wanted to expand my "home automation" setup, which was previously just a couple Phillips Hue bulbs.

I started by building the server out of two Raspberry Pis, these were fine for hosting a few apps like Home Assistant and an MQTT broker, but were somewhat unstable and crashing from time to time.

Not only that but adding more Raspberry Pis in 2025 gets quite expensive pretty quickly, a Raspberry Pi is no longer a cheap single board computer that would let people play with hardware and software for low cost, instead a 4GB Pi 5 model costs around 800 NOK (aprox. 75 USD), that is without Power supply, MicroSD card and case.

Later I figured there is a huge market for used small form-factor PCs / thin clients.
These may not sound very performant but are usually a good value for a buck.

First one I bought was a HP ProDesk 600 G1 - sure, it is over 10 years old, but for 600 NOK (aprox. 55 USD) I've got a 8GB Ram, 250GB SSD, 4 Core PC that doesn't need much power to run and is built on top of x86 which is way more compatible with most software than ARM-based solutions are.

## 💻 Hardware

| Model                   | CPU                      | Storage      | Memory |
| ----------------------- | ------------------------ | ------------ | ------ |
| Raspberry Pi 3b         | ARM64 (Quad @ 1.2Ghz)    | 32GB MicroSD | 1GB    |
| Raspberry Pi 5          | ARM64 (Quad @ 2.4Ghz)    | 32GB MicroSD | 4GB    |
| HP ProDesk 600 G1       | i5-4590T (Quad @ 2Ghz)   | 250GB SSD    | 8GB    |
| Lenovo ThinkCentre M92p | i5-3470T (Quad @ 2.9Ghz) | 120GB SSD    | 8GB    |

## 🔐 Security

I am attempting to follow GitOps good practices, learning a lot on the way.
Kubescape is used as a linter to scan the manifest files in order to find security vulnerabilities.
All secrets are encrypted used the "SealedSecrets" method. Encryption happens by utilizing the `publickey.peb` certificate and running `kubeseal --cert [path_to_cert] -o yaml -n [namespace] < [path_to_secret_file].yaml > [path_to_output_file].yaml`.
Additionally, to keep myself from exposing raw secrets to the public, any file called `secret.yaml` is ignored by git.

## 💾 Installed applications and tools

### Apps

End user applications, daily drivers

| Name              | Description                                           |
| ----------------- | ----------------------------------------------------- |
| 👀 Glance         | A customized dashboard/startpage                      |
| 🏠 Home Assistant | Home automation tool                                  |
| 🕸️ Adguard        | Lightweight DNS server filtering out unwanted domains |
| 🤑 Budge          | A personal finance tracker                            |

### Monitoring

Apps that help me monitor if everything on the cluster works as expected

| Name                 | Description                           |
| -------------------- | ------------------------------------- |
| 🚦 Traefik Dashboard | A dashboard for Traefik reverse proxy |
| 💡 Headlamp          | A kuberenetes dashboard               |
| 🧑‍⚕️ Uptime Kuma       | Uptime Tracker                        |

### Infrastructure

Tools that help managing my cluster and deploy apps in a secure way

| Name             | Description                                         |
| ---------------- | --------------------------------------------------- |
| 🤐 SealedSecrets | An operator that decrypts my encrypted secret files |
| 🚇 Tailscale     | Operator for using Tailscale as an Ingress type     |
| 🦆 DuckDNS       | A tiny DynDNS watcher                               |

