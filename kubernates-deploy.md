<b>Deploying Nginx container on Kubernetes<b>


<b>Deploying Nginx Container</b>

  kubectl run sample-nginx --image=nginx --replicas=2 --port=80 \
  kubectl get pods \
  kubectl get deployments
  
  
<b>Expose the deployment as service. This will create an ELB in front of those 2 containers and allow us to publicly access them:<b>

 kubectl expose deployment sample-nginx --port=80 --type=LoadBalancer \
 kubectl get services -o wide
