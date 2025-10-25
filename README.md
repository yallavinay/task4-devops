# Infrastructure as Code (IaC) with Terraform 🚀

## Project Overview
This project demonstrates Infrastructure as Code (IaC) principles using Terraform to provision and manage a local Docker container. Learn how to automate infrastructure deployment, management, and destruction using declarative configuration files.

## 🎯 Objectives
- **Provision** a local Docker container using Terraform
- **Understand** Infrastructure as Code concepts
- **Master** Terraform workflow and commands
- **Learn** Docker provider integration
- **Practice** infrastructure lifecycle management

## 🛠️ Tools & Technologies
- **Terraform** - Infrastructure as Code tool
- **Docker** - Containerization platform
- **Docker Provider** - Terraform provider for Docker

## 📁 Project Structure
```
task4-devops/
├── main.tf                    # Main Terraform configuration
├── terraform.tfstate          # Terraform state file
├── terraform.tfstate.backup   # Backup state file
└── README.md                  # This documentation
```

## 🔧 Prerequisites
Before running this project, ensure you have:

- **Docker Desktop** installed and running
- **Terraform** installed (version 1.0+)
- **Git** for version control
- **Command Line Interface** (PowerShell/CMD)

### Installation Links
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Terraform Download](https://www.terraform.io/downloads.html)

## 📋 Configuration Details

### main.tf Breakdown
```hcl
# Provider Configuration
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.2"
    }
  }
}

# Docker Provider
provider "docker" {}

# Docker Image Resource
resource "docker_image" "nginx_image" {
  name         = "nginx:latest"
  keep_locally = false
}

# Docker Container Resource
resource "docker_container" "nginx_container" {
  name  = "terraform-nginx"
  image = docker_image.nginx_image.image_id
  
  ports {
    internal = 80
    external = 8081  # Changed from 8080 to avoid port conflicts
  }
}
```

## 🚀 Getting Started

### Step 1: Initialize Terraform
```bash
terraform init
```
**What it does:** Downloads required providers and initializes the working directory.

### Step 2: Plan Your Infrastructure
```bash
terraform plan
```
**What it does:** Shows what Terraform will do before making changes (dry run).

### Step 3: Apply Configuration
```bash
terraform apply
```
**What it does:** Creates the Docker container based on your configuration.

### Step 4: Verify Deployment
```bash
docker ps
```
**What it does:** Lists running containers to verify your nginx container is running.

### Step 5: Test Your Application
Open your browser and navigate to:
```
http://localhost:8081
```
You should see the default nginx welcome page.

## 🔍 Key Terraform Commands

### Essential Commands
| Command | Purpose | When to Use |
|---------|---------|-------------|
| `terraform init` | Initialize working directory | First time setup |
| `terraform plan` | Preview changes | Before applying changes |
| `terraform apply` | Apply changes | Deploy infrastructure |
| `terraform destroy` | Remove infrastructure | Clean up resources |
| `terraform state list` | List resources in state | Check current state |
| `terraform show` | Show current configuration | Debug and verify |

### State Management
```bash
# List all resources in state
terraform state list

# Show detailed state information
terraform state show docker_container.nginx_container

# Remove resource from state (without destroying)
terraform state rm docker_container.nginx_container
```

## 🧪 Testing & Validation

### Manual Testing Checklist
- [ ] Container starts successfully
- [ ] Nginx responds on port 8081
- [ ] No port conflicts
- [ ] Container logs are accessible
- [ ] Resource cleanup works

### Verification Commands
```bash
# Check container status
docker ps -a

# View container logs
docker logs terraform-nginx

# Test HTTP response
curl http://localhost:8081

# Check port binding
netstat -ano | findstr :8081
```

## 🚨 Troubleshooting

### Common Issues & Solutions

#### Port Already in Use
**Error:** `bind: Only one usage of each socket address is normally permitted`

**Solution:** 
1. Check what's using the port: `netstat -ano | findstr :8081`
2. Change the external port in main.tf
3. Or stop the conflicting service

#### Docker Not Running
**Error:** `Cannot connect to the Docker daemon`

**Solution:**
1. Start Docker Desktop
2. Wait for Docker to fully initialize
3. Run `docker ps` to verify Docker is running

#### Terraform State Issues
**Error:** `Resource already exists`

**Solution:**
1. Import existing resource: `terraform import docker_container.nginx_container <container_id>`
2. Or destroy and recreate: `terraform destroy` then `terraform apply`

## 🧹 Cleanup & Best Practices

### Destroy Infrastructure
```bash
terraform destroy
```
**⚠️ Warning:** This will permanently delete your Docker container.

### Best Practices
1. **Always run `terraform plan` before `apply`**
2. **Keep state files secure** (use remote state in production)
3. **Use version control** for your Terraform files
4. **Document your infrastructure** changes
5. **Test in development** before production

## 📊 Expected Outcomes

After completing this project, you will understand:

✅ **Infrastructure as Code Concepts**
- Declarative vs Imperative configuration
- State management and tracking
- Resource dependencies and relationships

✅ **Terraform Workflow**
- Initialize → Plan → Apply → Destroy cycle
- Provider management and configuration
- State file importance and management

✅ **Docker Integration**
- Container lifecycle management
- Port mapping and networking
- Image management and caching

✅ **DevOps Practices**
- Infrastructure automation
- Version control for infrastructure
- Testing and validation procedures

## 🔗 Additional Resources

- [Terraform Documentation](https://www.terraform.io/docs/)
- [Docker Provider Documentation](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs)
- [Infrastructure as Code Best Practices](https://www.terraform.io/docs/cloud/guides/recommended-practices/index.html)

## 📝 Notes

- **Port 8081** is used instead of 8080 to avoid conflicts with other services
- **State files** are stored locally (use remote state for team projects)
- **Container name** is set to `terraform-nginx` for easy identification
- **Nginx image** is pulled from Docker Hub automatically

---

**Happy Infrastructure Coding! 🎉**

*This project demonstrates the power of Infrastructure as Code and how Terraform can simplify container management and deployment processes.*
