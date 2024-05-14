## Introduction
This guide provides a comprehensive template for setting up a basic self-hosted Kubernetes (k8s) cluster. It covers the installation of essential Kubernetes tools such as kubeadm, kubectl, and kubelet, as well as the initialization of a Kubernetes cluster and the installation of common plugins and applications. This document is designed for those who are deploying Kubernetes on Linux-based VPS or bare metal servers.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Key Components](#key-components)
- [Installation Steps](#installation-steps)
- [Preparing the Servers (Debian)](docs/debian/preparing-servers.md) [CentOS](docs/centos/preparing-servers.md), [Fedora](docs/fedora/preparing-servers.md)
- [Installation of Kubeadm](docs/installing-kubeadm.md)
- [Cluster Initialization](docs/initializing-cluster.md)
- [Installing Additional Components](docs/installing-additional-components.md)

## Prerequisites
Before initiating the installation process, ensure you have the following setup:
- A minimum of one VPS or bare metal server to serve as the control plane.
- Additional servers to function as worker nodes.
- Each server should have a minimum specification of 2GB RAM and 2 CPUs.

## Key Components
This setup involves a variety of crucial Kubernetes components, tools, and additional applications that aid in creating a production-ready environment:
- A choice among several Container Network Interface (CNI) plugins such as Cilium, Calico, Weave, or Flannel.
- The Nginx Ingress Controller for managing access to services in a Kubernetes cluster.
- A container runtime, options include Docker or Containerd.
- Cert Manager for handling certificates within the cluster.
- Helm for managing Kubernetes packages.
- An example application, such as the Echo Server, to test the deployment.

## Installation Steps
The following sequence will guide you through preparing your servers, installing required software, initializing your Kubernetes cluster, and adding additional functional components:
1. [Preparing the Servers](docs/preparing-servers.md): Guide on setting up and configuring your machines for Kubernetes installation.
2. [Installing kubeadm, kubectl and kubelet](docs/installing-kubeadm.md): Steps to install Kubernetes management tools.
3. [Initializing the Kubernetes Cluster](docs/initializing-cluster.md): Procedure to start and configure the cluster.
4. [Installing Additional Components](docs/installing-additional-components.md): Instructions on how to add plugins and applications to enhance the cluster's capabilities.
