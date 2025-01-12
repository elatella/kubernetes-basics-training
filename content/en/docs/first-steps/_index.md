---
title: "First steps"
weight: 2
---

In this lab, we will interact with the {{% param distroName %}} cluster for the first time.

{{% onlyWhenNot nosetup %}}
{{% alert title="Warning" color="warning" %}}
Please make sure you completed {{<link "setup">}} before you continue with this lab.
{{% /alert %}}
{{% /onlyWhenNot %}}
{{% onlyWhenNot openshift %}}


## Login

{{% alert title="Note" color="info" %}}
Authentication depends on the specific Kubernetes cluster environment.

You may need special instructions if you are not using our lab environment.
{{% /alert %}}

{{% onlyWhen rancher %}}
{{% onlyWhen cloudscale %}}

Our Kubernetes cluster of the lab environment runs on [cloudscale.ch](https://cloudscale.ch) (a Swiss IaaS provider) and has been provisioned with [Rancher](https://rancher.com/). You can log in to the cluster with a Rancher user.

{{% alert title="Note" color="info" %}}
Your trainer will provide you with the information needed to log in.
{{% /alert %}}
{{% /onlyWhen %}}

Log in to the Rancher web console and choose the desired cluster.

You now see a button at the top right that says **Kubeconfig File**. Click it, scroll down to the bottom and click **Copy to Clipboard**.

{{< imgproc kubectlconfigfilebutton.png Resize  "800x" >}}{{< /imgproc >}}

The copied kubeconfig now needs to be put into a file. The default location for the kubeconfig file is `~/.kube/config`.

{{% alert title="Note" color="info" %}}
If you already have a kubeconfig file, you might need to merge the Rancher entries with yours or use a dedicated file as described below.
{{% /alert %}}

Put the copied content into a kubeconfig file on your system.
If you decide to not use the default kubeconfig location at `~/.kube/config` then let `kubectl` know where you put it with the KUBECONFIG environment variable:

```
export KUBECONFIG=$KUBECONFIG:~/.kube-techlab/config
```

{{% alert title="Note" color="info" %}}
When using PowerShell on a Windows Computer use the following command. You'll have to replace `<user>` with your actual user

```
$Env:KUBECONFIG = "C:\Users\<user>\.kube-techlab\config"
```

To set the environment variable (`KUBECONFIG` = `C:\Users\<user>\.kube-techlab\config`) permanently, check the following documentation:

The `PATH` can be set in Windows in the advanced system settings. It depends on the version:

* [Windows 7](http://geekswithblogs.net/renso/archive/2009/10/21/how-to-set-the-windows-path-in-windows-7.aspx)
* [Windows 8](http://www.itechtics.com/customize-windows-environment-variables/)
* [Windows 10](http://techmixx.de/windows-10-umgebungsvariablen-bearbeiten/)

{{% /alert %}}

{{% /onlyWhen %}}

{{% onlyWhen mobi %}}
We are using the Mobi `kubedev` Kubernetes cluster. Use the following command to set the appropriate context:

```bash
kubectl config use-context kubedev
```

{{% alert title="Warning" color="warning" %}}
Make sure you have set up your kubeconfig file correctly. Check your [CWIKI](https://cwiki.mobicorp.ch/confluence/display/ITContSol/Set+up+Kubectl) for instructions on how to configure it.
{{% /alert %}}
{{% /onlyWhen %}}


## Namespaces

As a first step on the cluster, we are going to create a new Namespace.

A Namespace is a logical design used in Kubernetes to organize and separate your applications, Deployments, Pods, Ingresses, Services, etc. on a top-level basis. Take a look at the [Kubernetes docs](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/). Authorized users inside a namespace are able to manage those resources. Namespace names have to be unique in your cluster.

{{% onlyWhen rancher %}}
{{% alert title="Note" color="info" %}}
Additionally, Rancher knows the concept of a [_Project_](https://rancher.com/docs/rancher/v2.x/en/cluster-admin/projects-and-namespaces/) which encapsulates multiple Namespaces.
{{% /alert %}}

In the Rancher web console choose the Project called `Training`.

{{% onlyWhen mobi %}}
We use the project `kubernetes-techlab` on the `kubedev` cluster.
{{% /onlyWhen %}}

{{% /onlyWhen %}}


### {{% task %}} Create a Namespace

Create a new namespace in the lab environment. The `kubectl help` output can help you figure out the right command.

{{% alert title="Note" color="info" %}}
Please choose an identifying name for your Namespace, e.g. your initials or name as a prefix.

We are going to use `<namespace>` as a placeholder for your created Namespace.
{{% /alert %}}


### Solution

To create a new Namespace on your cluster use the following command:

```bash
kubectl create namespace <namespace>
```

{{% onlyWhen rancher %}}
{{% alert title="Note" color="info" %}}
Namespaces created via `kubectl` have to be assigned to the correct Rancher Project in order to be visible in the Rancher web console. Please ask your trainer for this assignment or you can create the Namespace directly within the Rancher web console.
{{% /alert %}}
{{% /onlyWhen %}}

{{% alert title="Note" color="info" %}}
By using the following command, you can switch into another Namespace instead of specifying it for each `kubectl` command.

Linux:

```bash
kubectl config set-context $(kubectl config current-context) --namespace <namespace>
```

Windows:

```bash
kubectl config current-context
SET KUBE_CONTEXT=[Insert output of the upper command]
kubectl config set-context %KUBE_CONTEXT% --namespace <namespace>
```

Some prefer to explicitly select the Namespace for each `kubectl` command by adding `--namespace <namespace>` or `-n <namespace>`. Others prefer helper tools like `kubens` (see {{<link "05_kubernetes">}}).
{{% /alert %}}

{{% onlyWhen rancher %}}


## {{% task %}} Discover the Rancher web console

Check the menu entries, there should neither be any Deployments nor any Pods or Services in your Namespace.

Display all existing Pods in the previously created Namespace with `kubectl` (there shouldn't yet be any):

```bash
kubectl get pod -n <namespace>
```

With the command `kubectl get` you can display all kinds of resources.
{{% /onlyWhen %}}
{{% /onlyWhenNot %}}
{{% onlyWhen openshift %}}


## Projects

As a first step on the cluster, we are going to create a new Project.

A Project is a logical design used in OpenShift to organize and separate your applications, Deployments, Pods, Ingresses, Services, etc. on a top-level basis.
Authorized users inside a Project are able to manage those resources. Project names have to be unique in your cluster.


### {{% task %}} Create a Project

Create a new Project in the lab environment. The `oc help` output can help you figure out the right command.

{{% alert title="Note" color="info" %}}
Please choose an identifying name for your Project, e.g. your initials or name as a prefix. We are going to use `<project>` as a placeholder for your created Project.
{{% /alert %}}


### Solution

To create a new Project on your cluster use the following command:

```bash
oc new-project <project>
```

{{% alert title="Note" color="info" %}}
Some prefer to explicitly select the Project for each `oc` command by adding `--namespace <project>` or `-n <project>`.

By using the following command, you can switch into another Project instead of specifying it for each `oc` command.

```bash
oc project <project>
```

{{% /alert %}}


## {{% task %}} Discover the OpenShift web console

Discover the different menu entries in the two views, the **Developer** and the **Administrator** view.

Display all existing Pods in the previously created Project with `oc` (there shouldn't yet be any):

```bash
oc get pod --namespace <project>
```

{{% alert title="Note" color="info" %}}
With the command `oc get` you can display all kinds of resources.
{{% /alert %}}
{{% /onlyWhen %}}
