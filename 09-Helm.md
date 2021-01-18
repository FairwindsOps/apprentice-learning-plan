# Part IX: Helm

Helm is a “package manager” for Kuberentes. It allows a simple, single command, to install and configure various software in a cluster. It also keeps track of ‘releases’ and allows you to roll back easily to any release. Helm exists in to parts, the `helm`  binary running from your workstation and the `tiller`, running in the cluster and interacting with the Kubernetes api. This creates an interesting security challenge that we’ll discuss later. This curriculum will walk you through configuration and use of Helm in a Kubernetes cluster and touch on chart creation. 

**PreRequisites**: 

- General understanding of command line use including `kubectl`. 
- Specific understanding of Kubernetes primitives and yaml
- General understanding of templateing
- Specific knowledge of Golang Templating is helpful but not required

**Definitions**:

- Chart: A set of templates configured to deploy an application into Kubernetes.
- Release: The instance of the created application. Each `helm upgrade` or `helm install` command creates a new release
- Values: The values used to configure the templated yaml. Most charts have sensible defaults and a few required values. Values can be passed in on the command line or through a yaml file(s)
- Helm: The local command
- Tiller: The in-cluster server component

**Preparation:** 

1.  Setup Kubernetes using Kind, Minikube, k3s.io, or GKE:
    1. https://github.com/kubernetes-sigs/kind
    2. https://kubernetes.io/docs/tasks/tools/install-minikube/
    3. https://cloud.google.com/kubernetes-engine/docs/how-to/creating-a-cluster
    4. https://k3s.io/
2. Install Helm on your workstation
    1. https://helm.sh/docs/using_helm/#installing-helm
3. If RBAC is enabled in your cluster (which it should be), create a Role and RoleBinding for the Tiller.
    1. Create a Namespace for Tiller: `kubectl create namespace helm-system`
    2. Create a ServiceAccount for Tiller: `kubectl -n helm-system create sa tiller`
    3. Bind `cluster-admin` cluster role to `tiller` ServiceAccount:  `kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=tiller:tiller`
4. Initialize Tiller  `helm init --upgrade --tiller-namespace helm-system --service-account tiller`
5. Set Environment `export TILLER_NAMESPACE=helm-system`

**Working with Helm:**

1. Create a release: `helm install stable/redis`
    1. This will install the basic Redis chart from the stable repository with a random name
2. List releases: `helm list`
    1. What name does your release have? 
    2. What is the `STATUS`
3. Delete release: `helm delete <release-name>`
4. Create a new named release: `helm install stable/redis` `--``name redis`
    1. What namespace did it install into? 
    2. How would you install it into a different namespace?
    3. List the pods 
5. Delete release
6. Create the same release again
    1. Why did it fail?
    2. Do you see any releases? 
    3. Helm stores a base64 encoded tar of each release in a ConfigMap in the namespace that Tiller is running in. Find it. `kubectl get -n tiller configmaps` 
    4. You will probably see two ConfigMaps, one that is from the randomly named release and one that is `redis.v1`
    5. You could delete the individual ConfigMap manually but as the number of releases grows, that will become painful. Instead you can use the `--purge` flag
    6. `helm delete redis` `--``purge` will delete all ConfigMaps for the release.
    7. FUN FACT: If your first release fails, you will always have to delete the release with purge. You can never ‘fix’ a release that is failed on its first install. 
7. What if you want to configure the chart outside other than the defaults?
    1. Use `helm inspect stable/redis` to see the templates and parameters and configuration information.  Sometimes that is overwhelming so commonly, you can use the [documentation](https://github.com/helm/charts/tree/master/stable/redis).  
    2. Lets install the chart again, but with `rbac.create` turned on. This will configure the chart to create rbac roles and bindings for the redis pods. 
        1. `helm install stable/redis --name=redis` `--``set rbac.create=true`
        2. Lets also enable the metrics endpoint. `helm upgrade redis stable/redis` `--``set rbac.create=true` `--``set metrics.enabled=true`
            1. Notice `upgrade` instead of `install`. Upgrade modifies an existing release, install creates a new one. You can do both in one command with `helm upgrade` `--``install`
            2. run `helm upgrade` `--``help` to see more options. How could the above command be different if you used  `--reuse-values`. Do you think you’ll need to user `--recreate-pods` for the Pods to get the new settings?
        3. You can also use a yaml file to pass values into the `helm` command with the `--values` flag. 
8. `helm init` stores the downloaded charts to `~/.helm/` or to the value of `$HELM_HOME`. You can see the charts you have downloaded locally by viewing that location
9. View the redis chart there or on github and open `/templates/redis-master-statefuleset.yml`. This is the Golang template language. The exact syntax is not important but you’ll notice that template blocks are wrapped in `{{ }}`.  Those that start with `template` are found in `/templates/helpers.tpl` otherwise the are in they used directly as `--set` or from the `--values` file.  You can see the default values file at `/values.yml`. 
10. Create your own chart: `helm create new-chart`
    1. Open the chart and see what `templates` are created for you and what values are crated for you. 
        1. What app will this chart deploy by default?
        2. Will it deploy successfully as is? Try. `helm upgrade` `--``install new-chart` Notice that because the chart is not coming from a repository like `stable/redis` you just provide the path to the chart. 
    2. Pick a simple app you’d like to install and convert this chart to deploy it. 
        1. Make sure to use the `templates`  in `new-chart/templates/helpers.tpl` for consistency
        2. What happens if you need to add a new file to `new-chart/templates` ? Will helm automatically deploy it? Try.
        3. How is `new-chart/templates/Notes.txt` used? Make sure to update it when you convert this chart to your chose app.
    3. View `new-chart/Chart.yml` to view the metadata about your `new-chart`
    4. Use a `--``values` file to `upgrade` the release of this chart. 



    


