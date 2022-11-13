# Run:ai Version 2.8

## Release Date
 October 2022 

## Release Content
<!-- 
* Now supporting _spread_ scheduling strategy as well. For more information see [scheduling strategies](../Researcher/scheduling/strategies.md). -->

### Node Pools

Node Pools is a new method for managing GPU and CPU resources by __grouping the resources__ into distinct pools. With node pools:

* You allocate Project and Department resources from these pools to be used by Workloads. 
* The administrator controls which workloads can use which resources, allowing an optimized utilization of resources according to more accurate customer needs. 

<!-- __NEED NEW LINK TO DOCS__ -->

<!-- ### Audit Logs

Audit Log (named: Events History) is a log of all administrative events that occurred in the system. This allows administrators to trace back system configuration changes with full details per event.

__NEED NEW LINK TO DOCS__ -->

### User Interface Enhancements

* The _Departments_ screen has been revamped and new functionality added, including a new and clean look and feel, and improved search and filtering capabilities.
* The _Jobs_ screen has been split into 2 tabs for ease of use:: 
    * _Current_:  (the default tab) consists of all the jobs that currently exist in the cluster. 
    * _History_:  consists of all the jobs that have been deleted from the cluster. Deleting Jobs also deletes their Log (no change).

### Installation improvements 

The Run:ai user interface [requires a URL address](../admin/runai-setup/cluster-setup/cluster-prerequisites/#network-requirements) to the Kubernetes cluster. The requirement is relevant for SaaS installation only. 

In previous versions of Run:ai the administrator should [provide an IP address](../admin/runai-setup/cluster-setup/cluster-prerequisites/#cluster-ip) and Run:ai would automatically create a DNS entry for it and a matching trusted certificate. 

In version 2.8,  the default is for the Run:ai administrator to provide a [DNS and a trusted certificate](https://docs.run.ai/admin/runai-setup/cluster-setup/cluster-prerequisites/#domain-name). 

The older option still exists but is being deprecated due to complexity.

### Inference 
The Deployment details page now contains the URL for the Inference service 


### Hyperparameter Optimization (HPO)

HPO Jobs are now presented as a single line in the Job List rather than a separate line per experiment. 

## Known Bugs

|Internal ID| Description  | Workaround   |
|-----------|--------------|--------------|
|RUN-6237 |Administrator role can see the option to edit / delete department, which is not allowed for this role. |No need. The action will fail.    |
|RUN-6236 |The Run:ai access control system prevents setting a role of researcher together with ML engineer or researcher manager at the same time. However, using the UI you can select these two roles by clicking the text near the check   |     None  |
|RUN-6235 |When you have admin role only, there is an error on the deployments  |Add viewer or ML engineer role to the user.   |
|RUN-6224 |Cpu utilization under nodes list is always zero.               |No workaround. To be fixed in a future hotfix of 2.8                     |
|RUN-6218 |When installing Run:ai on openshift a second time, oauth client secret is incorrect/not updated. As a result, login is not possible                  | Can be performed via manual configuration. Please contact Run:ai support.|
|RUN-6216 |In the multi cluster overview, the allocated GPU in the table of each cluster is wrong. The correct number is in the overview dashboard.             | None                            |
|RUN-6190 |When deleting a cluster, there are leftover pods that are not deleted. No side effects on functionality.                                             | Delete the pods manually.                                                |
|RUN-6187 |"New Department" button is visible to users with 'Administrator' role.  No functional issue as the create new department action will fail with an error.              |None                |
|RUN-5855 |(SaaS version only) The new control plane, versioned 2.8 does not allow the creation of a new deployment on a cluster whose version is lower than 2.8.                |Upgrade your cluster to 2.8                                              |
|RUN-5780 |It is possible to change runai/node-pool label of a running pod. This is a wrong usage of the system and may cause unexpected behavior.              |None.               |
|RUN-5527 |Idle allocated GPU metric is not displayed for MIG workloads in openshift.                      | None                 |
|RUN-5519 |When selecting a Job, the GPU memory utilization metrics is not displayed on the right-hand side. This is an NVIDIA DCGM known bug (see:  https://github.com/NVIDIA/dcgm-exporter/issues/103 ) which has been fixed in a later version but was not yet included in the latest NVIDIA GPU Operator|Install the suggested version as described by NVIDIA.                    |
|RUN-5478 |Dashboard panels of GPU Allocation/project and Allocated jobs per project metrics:  In rare cases, some metrics reflect the wrong number of GPUs     |  None                  |
|RUN-5444 |Dynamic MIG feature does not work with A-100 with 80GB of memory.        |     None               |
|RUN-5424 |When a workload is selected in the job list, the GPU tab in the right panel, shows the details of the whole GPUs in the node, instead of the details of the GPUs used by the workload.                 |None              |
|RUN-5226 |In rare occasions, when there is more than 1 NVIDIA MIG workload, nvidia-smi command to one of the workloads will result with no devices.        | None                   |



## Fixed Bugs

|Internal ID | Description   |
|------------|---------------|
|RUN-5676 |When Interactive Jupyter notebook workloads that contain passwords are cloned, the password is exposed in the displayed CLI command.       |
|RUN-5457 |When using the Home environment variable in conjunction with the ran-as-user option in the CLI, the Home environment variable is overwritten with the user's home directory.   |
|RUN-5370 |It is possible to submit two jobs with the same node-port.       |
|RUN-5314 |When you apply an inference deployment via a file, the allocated GPUs are displayed as 0 in the deployments list.           |
|RUN-5284 |When workloads are deleted while the cluster synchronization is down, there might be a non-existent Job shown in the user interface. The Job cannot be deleted.   |
|RUN-5160 |In some situations, when a Job is deleted, there may be leftover Kubernetes configMaps in the system         |
|RUN-5154 |In some cases, an error "failed to load data" can be seen in the graphs showing on the Job sidebar.             |
|RUN-5145 |The default Kubernetes "priority Class" for deployments is the same as the priority class for interactive jobs.      |
|RUN-5039 |In some scenarios, Dashboards may show "found duplicate series for the match group" error    |
|RUN-4941 |The scheduler is wrongly trying to schedule jobs on a node, where there are allocated GPU jobs at an "ImagePullBackoff" state. This causes an error of "UnexpectedAdmissionError"|
|RUN-4574 |The role "Researcher Manager" is not displayed in the access control list of projects.  |
|RUN-4554 |Users are trying to login with single-sign-on get a "review profile" page.      |
|RUN-4464 |Single HPO (hyperparameter optimization) workload is displayed in the Job list user interfgace as multiple jobs (one for every pod).                                             |


