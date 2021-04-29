# OpenShift cheat sheet

## Access built-in documentation

```
oc explain pod
oc explain pod.spec
oc.explain pod.spec.containers
```

## Log in via CLI

```
oc login <cluster_ip>:<port>
```

## Verify if Openshift is running

```
oc status
```

## Project management

Current project

```
oc project
```

List all projects

```
oc projects
```

Switch to different project

```
oc project <diff-project>
```

Create new project

```
oc new-project <demo-project>
```

## Pods

Create a pod from file
```
oc create -f <pod.yaml>
```

Get all the pods
```
oc get pods
```

Watch the pod lifecycle
```
oc get pods --watch
```

Start shell inside container
```
oc rsh <pod-name>
```

Delete pod
```
oc delete pod <pod_name>
```
