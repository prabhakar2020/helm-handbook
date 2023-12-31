# Microk8s - Kubectl - HelmCharts quick reference material
##### Microk8s
MicroK8s is a lightweight, easy-to-install Kubernetes distribution designed for development, testing, and deployment of Kubernetes applications. It is a single-node Kubernetes cluster that can be run on a local machine or a virtual machine. MicroK8s is part of the Kubernetes ecosystem and is maintained by Canonical, the company behind Ubuntu.
Microk8s Installation instructions - https://microk8s.io/
1. Install MicroK8s on Linux
    ```sudo snap install microk8s --classic```
1. Check the status while Kubernetes starts
    ```microk8s status --wait-ready```
    It may prompt uas to assign permissions
    ```
        sudo usermod -a -G microk8s $USER
        sudo chown -R $USER ~/.kube
        newgrp microk8s
    ```
1. Turn on the services you want
    ```microk8s enable dashboard
       microk8s enable dns
       microk8s enable registry
       microk8s enable istio
   ```
    Try microk8s enable --help for a list of available services and optional features. microk8s disable <name> turns off a service.
1. Install Kubernetes CTL - https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
    ```
    curl -LO https://dl.k8s.io/release/v1.28.3/bin/linux/amd64/kubectl
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    ```
1. Verify kubectl version
    ``` kubectl version ```
1. Configure Kubectl on the host
    ```
    cd $HOME
    mkdir .kube
    cd .kube
    microk8s config > config
    ```
`Try to restart machine so that Kubernetes will work properly`
1. Start using Kubernetes
    ```microk8s kubectl get all --all-namespaces```
    ```kubectl get all --all-namespaces```
1. Access the Kubernetes dashboard
    ```microk8s dashboard-proxy```
1. Start and stop Kubernetes to save battery
    Kubernetes is a collection of system services that talk to each other all the time. If you don't need them running in the background then you will save battery by stopping them. **microk8s start** and **microk8s stop** will do the work for you.

##### HelmCharts
A Helm chart is a package of pre-configured Kubernetes resources, designed to deploy and manage applications in a Kubernetes cluster. Helm is a package manager for Kubernetes that simplifies the process of deploying and managing applications. Helm charts encapsulate all the necessary Kubernetes YAML files, templates, and configuration values needed to deploy an application or service.
###### Install Helm
```
curl -L https://git.io/get_helm.sh | bash -s -- --version v3.8.2
```

Here are key components and concepts related to Helm charts:

###### Chart:

A Helm chart is a collection of files organized into a specific directory structure.
The chart contains YAML files that define Kubernetes resources such as Deployments, Services, ConfigMaps, and more.
It may include templates that allow parameterization and customization of resource configurations.
###### Values:

Helm charts use a values.yaml file to define default configuration values.
Users can override these default values by providing their own values.yaml file or by specifying values on the command line during chart installation or upgrade.
###### Template Engine:

Helm uses a template engine (Go templates) to allow dynamic generation of Kubernetes manifest files.
Templates enable parameterization of resource configurations, making charts flexible and reusable.

##### Create helm chart
``` helm create flask-rest-api ```

##### Install helm chart
``` helm install flask-app flask-rest-api ```
##### Delete helm chart
``` helm delete flask-app ```
##### List helm charts (already deployed)
``` helm list -a ```

`` 
Note: Change values.yaml Service->Type from ClusterIP to NodePort for better testing on your local machine. 
Use your local machine/ VM IP for testing instead of directly using service IP
``
##### Helm upgrade deployment
``` 
syntax: helm upgrade <customName> <nameOfChart>
example: helm upgrade flaskapp flask-rest-api
```
##### Helm rollback deployment
``` 
syntax: helm rollback <customName> <revisionID>
example: helm rollback flaskapp 1
```
##### Helm Lint
``` 
syntax: helm lint <helmChartName>
example: helm lint flask-rest-api
```

##### Helm dry-run
``` 
syntax: helm install <customName> --debug --dry-run <helmChartName>
example: helm install flaskapp --debug --dry-run flask-rest-api
```

##### Helm render templates locally in (deployment, service etc in one output)
``` 
syntax: helm template <helmChartName>
example: helm template flask-rest-api
```



