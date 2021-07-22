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

Create deployment config using image stream. Look at the image-strem section for creating image-stream resource.
```
oc new-app <is-name>
eg oc new-app myproject/hello-world
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
oc expose --port <8080> <pod/hellow-world-pod> --name <service_name>
```
Another way to create service
```
oc create service clusterip <service-name> --tcp <srource_port:target_port>

Then change the service-yml selector section to point to point to specific deployment config

selector:
- app: 
```


Create routes
```
oc expose <svc/hello-world>
```

## ConfigMap

Create using command line
```
oc create configmap <resource-name> \
--from-literal <key>="<value>"
e.g. oc create configmap message-map \
--from-literal MESSAGE="Hello From ConfigMap"
```

Get configmap
```
oc get configmap
```

Verify the content of configmap
```
oc get -o yaml <cm/message-map>
```

Consume configmap from inside pod (using env variable)
```
oc set env <dc/hello-world> --from <cm/message-map>
```

Create configmap from file
```
- the file_name which in this case is 'MESSAGE.txt' iskey and file content is value.
> oc create configmap <name> --from-file=MESSAGE.txt
- Key other than file_name
> oc create configmap <name> --from-file=<KEY>=<FILE_NAME>
```

Create configmap from directory 
```
oc create configmap <resource_name> --from-file <dir_name>
```

Verify the content of configmap
```
oc get -o yaml <cm/message-map>
```

## ImageStream

Get the image stream
```
oc get is
```

Get image stream tag
```
oc get imagestreamtag
oc get istag
```

Create image stream
```
oc import-image --conform <image_name>
e.g. oc import-image --conform quay.io/practicalopenshift/hello-world
```

Delete image stream
```
oc delete <is/hello-world>
```

Create image stream tag
```
oc tag <original> <destination>
e.g. oc tag quay.io/image-name:tag image-name:tag
```

Push the image to the private repository
```
Get the credentials: credentials.env
> source credentials.env
> echo $REGISTRY_USERNAME (for verification)
- Docker image name should have following format
  - Hos/Repository/Image_Name:tag
  - quay.io/practialopenshift/private-repo:tag
> docker login <quay.io>
> docker push <image_name>

```

Create docker registory secret to pull the image from private image, otherwise creating new app (deployment config) will fail
```
oc create secret docker-registory \
  <name e.g. demo-image-pull-secret> \
  --docker-server=$REGISTRY_HOST \
  --docker-username=$REGISTRY_USERNAME \
  --docker-password=$REGISTRY_PASSWORD \
  --docker-email=$REGISTRY_EMAIL
```

Link the service account with registory-secret to be able to pull the image. 
```
oc secret link <service_account> <secret_name> --for=pull
e.g. oc secret link default demo-image-pull-secret -for=pull
- The command above will use the service account named 'default' for the porject
```

To check if the secret is linked with serviceaccount
```
oc describe serviceaccount/default
```

V
```
oc 
```
