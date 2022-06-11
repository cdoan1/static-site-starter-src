# Fleet Health

## Determining ACM Health

- The Red Hat Advanced Cluster Management product is deliverated as an Operator Hub Operator.
- The Operator is deployed with a Subscription defintion.
- The instance Operand manifest is applied, requiring only a reference to an image pull secret.
- In the 2.4 release, we can verify the state of the ACM deployment by monitoring the status output of the multiclusterhub CR. The multiclusterhub CR reflects the state of the `MulticlusterHub Operator`, or **MCH**. If all the ACM components are successfully deployed, then the final status of the multiclusterhub CR will be **Running**. A state of `Progressing` indicates that the install process has not yet completed. A `Blocked` status indicates that the install or upgrade process was interrupted due to missing requirement or conflict that prevents the process from starting.
- In the 2.5 release, the cluster-lifecycle subcomponent was separated out as a standalone product component called `Multicluster Engine`, with a short name short name of **MCE**. When we deploy RHACM 2.5, both product component operators will be deployed. The `Multiclusterhub` operator and the `Multicluster Engine` operator. The **MCH** will refect the same status as before, with **Running** indicating sucessful completion of install or upgrade. The **MCE** will show a status of **Available** to indicate that its install/upgrade processing has completed successfully.

!!! note

    See this diagram to see the relationship between MCH/MCE and the interconnected pods and namespaces

- The MCH and MCE reflect the deploy time status of RHACM. We can derive the instantious pod state weither they are running or crashing, and historically if the pods have a history of restarting. From this we cannot tell if the internal function are nominal.
- To determine ACM Heath from metrics and utilization, the Openshift Monitoriing components can be inspected. If ACM Observability is enabled, then the ACM Hub (local-cluster) is aggregated with the rest of the fleet data and available in the aggregate Grafana dashboard.

### What is needed

- We need to understand the API latency on each ACM subcomponent.
- We need to be able to measure ACM to determine SLO.
- The fact that ACM is a workload running on top of Openshift, knowing the k8s apiserver SLO, does not necessarily reflect the SLO for the ACM component api.
- There is no specific dashboard that shows data for the ACM components. Up to now, we manually create the view, without ever persisting the views to a reusable custom dashboard.



