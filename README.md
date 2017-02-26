# aerospike-kube

This project contains the init container used in Kubernetes (k8s) and the aerospike petset definition

## Usage:

Configure `image/aerospike.conf` for your aerospike cluster.

Build and push the aerospike-install container to the Docker registry of your choice (See image/README.md)

Use the aerospike-install image as the init-container for the PetSet

Deploy the petset: `kubectl create -f aerospike-statefulset.yaml`

## Requirements

* Kubernetes 1.5+ with beta features (PetSet, init containers)

