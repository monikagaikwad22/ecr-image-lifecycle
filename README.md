# Secure Private Container Registry with Image Lifecycle and Retention Policies

## Project Overview

This project demonstrates how to create a secure private container registry using AWS services.
Docker images are stored in Amazon ECR with proper access control and lifecycle policies to automatically manage and clean up old images.

The system improves security, reduces storage usage, and ensures better image management.

---

## Technologies Used

* Docker
* AWS Elastic Container Registry (ECR)
* AWS IAM
* AWS CLI

---

## Architecture

Developer → Docker Image Build → Push to Amazon ECR → Lifecycle Policy Management

---

## Project Objectives

* Create a secure private container registry
* Implement access control using IAM
* Push multiple versions of Docker images
* Configure lifecycle policies to retain the latest images
* Automatically delete older images

---

## Implementation Steps

### 1. Create Private ECR Repository

Create a private repository in Amazon ECR to store container images.

Example repository name:
company-app

---

### 2. Build Docker Image

```id="ydp9k6"
docker build -t company-app:v1 .
```

---

### 3. Tag Docker Image

```id="m3vf6e"
docker tag company-app:v1 718383533665.dkr.ecr.ap-southeast-1.amazonaws.com/company-app:v1
```

---

### 4. Push Image to ECR

```id="ojt1uc"
docker push 718383533665.dkr.ecr.ap-southeast-1.amazonaws.com/company-app:v1
```

---

### 5. Push Multiple Versions

Example image versions:

```id="m9e9ba"
v1
v2
v3
v4
v5
```

---

### 6. Configure Lifecycle Policy

Lifecycle policy automatically removes older images while keeping the latest ones.

Example lifecycle rule:

```json id="d4jkhh"
{
  "rules": [
    {
      "rulePriority": 1,
      "description": "Keep last 3 images",
      "selection": {
        "tagStatus": "tagged",
        "tagPrefixList": ["v"],
        "countType": "imageCountMoreThan",
        "countNumber": 3
      },
      "action": {
        "type": "expire"
      }
    }
  ]
}
```

---

## Lifecycle Policy Result

Before policy:

v1
v2
v3
v4
v5

After lifecycle cleanup:

v3
v4
v5

Older images are automatically deleted.

---

## Security Validation

Access to the ECR repository is restricted using IAM policies.
Unauthorized users attempting to push or pull images will receive an access denied error.

---

## Screenshots

Include screenshots of:

# 1 **ECR repository**

<img width="1920" height="1008" alt="Elastic Container Registry - Create Private Repository - Google Chrome 3_11_2026 11_15_14 PM" src="https://github.com/user-attachments/assets/98549364-00ff-415d-a91e-454e6a5b2802" />

# 2 **ECR Login on terminal**

<img width="1920" height="1008" alt="ubuntu@ip-172-31-21-180_ ~_docker-project 3_11_2026 11_20_52 PM" src="https://github.com/user-attachments/assets/c24e6af8-b558-4e3d-a047-02d6ac0d4fb6" />

# 3 **Create application file v1 to v5**

<img width="1920" height="1008" alt="ubuntu@ip-172-31-21-180_ ~_docker-project 3_11_2026 11_23_00 PM" src="https://github.com/user-attachments/assets/a3d624b4-e24d-42f0-aaf2-2c160f98770e" />

# 4 **Docker Image Build v1 to v5**
![WhatsApp Image 2026-03-11 at 1 49 24 PM](https://github.com/user-attachments/assets/18f54ae0-1386-4662-ab49-b0861fcc869f)

# 5 **Image Tag v1 to v5**

![WhatsApp Image 2026-03-11 at 1 47 19 PM](https://github.com/user-attachments/assets/5b916a8c-e8e7-4764-98e9-e2107c7c4dc8)

# 6 **Image Push v1 to v5**

![WhatsApp Image 2026-03-11 at 1 49 24 PM](https://github.com/user-attachments/assets/43d8ea01-78d7-416a-9021-9d2664a82981)

# 7 **Lifecycle Policy Configure**
````
{
  "rulePriority": 1,
  "description": "Keep last 3 images",
  "selection": {
    "tagStatus": "tagged",
    "tagPrefixList": [
      "v"
    ],
    "countType": "imageCountMoreThan",
    "countNumber": 3
  },
  "action": {
    "type": "expire"
  }
}

````










---

## Conclusion

This project demonstrates how to securely manage container images using Amazon ECR. Lifecycle policies help automate image cleanup, reduce storage costs, and maintain an efficient container registry system.
