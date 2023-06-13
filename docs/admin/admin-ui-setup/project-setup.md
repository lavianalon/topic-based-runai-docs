---
title: Projects Overview
summary: This article describes the concepts needed for setting up a project.
authors:
    - Jason Novich
date: 2022-May-15
---
# Introduction to Projects

The **Projects** feature gives administrators and researchers benefits in managing the system. Benefits include:

* More insight and control over system resources.
* Improved prioritization of workloads.
* Efficient organizational structure.

For administrators, **Projects** provides improved monitoring of used and unused resources. Administrators are able to allocate system resources more effectively by assigning specific amount of resources to a project. Administrators are able to monitor resource utilization of projects providing more insight into system performance. Administrators also improve efficiency by assigning users (Researchers) to one or more projects providing better access control and isolation between initiatives.

For researchers, **Projects** provides autonomy over workload resource allocation. Researchers submit workloads and request resources that are then allocated from the project. The workloads are contained within the specific project which enables the researcher to organize workloads based on required resources or other commonalities.

## Quota management

Administrators assign quotas to specific projects through the node pools. Each Project is allocated with a total quota of GPUs, CPUs, and CPU memory based.  The total resources available for the project is the sum of all the node pool quotas. These resources are guaranteed to the project regardless of the cluster status. Node pools can set in order of workload running priority. For more information, see [Node pool priority](../../Researcher/scheduling/using-node-pools.md#multiple-node-pools-selection).

### Over quota

**Over quota** is the ability for the project or workload to receive more resources that specified in the quota settings. If there are resources that are unused, the project will be able to get more. This means that a workload requesting resources from one node pool whose quota is used up, can get its resources from a quota that belongs to different Project for the same node pool.

!!! Note
    GPUs that were allocated as **over quota** will be reassigned when needed for other workloads.

 **Over quota** is enabled per node pool and assigned an over quota priority. For more information, see [Over quota priority](../../Researcher/scheduling/the-runai-scheduler.md#over-quota-priority).

## Scheduling rules

Scheduling rules provide a means to control the projects compute resources. Rules can be applied based on a duration and utilization of resources.

### Idle GPU timeout

This rule controls the amount of time that GPUs which are idle will be remain assigned to the project. When the time elapses the GPUs will be resassigned as needed for other projects or for over quota requests. This rule is based on the workload type and is configured in days, hours, and minutes.

### Workspace duration

Setting a workspace duration will limit the length of time a workspace will run for. Workspaces are interactive sessions that typically require human interaction. This rule is used in cases where researchers have left their training sessions open, which leaves idle resources allocated. This rule enables better resource and over quota management. The workspace will stop regardless of the activities currently running and is configured in days, hours, and minutes.

### Training duration

Setting a training duration for a project will limit the length of time training workloads will run for. This enables better resource and over quota management. The training will stop regardless of the activities currently running and is configured in days, hours, and minutes.

### Node type (Affinity)

Node affinity is the ability to assign a Project to run on specific nodes. Machine learning frequently has workloads
that require the use of a CPU but not GPU. Node Affinity ensures workloads that are running receive optimized resources. This means workloads can only use nodes from one of those node affinity groups. A workload that specified which node to use, is bounded to it by the Project. There are a number of reasons to use specific nodes for a Project:

* The project needs specialized hardware.
* The project team is the owner of specific hardware.
* Direct specific workloads to work on hardware that optimzed for the workload.

## Working with Projects

To configure your project, see [Configuring Projects]().
