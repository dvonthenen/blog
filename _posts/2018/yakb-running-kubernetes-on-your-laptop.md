---
ID: 120
post_title: 'YAKB: Running Kubernetes on Your Laptop'
post_name: yakb-running-kubernetes-on-your-laptop
author: David vonThenen
post_date: 2018-10-22 12:52:24
layout: post
link: >
  https://dvonthenen.com/2018/10/22/yakb-running-kubernetes-on-your-laptop/
published: true
tags:
  - demonstration
  - dev/test
  - development
  - how-to
  - Kubernetes
  - minikube
categories:
  - Kubernetes
---
<p>Hi there! Yes, it's been a while since I have posted to my personal blog. Before I get to my post, I thought I would just bring you up to speed with what's been going on in my world since my last post. So I no longer work at Dell EMC. I also no longer work at Dell Technologies. After transfer upon transfer, I currently work at VMware. The change has been going good so far especially moving from companies that are traditionally hardware based to a company that is mostly software focused. While I have only been at VMware since March of this year, the momentum in the Kubernetes and CNCF communities and VMware's commitment to those communities has made VMware an obvious choice going forward. Which leads me to this blog post...</p>

<h2>Let's get to the Blog!</h2>

<p>So I am writing this "Yet Another Kubernetes Blog" post because I needed to document how someone can run Kubernetes on their laptop so that fellow future session attendees can follow along with future presentations that I might give. If this never gets used in one of my presentations, that's cool... but I thought, hey why not just put this out here so that others might benefit from this. You could potentially use this blog post to just test drive Kubernetes and play around with its functionality.</p>

<p><em>NOTE:</em> If you are running on Windows, well you might want to install something like VMware Workstation Pro so that you can get a RHEL7 VM running on your laptop. I believe you can try it for 30 days.</p>

<h3>Installing Virtual Box</h3>

<p>To simplify the installation among the two platforms, we are going to need to install Virtual Box (5.2 is recommended). To do that visit the <a href="https://www.virtualbox.org/wiki/Downloads">Virtual Box Homepage</a>, then download the installation package based on your platform.</p>

<p><em>On MAC</em>, just download the DMG file and install Virtual Box like any other application on your MAC.</p>

<p><em>On RHEL7</em>, download the appropriate RPM and then install using the following command:</p>

<pre>sudo rpm -ivh 
</pre>

<h3>Installing kubectl</h3>

<p>Next, we need to install <code>kubectl</code> which is the Kubernetes command line tool to manage a Kubernetes cluster. You will be using this utility for the majority (if not all) of the operations to view what's going on in your cluster to kicking off applications in your cluster. Fortunately, installation of this component is pretty straightforward.</p>

<p><em>On MAC</em>, you can run the following command:</p>

<pre>curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
</pre>

<p><em>On RHEL7</em>, you can run the following command:</p>

<pre>curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
</pre>

<p>You can verify that kubectl is installed correctly by running the following command: <code>kubectl help</code>. Let's move onto the last component <code>minikube</code>.</p>

<h3>Installing minikube</h3>

<p>So <code>minikube</code> is a simple tool that allows you to quickly deploy a single node Kubernetes cluster on your laptop/computer. We are going to be using that for demonstration purposes and to kick the tires on Kubernetes. You can install <code>minikube</code> by doing the following:</p>

<p><em>On MAC</em>, you can run the following commands:</p>

<pre>curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

minikube config set memory 4096
minikube config set cpus 2
</pre>

<p><em>On RHEL7</em>, you can run the following commands:</p>

<pre>curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

minikube config set memory 4096
minikube config set cpus 2
</pre>

<p><em>RHEL7 NOTE:</em> If you see the following error (I did not during my install, but it has been reported to happen sometimes):</p>

<pre>Could not read CA certificate "/etc/docker/ca.pem": open /etc/docker/ca.pem: no such file or directory
</pre>

<p>The fix is to update /etc/sysconfig/docker to ensure that minikube’s environment changes are respected:</p>

<pre>&lt; DOCKER_CERT_PATH=/etc/docker
---
&gt; if [ -z "${DOCKER_CERT_PATH}" ]; then
&gt;   DOCKER_CERT_PATH=/etc/docker
&gt; fi
</pre>

<p>You can verify that <code>minikube</code> is installed correctly by running the following command: <code>minikube version</code>. Pretty simple!</p>

<h3>I want a Kubernetes!</h3>

<p>So now that you have all the associated software installed on your laptop, let's bring up a Kubernetes cluster!</p>

<p><em>On MAC and RHEL7</em>, run the following command:</p>

<pre>minikube start
</pre>

<p>To make sure you can access your Kubernetes cluster, run the following command:</p>

<pre>kubectl get pods --all-namespaces
</pre>

<p>To stop and delete your cluster, run the following command:</p>

<pre>minikube stop
</pre>

<h2>Conclusion</h2>

<p>Well, there it is! A simple way to get a Kubernetes cluster running on your laptop without requiring a public cloud account or a ton of hardware sitting in a lab or datacenter somewhere. This by far isn't a magic bullet and it only good for running small light-weight applications. If anything, it's a simple way to familiarize yourself with Kubernetes and general management of the cluster.</p>

<p>I plan on posting to my blog more often now... so stay tuned on some cool future blog posts! If you have any suggestions on topics you want to hear more on, please let me know! I am interested in everything from intro-101-type blogs to deep-dives. Just drop me a line!</p>
