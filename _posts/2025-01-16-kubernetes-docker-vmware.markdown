---
layout: post
title:  "Kubernetes, Docker, VMware, & AWS"
date:   2025-01-16 13:43:52 -0500
categories: kubernetes docker vmware virtualization
---
# Preface
The main reason I am posting this, is that I have had plenty of conversations where [k8s][kubernetes] has been mentioned and, before I was familiar with it, it sounded quite similar (logically speaking) to [VMware][vmware]. Thus, I writing a post so I can effectively convey the similarities and the differences between these two methods of logically organizing compute elements, while also including documentation specific to application containerization at the container level with Docker. In essence, I find this important because in some of these cases the other party was not familiar with VMware, so I hope this provides a translational layer between the technologies. Of note, while I have experience with [AWS][aws], this was later in my experience than VMware.

# VMware

In terms of VMware, in my experience it normally exists in enterprise environments, where 2k+ servers could be considered a Medium Environment Size by [VMware Storage Requirements (Table 1)][vmware-storage-requirements] and in many cases was a layer atop what was originally primarily physical server rooms and racks. For clarity's sake, while I have seen VMware in smaller environments, it is not as frequent.

VMware, hierarchically speaking, usually comprises a vCenter Server, that is essentially a Root Node of the tree of the VMware Environment, where and logical children of that root node are [ESX(i)][esxi] servers, that is virtual/physical servers running the ESX(i) operating system. 

These ESX(i) servers can be logically clustered (in VMware [Clusters][vmware-clusters]) such that resources are dynamically allocated between hosts -- meaning resources can be shared between hosts within the cluster, through [Resource Pools][resource-pool]. Within the Operating System layer of these ESX(i) hosts, are VMs (Virtual Machines) that can be migrated, replicated, failed over, etc. between ESX(i) hosts in the VMware environment within that cluster. Traditionally, VMs handle running one application per VM, where virtual machines are allocated only as much as they need, with a little overhead because this reduces resource contention and in many cases helps with Virtual Machine performance in the VMware environment.

# Kubernetes

Kubernetes is different in that the logical structure is different from VMware. Specifically, where VMware has clusters of hosts of VMs, k8s has clusters of nodes of pods of containers, where:

- [k8s clusters][k8s-cluster] are similar with VMware ESX(i) clusters, 
- [nodes][k8s-node] are essentially VMs, especially as it is reflected within VMware, and
- [k8s pods][k8s-pods] can handle multiple containers within a single VM.
- [containers][k8s-container] are what are contained within a pod and tend to contain an application

Having said that, Kubernetes absolutely can, and frequently does exist within a VMware environment — that is, they are not mutually exclusive.

# Docker

One step further things like Docker exist simplifying our ability to manage containers, specifically. Thus, what containers are directed by Kubernetes, and possibly deployed within VMware,  could be managed at the container level by something like [Docker][docker]. In essence, containers can be packaged within Docker and managed by Kubernetes.

Rather, there is traditionally an infrastructure team handling deployments within VMware (if it exists within that environment) and within this environment, VMs could be allocated for DevOps engineers so that they could handle Kubernetes Deployments.

VMware is more of a virtualization platform at the Operating System layer (not considering networking, storage, etc.) where Kubernetes and Docker are application-layer virtualization platforms, and where Kubernetes is a tool for “orchestrating containerization” through Docker.

# AWS, VMware, Kubernetes, & Docker

In essence, VMware is an environment where VMs could host a Kubernetes environment that is orchestrating containerization through Docker. Alternatively, AWS can do this as well, where AWS could host a vCenter appliance within [VMware Cloud on AWS][vmware-cloud], and from there host Kubernetes orchestrating a Docker environment. Here we can see a set of physical servers hosting a vCenter server, that has a clusters with resource pools, and VMs with k8s clusters of pods handling direction of Docker containers!

![Image]({{ site.baseurl }}/assets/kubernetes.png)

# Conclusion

VMware, Kubernetes, Docker, and AWS are all various functions for performing computation in varying levels. Whereas VMware and AWS are more full-fledged, Docker and Kubernetes handle virtualization at the application layer, and are normally hosted within an Operating System hosted within AWS/VMware.

[docker]: https://www.docker.com/
[vmware]: https://www.vmware.com/
[kubernetes]: https://kubernetes.io/
[aws]: https://aws.amazon.com/
[vmware-storage-requirements]: https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vcenter.upgrade.doc/GUID-FB268055-5D36-4624-A64C-9800D3FCB689.html
[esxi]: https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/vsphere/8-0.html
[vmware-clusters]: https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vcenterhost.doc/GUID-F7818000-26E3-4E2A-93D2-FCDCE7114508.html
[resource-pool]: https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.resmgmt.doc/GUID-60077B40-66FF-4625-934A-641703ED7601.html
[k8s-cluster]: https://kubernetes.io/docs/concepts/architecture/
[k8s-node]: https://kubernetes.io/docs/concepts/architecture/nodes/
[k8s-pods]: https://kubernetes.io/docs/concepts/workloads/pods/
[k8s-container]: https://kubernetes.io/docs/concepts/containers/
[vmware-cloud]: https://docs.aws.amazon.com/prescriptive-guidance/latest/identity-access-management-vmware-cloud-aws/welcome.html