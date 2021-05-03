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

## DeploymentConfig

Create a deployment config
```
oc new-app <image-tag> --name <lable-name>
e.g oc new-app quay.io/praticalopenshift/hello-world --name demo-app
```

All the resources currently in the project
```
oc status
```

List deployment config
```
oc get dc
```

Get the deployment config defination file (in yaml format)
```
oc get -o yaml <dc/hello-world>
```

Delete deployment config. There is better way to delete, look at the next command.
```
oc delete <dc-full-name>
Use the name returned by: oc status
e.g. oc delete dc/hello-world
```

Delete multiple type of resources using one command (lable selector)
```
Run the following command to get the labels
> oc describe <dc/hello-world> 
Use the labels to delete resources
> oc delete all -l <app=hellow-world>
```

Create deployment config using git repository
```
oc new-app <git-repository>
```

Track the progress of the build. Output from build is very similar to docker build output.
```
oc logs -f <bc/hello-world>
```

Get the replication controller
```
oc get rc
```

Roolout: deploy newer version of application
- First deploye new pod
- Terminate the old deployment
```
oc rollout <tag> <dc/hello-world>
```

Roolback: Deploy previous version of application
```
- Rollback one version
> oc roolback <dc/hellow-world>
```

## Services & Routes

Create service
```
oc expose --port <8080> <pod/hellow-world-pod>
```

Create routes
```
oc expose <svc/hello-world>
```

