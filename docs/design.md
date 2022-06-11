# Design Descisions

Reference: [https://www.redhat.com/en/blog/how-does-red-hat-support-day-2-operations](https://www.redhat.com/en/blog/how-does-red-hat-support-day-2-operations)

## Day 0 Operations

### Planning - How do we do Ops

### Planning - Sizing

### One ACM Hub / Bastion Host per Region

- Currently, we support AKS cluster deployed across the NA region. A single ACM hub cluster will be available to import and manage all the AKS clusters in the North America region.
- The EMEA region will consist of Europe, Middle East, and Africa countries.

## Day 1 Operations

### New Environment Deployments

## Day 2 Operations

### Importing AKS Clusters

AKS clusters are imported into the ACM hub as a Day 2 operation based on an ansible playbook.
The idempontent playbook is run against an inventory of AKS clusters.

The following tasks are performed on each cluster:

1. Setup network to allow the AKS cluster to see the ACM hub
2. Setup networking to allow the AAP cluster to access the k8s endpoint.
3. Generate the ManageCluster CR
4. Read the generated import secret, and apply these on the AKS cluster.

As new AKS clusters come online, the ansible playbook running on a schedule will iterate over the inventory of clusters and import the clusters into ACM. 

!!! note

    If it takes 5m to run the import playbook from start to end for a single AKS cluster, then iterating over 100 AKS clusters, will take 500m or 8.3 hours, as a rough estimate. Enabling parallism will reduce the total time.


An alternative import procedure is available, that uses a generic import payload to be applied on the AKS cluster. This requires the ACM hub to be configured to generate a generic payload. Using this procedure will simplify the import procedure by not having to access the ACM hub cluster during the import procedure. We would just need to access the target cluster. This will still be a Day 2 operation.


### Backup and Restore | Disaster Recovery

Stuff

### Upgrade - OCP

Stuff

### Upgrade - ACM

Stuff

### Alerts and Alert Management

Stuff

### Observability

Stuff


