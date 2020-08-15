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
kubectl run pod-manu --generator=run-pod/v1 --image=preet2fun/simple-node:v1


