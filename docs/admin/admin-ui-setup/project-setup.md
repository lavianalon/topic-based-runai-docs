---
title: Projects Overview
summary: This article describes the concepts needed for setting up a project.
authors:
    - Jason Novich
date: 2022-May-15
---
# Introduction

Run:ai introduces the concept of **Projects** to streamline resource allocation, prioritize work, and create segregation between different initiatives. 
A project can represent a team, an individual, or an initiative that shares resources, or has specific resource quotas.

Researchers who submit jobs need to associate a Project name with the Job request. The Run:ai scheduler will compare the request against the current 
allocated resources and determine whether the job should be schedules (with resources) if it should remain in the queue for future allocation.

A project consists of the following:

* [Node Pools](#node-pools)
* [Project Quotas](#project-quotas)
* [Assigned users](#assign-users-to-project)

## Modeling Projects

Administrators can model projects:

* Per individual user.
* Per team of users.
* Per a real organizational Project.

## Node Pools

Administrators can assign node pools to the project. By default, all nodes in a cluster are part of the `Default` node pool.
Administrators can choose to create new or additional node pools.
For detailed information on node pools, see [Using node pools](../../Researcher/scheduling/using-node-pools.md).
Each node pool is automatically associated with all Projects and Departments and does not have an associated quota.

When submitting a Job, the Researcher chooses one or more node pools and their priority. If no node pools are added to the job,
the `default priority list` of node pools set by the Administrator is used.
The scheduler tries scheduling the job on the first node pool. If unsuccessful,
it then tries each one in the priority list until it can successfully allocate the resources. If all attempts were unsuccessful, the job is placed in the queue.

## Project Quotas

Each Project is allocated with a total quota of GPU and CPU resources (CPU Compute & CPU Memory) and is the sum of all the node pool quotas associated with this Project.
This is a **guaranteed quota** of resources that the project gets regardless of the cluster status.

Users of a Project are able to get more resources than in the quota. This is called **over-quota** and is enabled per project by the Administrator. As long as there are unused GPUs, a Researcher can get more. **However, these GPUs can be taken away at any given moment.

When the node pools flag is enabled, over-quota is also enabled and calculated per node pool. This means that a workload requesting resources from one node pool whose quota is used up, can get its resources from a quota that belongs to different Project for the same node pool. For more details on over-quota scheduling see [the Run:ai Scheduler](../../Researcher/scheduling/the-runai-scheduler.md).


!!! Note
    As a rule, the sum of the Projects' allocations should be equal to the number of GPUs in the cluster.

### Controlling Over-Quota Behavior

By default, the amount of over-quota available for Project members is proportional to the original quota provided above. The [Run:ai scheduler document](../../Researcher/scheduling/the-runai-scheduler.md) provides further examples which show how over-quota is distributed amongst competing Projects. So, for example, a Project with a high **quota** will receive little or no **over-quota**.

<!-- As an administrator, you may want to disconnect the two parameters.  To perform this: -->
## Assign Users to Project

When [Researcher Authentication](../runai-setup/authentication/researcher-authentication.md) is enabled, the Project form will contain an additional *Access Control* tab. The tab will allow you to assign Researchers to their Projects.

If you are using Single-sign-on, you can also assign Groups.

See [Configure projects UI](project-setup-ui.md).

## Other Project Properties
### Limit Jobs to run on Specific Node Groups

You can assign a Project to run on specific nodes (machines). This is achieved by two different mechanisms:

*   Node Pools: 
        All node pools in the system are associated with each Project. Each node pool can allocate GPU and CPU resources (CPU Compute & CPU Memory) to a Project. By associating a quota on specific node pools for a Project, you can control which nodes a Project can utilize and which default priority order the scheduler will use (in case the workload did choose so by itself). Each workload should choose the node pool(s) to use, if no choice is made, it will use the Project's default 'node pool priority list'. Note that node pools with zero resources associated with a Project or node pools with exhausted resources can still be used by a Project when the Over Quota flag is enabled.

*   Node Affinities (aka Node Type)
        Administrator can associate specific node sets characterized by a shared run-ai/node-type label value to a Project. This means descendant workloads can only use nodes from one of those node affinity groups. A workload can specify which node affinity to use, out of the list is bounded to its parent Project.

There are many use cases and reasons to use specific nodes for a Project and its descendant workloads, here are some examples:
 
*   The project team needs specialized hardware (e.g. with enough memory).
*   The project team is the owner of specific hardware which was acquired with a specialized budget.
*   We want to direct build/interactive workloads to work on weaker hardware and direct longer training/unattended workloads to faster nodes.

#### The difference between Node Pools and Affinities

Node pools represent an independent scheduling domain per Project, therefore are completely segregated from each other. To use a specific node pool (or node pools), any workload must specify the node pool(s) it would like to use. While for affinities, workloads that ask for a specific affinity will only be scheduled to nodes marked with that affinity, while workloads that did not specify any affinity might be scheduled as well to those nodes with an affinity. Therefore the scheduler cannot guarantee quota for node affinities, only to node pools.


Note that using node pools and affinities narrows down the scope of nodes a specific project is eligible to use. It, therefore, reduces the odds of a specific workload under that Project getting scheduled. In some cases, this may reduce the overall system utilization.



#### Further Affinity Refinement by the Researcher

The Researcher can limit the selection of node groups by using the CLI flag ``--node-type`` with a specific label. When setting specific Project affinity, the CLI flag can only be used with a node group out of the previously chosen list.  See CLI reference for further information [runai submit](../../Researcher/cli-reference/runai-submit.md) 

## See Also

Run:ai supports an additional (optional) level of resource allocation called [Departments](department-setup.md). 
