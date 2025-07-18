# Cloud Computing Semester Project [(you are here)](https://github.com/mirza-src/cc-project)

This repository contains a cloud-native implementation for a Cloud Computing semester project, demonstrating scalable infrastructure deployment and application management using modern DevOps practices and tools.

## 🏗️ Project Overview

The project consists of three main components that work together to create a cloud-native infrastructure and application deployment pipeline:

1. **Infrastructure as Code** (`infrastructure-live/`) - [Terraform](https://terraform.io) configurations for Azure infrastructure
2. **Application Source Code** (`cc-app/`) - A [Next.js](https://nextjs.org) file compression and hashing service
3. **GitOps Configuration** (`argocd-gitops/`) - [ArgoCD](https://argoproj.github.io/cd/) manifests for declarative deployments

## 📁 Project Structure

### 🏗️ Infrastructure Live (`infrastructure-live/`)

**Source Repository**: https://github.com/mirza-src/cc-infrastructure-live

This folder contains [Terraform](https://terraform.io) configurations for provisioning Azure infrastructure using Infrastructure as Code.

**Key Components:**

- **Kubernetes Cluster**: [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/products/kubernetes-service)
- **DNS Zone**: [Azure DNS](https://azure.microsoft.com/en-us/products/dns) for domain management
- **Networking**: Virtual networks and subnets
- **Identity**: [Workload Identity](https://azure.github.io/azure-workload-identity/) for passwordless service authentication for managing DNS records

**Prerequisites:**

- Azure Storage Account for Terraform state
- [Azure Key Vault](https://azure.microsoft.com/en-us/products/key-vault) for secrets management
- Azure Service Principal with Owner permissions
- [Novops](https://github.com/PierreBeucher/novops) for environment variable management

**Architecture:**

- Two separate Terraform projects:
  1. `cc-project/`: Core infrastructure (AKS + DNS)
  2. `cc-project/kubernetes-resources/`: ArgoCD deployment with `bootstrap` configuration

### 🖥️ Application (`cc-app/`)

**Source Repository**: https://github.com/MairaTariq16/cc-app

A [Next.js](https://nextjs.org) application that provides file compression and hashing services with the following features:

**Features:**

- File compression using [archiver](https://www.npmjs.com/package/archiver)
- File hashing using [crypto-js](https://www.npmjs.com/package/crypto-js)
- Multi-architecture container images (AMD64/ARM64)

**Performance Testing:**

- [k6](https://k6.io/) load testing scripts

**Deployment:**

- [Docker](https://www.docker.com/) containerization with multi-arch builds
- [GitHub Actions](https://github.com/features/actions) CI/CD pipeline
- Container images pushed to [GitHub Container Registry (GHCR)](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)
- [Helm](https://helm.sh/) chart for Kubernetes deployment

### 🔄 GitOps Configuration (`argocd-gitops/`)

**Source Repository**: https://github.com/mirza-src/cc-argocd-gitops

GitOps repository containing all Kubernetes manifests managed by [ArgoCD](https://argoproj.github.io/cd/) for declarative deployments.

**Infrastructure Applications:**

- **[ArgoCD](https://argoproj.github.io/cd/)**: GitOps continuous delivery tool
- **[cert-manager](https://cert-manager.io/)**: Automatic TLS certificate management
- **[External DNS](https://kubernetes-sigs.github.io/external-dns/)**: Automatic DNS record management
- **[Ingress NGINX](https://kubernetes.github.io/ingress-nginx/)**: Kubernetes ingress controller
- **[Kubeview](https://github.com/benc-uk/kubeview)**: Kubernetes cluster visualization

**Application Deployments:**

- **cc-app**: Main file compression/hashing service
- **php-apache**: [Kubernetes example application](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/) (attempted)
- **[Stirling PDF](https://github.com/Stirling-Tools/Stirling-PDF)**: PDF manipulation tool (attempted)

## 🛠️ Tools and Technologies

### Infrastructure & Platform

- **[Terraform](https://terraform.io)**: Infrastructure as Code
- **[Azure](https://azure.microsoft.com/)**: Cloud platform
- **[Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/products/kubernetes-service)**: Managed Kubernetes
- **[Azure DNS](https://azure.microsoft.com/en-us/products/dns)**: DNS hosting
- **[Azure Key Vault](https://azure.microsoft.com/en-us/products/key-vault)**: Secrets management

### GitOps & CI/CD

- **[ArgoCD](https://argoproj.github.io/cd/)**: GitOps continuous delivery
- **[GitHub Actions](https://github.com/features/actions)**: CI/CD pipelines
- **[Helm](https://helm.sh/)**: Kubernetes package manager

### Application & Development

- **[Next.js](https://nextjs.org)**: React framework
- **[Docker](https://www.docker.com/)**: Containerization
- **[k6](https://k6.io/)**: Load testing

### Security & Authentication

- **[cert-manager](https://cert-manager.io/)**: TLS certificate automation
- **[Workload Identity](https://azure.github.io/azure-workload-identity/)**: Passwordless authentication
- **[External DNS](https://kubernetes-sigs.github.io/external-dns/)**: Automated DNS management

### Development Experience

- **[Nix](https://nixos.org/)**: Reproducible development environments
- **[direnv](https://direnv.net/)**: Environment variable management
- **[devenv](https://devenv.sh/)**: Development environment configuration
- **[Novops](https://github.com/PierreBeucher/novops)**: Secrets and environment management

## 🚀 Deployment Architecture

### Infrastructure Flow

1. **Manual Setup**: Azure Storage Account, Key Vault, and Service Principal creation
2. **DNS Configuration**: Domain `mirzaesaaf.me` purchased from [Namecheap](https://www.namecheap.com/) (free student account)
3. **Terraform Deployment**:
   - Core infrastructure (AKS cluster + DNS zone)
   - ArgoCD installation with bootstrap application
4. **Manual DNS Setup**: Configure Namecheap nameservers to point to Azure DNS zone
5. **GitOps Sync**: ArgoCD automatically deploys all applications and infrastructure components

### Application Scaling Challenges

The project initially planned to use [Stirling PDF](https://github.com/Stirling-Tools/Stirling-PDF) followed by [php-apache example](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/) for demonstration but encountered several challenges:

1. **Architecture Compatibility**: Limited ARM64 support in applications
2. **Resource Requirements**: Insufficient quota/funds for required compute resources

This led to the development of a custom **cc-app** in Next.js with:

- Multi-architecture container images (workarounds for slow cross-platform builds)
- Lightweight resource requirements
- Custom performance testing scripts

## 🌐 Live Applications

- **CC-App**: [https://cc-app.mirzaesaaf.me](https://cc-app.mirzaesaaf.me) - File compression and hashing service
- **Kubeview**: [https://kubeview.mirzaesaaf.me](https://kubeview.mirzaesaaf.me) - Kubernetes cluster visualization (read-only)
- **ArgoCD**: [https://argocd.mirzaesaaf.me](https://argocd.mirzaesaaf.me) - GitOps dashboard

## ⚙️ Development Setup

### Prerequisites

Each project directory contains its own `flake.nix` with development environment configuration for seamless development experience.

1. **Install Nix** (recommended using [Determinate Systems installer](https://install.determinate.systems/)):

   ```bash
   curl -fsSL https://install.determinate.systems/nix | sh -s -- install
   ```

2. **Install direnv**:

   ```bash
   # On most systems
   curl -sfL https://direnv.net/install.sh | bash

   # Add to your shell configuration (~/.zshrc, ~/.bashrc, etc.)
   eval "$(direnv hook zsh)"  # or bash, fish, etc.
   ```

### Usage

1. **Clone the repository**:

   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Enter development environment**:

   ```bash
   # direnv will automatically load the development environment (requied only once)
   direnv allow

   # Or manually enter the nix development shell
   nix develop
   ```

## 🎯 Project Goals Achievement

This project demonstrates several cloud computing concepts from the course:

- **Rapid Elasticity**: Kubernetes HPA for automatic scaling based on CPU/memory metrics
- **On-Demand Self-Service**: Automated provisioning via Terraform and GitOps
- **Broad Network Access**: Web-accessible applications with proper ingress configuration
- **GitOps**: Declarative infrastructure and application management
- **Identity Federation**: Workload Identity for secure, passwordless authentication
- **Automated Certificate Management**: Let's Encrypt integration via cert-manager
- **DNS Automation**: External DNS for dynamic record management

### Infrastructure as Code

- **State Management**: Remote state storage in Azure Storage
- **Secret Management**: Azure Key Vault integration with Novops

## 📊 Performance Testing

The cc-app includes basic performance testing using [k6](https://k6.io/):

```bash
cd cc-app
BASE_URL=https://cc-app.mirzaesaaf.me npm run perf:report     # Generate web dashboard report
BASE_URL=https://cc-app.mirzaesaaf.me npm run perf:all        # Run all performance tests
BASE_URL=https://cc-app.mirzaesaaf.me npm run perf:hash       # Test hashing endpoint
BASE_URL=https://cc-app.mirzaesaaf.me npm run perf:compress   # Test compression endpoint
```

Reports demonstrate the application's behavior under load and validate HPA scaling policies.

## 📝 Notes

- Domain `mirzaesaaf.me` requires manual nameserver configuration in Namecheap
- Student Azure account has quota limitations affecting VM availability
- Multi-architecture builds include specific workarounds for cross-platform compatibility
