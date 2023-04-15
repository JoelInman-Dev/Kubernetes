# Kubernetes Backup Policies and Processes

## Step by step procedure

I am following along with Kasten K10s guide on KubeCampus. I am hoping to learn different methods of disaster recovery and data retention and security policies within a cluster environment.

### Step 1: Add the Kasten K10 Helm repository

    The Helm package manager (v3.0.0+) and the Kasten Helm charts repository is required.

Installing K10 via helm will also automatically create a new Service Account to grant K10 the required access to your Kubernetes resources.

### Step 2: Install K10

Following a successful installation, there are several options for setting up access to the Kasten K10 dashboard. For more information, refer to [Dashboard Access](https://docs.kasten.io/latest/access/dashboard.html#dashboard) documentation.

### Step 3: Backup Policy Creation

K10 gives you the ability, with extremely minor application modifications, to add functionality to backup, restore, and migrate application data in an efficient and transparent manner.

### Step 4: Restore from Backup

Restore the data using K10 by selecting the appropriate restore point.

---

Today’s complex enterprise IT workloads require the scale and flexibility that only Kubernetes can deliver. But security issues facing Kubernetes users continue to disrupt critical operations, particularly as ransomware inflicts increasing damage on enterprises.

Conservative estimates put global ransomware losses upwards of $20 billion in 2020, with 2021 expected to be even worse.

While the primary goal is to be able to prevent a ransomware attack altogether, it is equally as important to plan for how to recover from one. Hardened backup capabilities are a must and enterprise IT teams should be aware of the different operating requirements for Kubernetes applications in contrast to legacy, monolithic systems.

**Application State and Configuration Data** – Each application must include the state that spans across storage volumes and databases (NoSQL/relational), as well as configuration data included in Kubernetes objects such as configmaps and secrets.

**Snapshots versus Backups** – Snapshots are typically stored alongside primary data and are not fully isolated. This means that, alone, snapshots might not be available if something happens to the cluster holding them, rendering them unreliable for recovery and long-term data retention due to potential data loss.

**Application Security for Cloud-Native Environments** – Cloud-native environments have shifted the importance and function of application security. End-to-end encryption and customer-owned management capabilities are paramount and should include integrated authentication and role-based access control (RBAC).

Lastly, application security must allow for a quick recovery from ransomware attacks.

---

## Prioritizing Recovery

Despite planning for disaster, should a ransomware attack succeed, recovery is an organization’s next line of defense. Ransomware attacks are not one-size-fits all and attackers work diligently to find the right targets, as well.

In the case of Kubernetes, an attack on a cluster may stem from something as “simple” as an overlooked, unauthenticated endpoint or an unpatched vulnerability. In the event of a successful attack, fast restores are essential to protecting sensitive data from being exploited and resuming business operations quickly.

Enterprise IT teams run and maintain thousands of applications across different locations using platforms like Kubernetes that enable automation – overseeing all of them manually is a task nearly beyond human capabilities.

Tools for backup and recovery functions should also promote automation and integrate seamlessly into existing workflows. Enabling immutability, creating backups with unique code paths, protecting backups for maximum effectiveness and enabling seamless restores are part of a robust ransomware data protection strategy.

If an organization is attacked by ransomware, and there is no plan to defend against it, the attack can result in significant business disruption and major financial losses, even without paying ransom (which a business should never ever ever do).

A robust ransomware data protection strategy can enable organizations to recover from data loss and ransomware attacks with sustained success.

Beyond tooling, IT teams need to educate stakeholders on how to avoid ransomware and detect phishing campaigns, suspicious websites and other scams, like social engineering, while security resources should be aimed at hardening application infrastructure – systems and networks – and maintaining all software updates on a regular basis.

For applications running on Kubernetes, IT teams need to be aware of their unique requirements to protect them and ensure that backup and recovery capabilities are hardened accordingly. Kubernetes is the unifying fabric of modern computing. Using it to mitigate the most pressing data threats in today’s risk landscape is one of the best defenses yet.
