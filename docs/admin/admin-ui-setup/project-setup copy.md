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

For researchers, **Projects** provides autonomy over workload resource allocation. Researchers submit workloads and request resources that are then allocated from the project. The workloads are contained within the specific project. This enables the researcher to organize workloads based on the required resources or other commonalities.

## Modeling Projects - not new title, formatting, rewritten content

Administrators can model projects:

* Per individual user.
* Per team of users.
* Per a real organizational Project.

## Node Pools - not new title, formatting, rewritten content

Administrators can assign node pools to the project. By default, all nodes in a cluster are part of the `Default` node pool.
Administrators can choose to create new or additional node pools.
For detailed information on node pools, see [Using node pools](../../Researcher/scheduling/using-node-pools.md).
Each node pool is automatically associated with all Projects and Departments and does not have an associated quota.

When submitting a Job, the Researcher chooses one or more node pools and their priority. If no node pools are added to the job,
the `default priority list` of node pools set by the Administrator is used.
The scheduler tries scheduling the job on the first node pool. If unsuccessful,
it then tries each one in the priority list until it can successfully allocate the resources. If all attempts were unsuccessful, the job is placed in the queue.

## Project Quotas - not new title, formatting, rewritten content

Each Project is allocated with a total quota of GPU and CPU resources (CPU Compute & CPU Memory) and is the sum of all the node pool quotas associated with this Project.
This is a **guaranteed quota** of resources that the project gets regardless of the cluster status.

Users of a Project are able to get more resources than in the quota. This is called **over-quota** and is enabled per project by the Administrator. As long as there are unused GPUs, a Researcher can get more. **However, these GPUs can be taken away at any given moment.**

When the node pools flag is enabled, over-quota is also enabled and calculated per node pool. This means that a workload requesting resources from one node pool whose quota is used up, can get its resources from a quota that belongs to different Project for the same node pool. For more details on over-quota scheduling see [the Run:ai Scheduler](../../Researcher/scheduling/the-runai-scheduler.md).


!!! Note
    (Admonition changed and rewritten) As a rule, the sum of the Projects' allocations should be equal to the number of GPUs in the cluster.

### Controlling Over-Quota Behavior not new title, formatting, rewritten content - removed configuration steps and placed in a task article

By default, the amount of over-quota available for Project members is proportional to the original quota provided above. The [Run:ai scheduler document](../../Researcher/scheduling/the-runai-scheduler.md) provides further examples which show how over-quota is distributed amongst competing Projects. So, for example, a Project with a high **quota** will receive little or no **over-quota**.

<!-- As an administrator, you may want to disconnect the two parameters.  To perform this: -->
## Assign Users to Project  - not new title, formatting, rewritten content - removed configuration steps and placed in a task article

When [Researcher Authentication](../runai-setup/authentication/researcher-authentication.md) is enabled, the Project form will contain an additional *Access Control* tab. The tab will allow you to assign Researchers to their Projects.

If you are using Single-sign-on, you can also assign Groups.

See [Configure projects UI](project-setup-ui.md).

## Other Project Properties
### Limit Jobs to run on Specific Node Groups - not new title, new formatting, rewritten content, removal of procedures and put into a new file

You can assign a Project to run on specific nodes (machines) using two different mechanisms:

* Node Pools: 
    All node pools in the system are associated with each Project. Each node pool can allocate GPU and CPU resources (CPU Compute & CPU Memory) to a Project. By associating a quota on specific node pools for a Project, you can control which nodes a Project can utilize and which default priority order the scheduler will use. Each workload chooses the node pool(s) to use, and if no choice is made, it will use the Project's default 'node pool priority list'.  **Note** Node pools with zero resources associated with a Project, or node pools with exhausted resources, can still be used by a Project when the Over-Quota flag is enabled.

* Node Affinities (Node Type)
    An Administrator can associate specific node sets characterized by a shared run-ai/node-type label to a Project so that descendant workloads can only use nodes from one of those node affinity groups. A workload can specify which node affinity to use out of the list that is bounded to its parent Project.

Some use cases and reasons to use specific nodes for a Project and its descendant workloads are:

* The project team needs specialized hardware (for example, memory).
* The project team is the owner of the specific hardware which was acquired with a specialized budget.
* To direct build/interactive workloads to work on weaker hardware and direct longer training/unattended workloads to work on faster nodes.

### The difference between Node Pools and Affinities - not new title, new formatting, rewritten content, removal of procedures and put into a new file

Node pools represent an independent scheduling domain per Project and are completely segregated from each other. To use a specific node pool (or node pools), the workload must specify the node pool(s) it wants to use.  Affinities represent workloads that ask for a specific affinity and will only be scheduled to nodes marked with that affinity. Workloads that did not specify any affinity might be scheduled as well to those nodes with an affinity. Therefore, the scheduler cannot guarantee a quota for node affinities, only to node pools.

!!!Note 
    Using node pools and affinities narrows down the scope of nodes a specific project is eligible to use and thereby reduces the odds of a specific workload under that Project getting scheduled. In some cases, this may reduce the overall system utilization.

### Further Affinity Refinement by the Researcher - not new title, new formatting, rewritten content, removal of procedures and put into a new file 


The Researcher can limit the selection of node groups by using the CLI flag ``--node-type`` with a specific label. When setting specific Project affinity, the CLI flag can only be used with a node group out of the previously chosen list.  See CLI reference for further information [runai submit](../../Researcher/cli-reference/runai-submit.md) 

## See Also

Run:ai supports an additional (optional) level of resource allocation called [Departments](department-setup.md).
