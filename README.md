# MongoDB ReplicaSet on RKE Cluster with NFS Persistence

This project demonstrates a production-grade deployment of a highly available MongoDB ReplicaSet on a self-managed Kubernetes cluster (RKE). It includes persistent storage using NFS and automated deployment scripts.

## 🏗 Architecture Overview

The infrastructure consists of:
* **Cluster:** 3-node RKE (Rancher Kubernetes Engine) cluster.
* **Database:** MongoDB ReplicaSet (3 nodes) for High Availability.
* **Storage:** NFS External Provisioner for dynamic Persistent Volume provisioning.
* **Network:** Services exposed via NodePorts (30017, 30018, 30019) for external access.

## 📂 Project Structure

'''text
.
├── rke/                    # Cluster Infrastructure configuration
│   └── cluster.yml
├── k8s/                    # Kubernetes Manifests
│   ├── mongodb/            # MongoDB specific (Values, Secrets)
│   │   ├── mongo-values.yaml
│   │   └── secrets.yaml.example
│   └── storage/            # Storage & NFS configs
│       ├── nfs-storageclass.yaml
│       └── nfs-pvs-replicaset.yaml
├── scripts/                # Automation scripts
│   └── deploy.sh
└── README.md
'''

## 🚀 Getting Started

### Prerequisites
* RKE CLI installed.
* Helm 3 installed.
* Kubectl installed.
* 3 Linux nodes with SSH access.

### Installation

1. **Clone the repository:**
'''bash
git clone <your-repo-url>
cd k8s-mongodb-infrastructure
'''

2. **Configure Secrets:**
Copy the example secret and fill in your passwords:
'''bash
cp k8s/mongodb/secrets.yaml.example k8s/mongodb/secrets.yaml
# Edit k8s/mongodb/secrets.yaml with your passwords
'''

3. **Run Deployment:**
'''bash
chmod +x scripts/deploy.sh
./scripts/deploy.sh
'''

## 🛠 Operational Commands

### Check Replication Status
To verify the MongoDB ReplicaSet status and identify the Primary node:
'''bash
kubectl exec -it my-mongo-mongodb-0 -- mongosh admin -u root -p $MONGODB_ROOT_PASSWORD --eval "rs.status()"
'''

### Connection String (MongoDB Compass)
To connect from your local machine, use the following format:
mongodb://root:PASSWORD@<NODE_IP>:30017/?authSource=admin&directConnection=true

### Test Persistence
1. Connect to MongoDB and insert a document.
2. Delete a MongoDB Pod: kubectl delete pod my-mongo-mongodb-0.
3. Wait for the Pod to restart and verify the data still exists.

## 🔒 Security Note
Passwords and sensitive configuration are managed via Kubernetes Secrets. The secrets.yaml file is excluded from git via .gitignore to prevent credential leaks.