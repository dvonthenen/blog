---
post_title: 'Helm Chart for vSphere Cloud Controller Manager'
layout: post
published: true
---
If you did know, [Helm](https://github.com/helm/helm) was accepted into the [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/blog/2018/06/01/cncf-to-host-helm/) list of incubating projects in June of this year. Even before it's acceptance, Helm has established itself as a powerful application in the Kubernetes community and has a large a vibrant ecosystem as a standalone project. In fact, it's grown to such a significant size that Helm has even had it's own conference sole based on the Helm project.

### What is Helm?

![Helm Logo](https://github.com/dvonthenen/blog/raw/master/images/helm.png)

So enough talk about the accolades, what is Helm? Helm is a tool that streamlines installing and managing Kubernetes applications. Think of it as a package manager, like apt or yum, for a Kubernetes environment. Helm deploys applications to a Kubernetes cluster via a [Helm Chart](https://github.com/helm/charts/), which is nothing more than a curated application definition for Helm. Think of a Helm Chart as a deployment definition similar to how RPM feeds into yum.

Helm and Charts enables:
- Ease of application distribution via Helm Charts
- Reproducible builds of your Kubernetes applications
- Intelligent management of your Kubernetes manifest files
- Release management of Helm packages

### What is the vSphere Cloud Controller Manager?

Currently, the way Kubernetes handles cloud provider specific functionality is by embedding code directly into the Kubernetes code base. They are commonly referred to as in-tree cloud providers. There is an effort to [move that cloud provider specific code out-of-tree](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/cloud-provider/cloud-provider-refactoring.md) or external to the Kubernetes code base.

![VMware Logo](https://github.com/dvonthenen/blog/raw/master/images/vmware_logo.png)

The [vSphere Cloud Controller Manager (CCM)](https://github.com/kubernetes/cloud-provider-vsphere) is an external cloud provider that handles functionality specific to VMware vSphere infrastructure running on Kubernetes. There will be additional components that will sit beside the vSphere CCM, such as an out-of-tree vSphere CSI plugin that will handle storage related functionality for vSphere. More information on that will be revealed shortly.

### How do I Deploy the vSphere CCM using Helm?

Deploying the [vSphere CCM Helm Chart](https://github.com/helm/charts/tree/master/incubator/vsphere-ccm/) is incredibly easy with Helm. Before you begin, it's assumed that you have created your cluster using [kubeadm](https://kubernetes.io/docs/setup/independent/install-kubeadm/) and configured your cluster to use an [external cloud provider/controller](https://kubernetes.io/docs/tasks/administer-cluster/running-cloud-controller/#running-cloud-controller-manager). If you created your Kubernetes cluster using some other mechanism besides kubeadm, the Helm Chart can be used for such deployments and will discussed in subsequent blog post.

 If you have a simple vSphere configuration with a single vCenter containing a single datacenter, deploying the vSphere CCM is extremely simple.

 Run the following Helm command substituting the vCenter credentials and name of your datacenter in the command:

 <pre>
helm install incubator/vsphere-ccm --set cfg.enabled=true --set cfg.vcenter=<vCenter IP> --set cfg.username=<vCenter Username> --set cfg.password=<vCenter Password> --set cfg.datacenter=<vCenter Datacenter>
 </pre>

 ![Easy Button](https://github.com/dvonthenen/blog/raw/master/images/easybutton.png)

Yep, it's that easy! For more complex vSphere deployments, such as having multiple vCenters and/or multiple datacenters, you can simply create your own `vsphere.conf` and Kubernetes secret `vsphereccm` in the `kube-system` namespace (to handle multple datacenters or multiple vCenters), you can learn more about the `vsphere.conf` and `vsphereccm` secret by reading the following [documentation](https://github.com/kubernetes/cloud-provider-vsphere/blob/master/docs/deploying_cloud_provider_vsphere_with_rbac.md).

Once the a configmap containing your `vsphere.conf` and a Kubernetes secret `vsphereccm` with vCenter credentials has been created, run the following command:

<pre>
helm install incubator/vsphere-ccm
</pre>

Not bad at all! Incredibly easy!

### Next Steps?

This is a pretty cool idea to have a Helm Chart that deploys an out-of-tree or external Cloud Controller Manager (CCM) to a Kubernetes cluster. I didn't get into some of the additional functionality that the chart provides like dealing with hand-rolled or non-kubeadm created clusters, but it does have the facility to deal with those configurations by either overriding the locations of various certificate files or even placing them within configmaps. Sounds like another blog post might be in order.

Give the vSphere CCM Helm Chart a test drive, let me know what you think, and if you think of any additional functionality that you might what the chart to include just drop me a line.
