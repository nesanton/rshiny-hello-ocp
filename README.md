# rshiny-hello-ocp

A simple PoC for an R-studio/Shiny hello world application running in OpenShift.

> for simplicity I'm using rocker/shiny container image. This image is not OpenShift friendly - it can't run from any user. See below instructions to solve it. For any serious use you are advised to assess image supply chain and consider creating your own image. 

## Usage

* make a new namespace

```bash
oc new-project hello-shiny
```

Apply this template

```bash
oc process -f | oc apply -f -
```

Allow the image to run from the hardcoded user (need admin privileges in the cluster). Pods in this namespace use the `default` service account. If you don't plan to run any more pods here, just allow the `default` to use `anyuid` scc:

```bash
oc adm policy add-scc-to-user anyuid -z default
# assuming there's only one pod:
oc delete pod $(oc get pod --template '{{(index .items 0).metadata.name}}')
```

Get the route and paste it into your browser:

```bash
oc get route rshiny-hello
```