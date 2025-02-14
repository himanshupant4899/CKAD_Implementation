# CKAD_Implementation_1

# Jekyll Kubernetes Setup

This project sets up a Kubernetes environment with a jekyll pod, persistent storage, service configuration, and RBAC settings for managing resources in the development namespace.

## Requirements

### Pod Configuration
- *Pod Name*: jekyll
- *Init Container*: 
  - Name: copy-jekyll-site
  - Image: gcr.io/kodekloud/customimage/jekyll
  - Command: 
    bash
    jekyll new /site
    
  - Mount Path: /site
  - Volume Name: site
  
- *Container*: 
  - Name: jekyll
  - Image: gcr.io/kodekloud/customimage/jekyll-serve
  - Volume Name: site
  - Mount Path: /site
  - Persistent Volume Claim (PVC): jekyll-site

- *Pod Label*: run=jekyll

### PersistentVolume and PVC
- *Persistent Volume*: 
  - PVC Name: jekyll-site
  - Namespace: development
  - Storage Request: 1Gi
  - Access Modes: ReadWriteMany

- The jekyll-site PVC should be bound to the PersistentVolume named jekyll-site.

### Service Configuration
- *Service Name*: jekyll
- *Target Port*: 4000
- *Service Port*: 8080
- *NodePort*: 30097
- *Namespace*: development

### RBAC Configuration
- *Role Binding*: developer-rolebinding
  - Role: developer-role
  - Namespace: development
  - Associated User: martin

### Role Configuration
- *Role Name*: developer-role
  - Permissions:
    - All (*) permissions for services in the development namespace.
    - All (*) permissions for PersistentVolumeClaims in the development namespace.
    - All (*) permissions for pods in the development namespace.

## Setup Instructions

1. *Create Persistent Volume*:
    - Define the PersistentVolume for jekyll-site in the development namespace.
  
2. *Create Persistent Volume Claim*:
    - Ensure the PVC jekyll-site is created and bound to the PersistentVolume.

3. *Deploy Pod*:
    - Deploy the jekyll pod with the necessary init container and volume configuration.

4. *Create Service*:
    - Create a service for jekyll with target ports 4000, 8080, and NodePort 30097.

5. *Configure RBAC*:
    - Create the developer-role with all permissions for services, PVCs, and pods in the development namespace.
    - Bind the developer-role to the user martin via the developer-rolebinding.


