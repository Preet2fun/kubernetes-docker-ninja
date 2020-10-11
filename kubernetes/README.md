# kubernetes-docker-ninja

#check the version of the cluster that you are running:
$ kubectl version

#simple diagnostic for the cluster. This is a good way to verify that your cluster is generally healthy:
$ kubectl get componentstatuses

#you can list out all of the nodes in your cluster:
$ kubectl get nodes

#You can use the kubectl describe command to get more information about a specific node, such as node-1:
$ kubectl describe nodes node-1

#If your cluster runs the Kubernetes proxy with a DaemonSet, you can see the proxies by running:
$ kubectl get daemonSets --namespace=kube-system kube-proxy

#The DNS service is run as a Kubernetes deployment, which manages these replicas:
$ kubectl get deployments --namespace=kube-system coredns

#There is also a Kubernetes service that performs load balancing for the DNS server:
$ kubectl get services --namespace=kube-system coredns

#If you want to see what the command will do without actually making the apply
#changes, you can use the flag to print the objects to the terminal without --dry-run actually sending them to the server.
kubectl apply -f namespace_obj.yaml --dry-run

#to add the color=red label to a Pod named bar, you can run:
$ kubectl label pods bar color=red

#You can use the following to see the logs for a running container:
$ kubectl logs <pod-name>

#You can also use the exec command to execute a command in a running container:
$ kubectl exec -it <pod-name> -- bash

#You can also copy files to and from a container using the cp command:
$ kubectl cp <pod-name>:</path/to/remote/file> </path/to/local/file>
exp : kubectl cp nginx:/tmp/abc.txt /root/kubernetes/def.txt

#you can use the top command to see the list of resources in use by either nodes or Pods
$ kubectl top nodes
$ kubectl top nodes



######################################### POD ##################################################

#The simplest way to create a Pod is via the imperative kubectl run command
$ kubectl run pod-manu --generator=run-pod/v1 --image=preet2fun/simple-node:v1
$ kubectl run alpaca-prod \
--image=gcr.io/kuar-demo/kuard-amd64:blue \
--labels="ver=1,app=alpaca,env=prod"

# belwo command will show labels along with its basic information
kubectl get pods --show-labels
kubectl get pods --selector="app=bandicoot,ver=2"
kubectl get pods --selector="app in (alpaca,bandicoot)"


# to add label to specific pod after its creation use below command
kubectl label pod alpha-prod "color=red"

#You can remove a label by applying a dash suffix:
$ kubectl label pod alpaca-prod "color-"

# to take configuration of current running pod in yaml file for edit it use below command and then edit that file using VIM editor
kubectl get pod resource-example -o yaml >> resource-request-limits-pod.yaml


########################################## Deployments ###########################################

#This is the simplest way to create deployment via imperative kubectl run command
# to get specific parameter using jsonpath we can use belwo command
kubectl get deployment <name_of_deployment> -o jsonpath --template {.spec.selector.matchLabels}

#for download of any running deployment into a YAML file use belwo command:
kubectl get deployments kuard --export -o yaml > kuard-deployment.yaml
kubectl replace -f kuard-deployment.yaml --save-config

# Below are the diffrent command for deployment rollout
# Rollback to the previous deployment
kubectl rollout undo deployment/abc
  
# Check the rollout status of a daemonset
kubectl rollout status daemonset/foo
kubectl rollout history deployment/kuard

Available Commands:
  history     View rollout history
  pause       Mark the provided resource as paused
  restart     Restart a resource
  resume      Resume a paused resource
  status      Show the status of the rollout
  undo        Undo a previous rollout

#If you are in the middle of a rollout and you want to temporarily pause it for some
#reason (e.g., if you start seeing weird behavior in your system and you want to investigate),
#you can use the pause command:
kubectl rollout pause deployments kuard
deployment "kuard" paused

#If, after investigation, you believe the rollout can safely proceed, you can use the
#resume command to start up where you left off:
kubectl rollout resume deployments kuard
deployment "kuard" resumed

#If you are interested in more details about a particular revision, you can add the
# --revision flag to view details about that specific revision:
kubectl rollout history deployment kuard --revision=2


#you can roll back to a specific revision in the history using the flag: --to-revision
kubectl rollout undo deployments kuard --to-revision=3
deployment "kuard" rolled back

#If you ever want to delete a deployment, you can do it either with the imperative command:
kubectl delete deployments kuard

#or using the declarative YAML file we created earlier:
kubectl delete -f kuard-deployment.yaml

########################################## ReplicaSets ###########################################
#imperative commands for scale up/down of replicaset
kubectl scale replicaset <replicaset-name> --replicase=3

#Scaling based on CPU usage is the most common use case for Pod autoscaling and you can use below commad for same
kubectl autoscale rs kuard --min=2 --max=5 --cpu-percent=80

# to see horizontal pod autoscaling(hpa) use below commnd
kubectl get hpa

#If you don’t want to delete the Pods that are being managed by the ReplicaSet, you
#can set the --cascade flag to false to ensure only the ReplicaSet object is deleted and not the Pods:
kubectl delete rs kuard --cascade=false

######################################### ConfigMaps and Secerets ###############################
# below commnd will create configmaps imperative way,
kubectl create configmap my-config --from-file=my-config.txt --from-literal=extra-param=extra-value --from-literal=another-param=another-value

#Create a generic secret named kuard-tls using the create secret command, here kuard.crt and kuard.key are pre generated certificates
$ kubectl create secret generic kuard-tls \
--from-file=kuard.crt \
--from-file=kuard.key

# Use the create secret docker-registry to create this special kind of secret:
$ kubectl create secret docker-registry my-image-pull-secret \
--docker-username=<username> \
--docker-password=<password> \
--docker-email=<email-address>
