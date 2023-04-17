# Kanister - Application-Level Data Management

## Why do i need to use Kanister

When working with systems that store and manage important data (like complex databases), special tools are needed to make sure that the data is handled correctly. This is especially true when these programs are designed to run on cloud-based servers. 

The tools that are typically used to manage cloud-based programs are good at controlling the basic functions of the servers (like turning them on and off), but they don't always have the ability to manage the data in the same sophisticated way that other tools can. 

So, when it comes to managing complex databases, for example, we need to use tools that are specifically designed to understand the data and how it's organized.

## Kubernetes Native Backup

Kubernetes-native backup is one of the most critical, yet overlooked, challenges faced by Kubernetes operators today.

It uses its own placement policy to distribute application components across all servers for fault-tolerance and performance. As such, different applications may be co-located on the same server. Also, there is the dynamic nature of cloud native environments: containers can be dynamically rescheduled or scaled on different nodes for better load balancing. New deployments, that happen on an hourly basis, can involve rolling upgrades, and new application components can be added or removed at any time.

In short, the definition of a cloud native application is constantly shifting. A data management solution needs to understand this cloud native architectural pattern, be able to work with a lack of IP address stability, and be able to deal with continuous change.

Purpose-built for Kubernetes, Kasten K10 provides an easy-to-use, scalable, and secure system for backup/restore, disaster recovery, and mobility of Kubernetes applications.

## Kanister vs K10

For stateful, cloud-native applications, data operations must often be performed by tools with a semantic understanding of the data. The volume-level primitives provided by orchestrators are not sufficient to support data workflows like backup/recovery of complex, distributed databases.

## Kasten K10 Actions

An Action API resource is used to initiate Kasten K10 data management operations. The actions can either be associated with a Policy or be stand-alone on-demand actions. Actions also allow for tracking the execution status of the requested operations.

### BackupAction

Backup actions are used to initiate backup operations on applications. A backup action can be submitted as part of a policy or as a standalone action.

### RestoreAction

Restore actions are used to restore applications to a known-good state from a restore point.

### ExportAction

Export actions are used to initiate an export of an application to external data storage, such as S3-compatible object stores, using an existing restore point.

### BackupClusterAction

Backup cluster actions are used to initiate backup operations on cluster-scoped resources. A backup cluster action can be submitted as part of a policy or as a standalone action.

### RestoreClusterAction

Restore cluster actions are used to restore cluster-scoped resources from a ClusterRestorePoint. A restore cluster action can be submitted as part of a policy or as a standalone action.

### RunAction

RunActions are used for manual execution and monitoring of actions related to policy runs.

### CancelAction

CancelActions are created to halt progress of another action and prevent any remaining retries. Cancellation is best effort and not every phase of an Action may be cancellable. When an action is cancelled, its state becomes Cancelled.

### ReportAction

A ReportAction resource is created to generate a K10 Report and provide insights into system performance and status. A successful ReportAction produces a K10 Report that contains information gathered at the time of the ReportAction.

## Policies

Policies are used to automate your data management workflows. A Policy custom resource [CR] is used to perform operations on K10 Policies.

K10 Policies allow you to manage application protection and migration at scale. 

## Policy Scheduling

Within the Policy API Type, K10 provides control of:

- How often the primary snapshot or import action should be performed

- How often snapshots should be exported into backups

- Which and how many snapshots and backups to retain

- When the primary snapshot or import action should be performed

Location profiles are used to create backups from snapshots, move applications and their data across clusters and potentially across different clouds, and to subsequently import these backups or exports into another cluster.

K10 can usually invoke protection operations such as snapshots within a cluster without requiring additional credentials. While this might be sufficient if K10 is running in some of (but not all) the major public clouds and if actions are limited to a single cluster, it is not sufficient for essential operations. These include performing real backups, enabling cross-cluster and cross-cloud application migration, and enabling DR of the K10 system itself.

To enable these actions that span the lifetime of any one cluster, K10 needs to be configured with access to external object storage or external NFS file storage. This is accomplished via the creation of Location Profiles.

## Kanister-Enabled Applications

K10 defaults to volume snapshot operations when capturing data, but there are situations where customization is required. For example, the best way to protect your application's data may be to take a logical dump of the database. This requires using tools specific to that database.

Kanister uses Blueprints to define these database-specific workflows and open-source Blueprints are available for several popular applications. It's also simple to customize existing Blueprints or add new ones. When configured to do so, K10 will automatically use Kanister Blueprints and manage the resulting application-level Kanister artifacts.

## Specifying a Kanister Blueprint for Your Application

To request K10 to use a custom Kanister Blueprint to manage a workload (e.g., Deployment or StatefulSet), please (a) create the Blueprint in the K10 namespace (default kasten-io) and (b) annotate the workload with the name of the created blueprint as follows:

```yaml
kubectl --namespace=<app-namespace> annotate <workload>/<workload-name> \
    kanister.kasten.io/blueprint=<blueprint-name>
```

Finally, note that the Blueprint used must have a backup and restore action defined and ideally a delete action for retirement too.

## Use Case Testing

Once you have installed your application, you will be able to use the K10 Dashboard to bring the application in compliance and protect it by creating one or more policies. You can also subsequently restore the application and its data to a previous version.