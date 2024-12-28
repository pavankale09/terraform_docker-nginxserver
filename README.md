"# terraform_docker-nginxserver" 

Step-by-Step Guide to Execute the Simple Project: Provision an Nginx Web Server on Docker
1. Prerequisites
Install Docker: Ensure Docker is installed and running on your machine.
Download Docker.
Install Terraform: Verify Terraform is installed.
Run terraform -version to check. Download from Terraform Download Page if not installed.

2. Set Up the Project
Create a Directory for the Project:

mkdir terraform-docker-nginx
cd terraform-docker-nginx
Create Terraform Configuration Files:

Create a file named main.tf:
Add the Configuration to main.tf: Copy the following Terraform configuration into the main.tf file:
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {
  host = "npipe:////.//pipe//docker_engine" # Adjust for your OS if needed
}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "nginx_server"

  ports {
    internal = 80
    external = 8080
  }
}

output "nginx_url" {
  value = "http://localhost:8080"
}


3. Initialize the Terraform Project
Run the following command to download the necessary provider (Docker):
terraform init
Expected output:


Terraform initializes and downloads the Docker provider plugin.

4. Validate the Configuration
Run the following command to ensure there are no syntax errors:

terraform validate
Expected output:
Validation success message.

5. Plan the Deployment
Preview the changes Terraform will make:
terraform plan
Expected output:
Terraform shows it will pull the Nginx image and create a container.

6. Apply the Configuration
Deploy the infrastructure:
terraform apply
When prompted, type yes to confirm the deployment.
Expected output:
Terraform outputs the URL where the Nginx server is accessible:
nginx_url = "http://localhost:8080"

7. Test the Deployment
Open your browser and navigate to http://localhost:8080.
You should see the default Nginx welcome page.

8. Clean Up
To remove the Docker container and image created by Terraform:

terraform destroy
When prompted, type yes to confirm the destruction.
