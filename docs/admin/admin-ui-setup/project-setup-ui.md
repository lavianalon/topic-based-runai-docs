# Configure projects UI

## Create a Project

!!! Note 
    To be able to create or edit Projects, you must have *Editor* access. See the [Users](admin-ui-users.md) documentation.
To create a project:

1. Login to the Projects area of the Run:ai user interface at `<company-name>.run.ai`.
2. On the top right, select "Add New Project".
3. Choose a Project name and a Project quota.
4. Press "Save".

## Configure over-quota

To configure over-quota settings:

1. Under `General | Settings` turn on the `Enable Over-quota Priority`.
2. When creating a new Project, move the slider for over-quota priority from`None` to `High`.

## Setting Node Pools for a Specific Project

By default, all node pools are associated with every Project and Department using zero resource allocation. This means that by default any Project can use any node-pool if Over-Quota is set for that Project, but only for preemptible workloads (for example, Training or Interactive workloads using Preemptible flag).

To guarantee resources for all workloads including non-preemptible workloads, the administrator should allocate resources in node pools.

To configure resources for node pools

1. Go to the _Node Pools_ tab under Project and set a quota to any of the node pools (GPU resources, CPU resources) you want to use.
2. To set the Project's default node pool's order of priority, you should set the precedence of each node pool, this is done in the Project's node pool tab.
3. The node pool default priority order is used if the workload did not specify its preferred node pool(s) list of priority.
4. To mandate a Workload to run on a specific node pool, the Researcher should specify the node pool to use for a workload. 
5. If no node-pool is specified - the Project's 'Default' node-pool priority list is used. 
6. Press 'Save' to save your changes.

## Setting Affinity for a Specific Project
 
To mandate Jobs to run on specific node groups:

1. Create a Project or edit an existing Project.
2. Choose either **training** or **interactive**.
3. Go to the *Node Affinity* tab and set a limit to specific node groups.
4. If the label does not yet exist, press the + sign and add the label.
5. Press Enter to save the label.
6. Select the label.

## Limit Duration of Interactive and Training Jobs

As interactive sessions involve human interaction, you can additional enforce a policy that sets the time limit for such sessions. This policy is often used to handle situations for when sessions are left open when there is no need to access the resources.

!!! Warning
    This feature will cause containers to automatically stop. Any work not saved to a shared volume will be lost.

To set a duration limit for interactive Jobs:

1. Create a Project or edit an existing Project.
2. Go to the *Time Limit* tab. This is effective for Interactive Jobs that are Preemptible, non-Preemptible, or both.  You can limit interactive Jobs using two criteria:
    1. Set a hard time limit (day, hour, minute) to an Interactive Job, regardless of the activity of this Job (for example, stop the Job after 1 day of work.)
    2. Set a time limit for Idle Interactive Jobs (for example, an Interactive Job idle for `X` time is stopped. Idle means no GPU activity.)

!!! Note
    The setting only takes effect for Jobs that have started after the duration has been changed.

In some cases you may want to stop a Training Job if `X` time elapsed since it has started to run. This can be to clean up stale Training Jobs or Jobs that are running for too long due to the wrong parameters set or other errors in the model.

To set a duration limit for Training Jobs:

1. Create a Project or edit an existing Project.
2. Go to the *Time Limit* tab and set a time limit for Idle Training Jobs (for example, a Training Job idle for `X` time is stopped. Idle means no GPU activity.)
    
!!! Note
    The setting only takes effect for Jobs that have started after the duration has been changed. 
