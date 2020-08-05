## August 4th

There is now an optional second level of Project hierarchy called _Departments_. For further information on how to configure and use Departments, see [Working with Departments](../Administrator/Admin-User-Interface-Setup/Working-with-Departments.md) 

## July 28th

You can now enforce a cluster-wise setting which mandates all containers running using the Run:AI CLI to run as __non root__. For further information, see [Enforce non-root Containers](../Administrator/Cluster-Setup/enforce-run-as-user.md)

## July 21th

It is now possible to mount a Persistent Storage Claim using the Run:AI CLI. See the ``--pvc`` flag in the [runai submit](../Researcher/Command-Line-Interface-API-Reference/runai-submit.md) cli flag


## June 13th, 2020

#### New Settings for the Allocation of CPU and Memory

It is now possible to set limits for CPU and memory as well as to establish defaults based on the ratio of GPU to CPU and GPU to memory. 

For further information see: [Allocation of CPU and Memory](../Researcher/Scheduling/Allocation-of-CPU-and-Memory.md)

## June 3rd, 2020

#### Node Group Affinity

Projects now support _Node Affinity._ This feature allows the administrator to assign specific projects to run only on specific nodes (machines). Example use cases:

*   The project team needs specialized hardware (e.g. with enough memory)
*   The project team is the owner of specific hardware which was acquired with a specialized budget
*   We want to direct build/interactive workloads to work on weaker hardware and direct longer training/unattended workloads to faster nodes

For further information see: [Working with Projects](../Administrator/Admin-User-Interface-Setup/Working-with-Projects.md)

#### Limit Duration of Interactive Jobs

Researchers frequently forget to close Interactive jobs. This may lead to a waste of resources. Some organizations prefer to limit the duration of interactive jobs and close them automatically. 

For further information on how to set up duration limits see: [Working with Projects](../Administrator/Admin-User-Interface-Setup/Working-with-Projects.md)

## May 24th, 2020

#### Kubernetes Operators

Cluster installation now works with Kubernetes _Operators_. Operators make it easy to install, update, and delete a Run:AI cluster. 

For further information see: [Upgrading a Run:AI Cluster Installation](../Administrator/Cluster-Setup/Upgrading-Cluster-Install.md) and [Deleting a a Run:AI Cluster Installation](../Administrator/Cluster-Setup/Deleting-Cluster-Install.md)

## March 3rd, 2020

#### Admin Overview Dashboard

A new admin overview dashboard which shows a more holistic view of multiple clusters. Applicable for customers with more than one cluster.