

<b>Launch Ubuntu EC2 instance in AWS</b>

<b>Create and attach IAM role to EC2 Instance.</b> 

<b>Install Kops on EC2</b> \
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64 \
chmod +x kops-linux-amd64 \
sudo mv kops-linux-amd64 /usr/local/bin/kops \

<b>Install kubectl</b> \
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
chmod +x ./kubectl \
sudo mv ./kubectl /usr/local/bin/kubectl \

<b>Install awscli</b>
sudo apt install awscli \

<b>Create S3 bucket in AWS</b> \
aws s3 mb s3://javahome.in.k8s --region ap-south-1 \


<b>Create private hosted zone in AWS Route53</b> \

<b> Configure environment variables.<b> \
// export KOPS_CLUSTER_NAME=javahome.in \
export KOPS_STATE_STORE=s3://javahome.in.k8s \


<b>Create ssh key pair</b> \
ssh-keygen

<b>Create a Kubernetes cluster definition</b> \
kops create cluster \
--state=${KOPS_STATE_STORE} \
--node-count=2 \
--master-size=t2.micro \
--node-size=t2.micro \
--zones=ap-south-1a,ap-south-1b \
--name=${KOPS_CLUSTER_NAME} \
--dns public \
--master-count 1

 * list clusters with: kops get cluster
 * edit this cluster with: kops edit cluster easternspace.in
 * edit your node instance group: kops edit ig --name=easternspace.in nodes
 * edit your master instance group: kops edit ig --name=easternspace.in master-ap-south-1a

Finally configure your cluster with: kops update cluster --name easternspace.in --yes


<b>Create kubernetes cluster</b> \
kops update cluster --yes \

Suggestions:
 * validate cluster: kops validate cluster
 * list nodes: kubectl get nodes --show-labels
 * ssh to the master: ssh -i ~/.ssh/id_rsa admin@api.easternspace.in
 * the admin user is specific to Debian. If not using Debian please use the appropriate user based on your OS.
 * read about installing addons at: https://github.com/kubernetes/kops/blob/master/docs/operations/addons.md.

<b>kops validate cluster</b>

<b>To connect to the master</b> \
ssh admin@api.javahome.in


<b>Destroy the kubernetes cluster</b> \
kops delete cluster  --yes


<b> Update Nodes and Master in the cluster</b> \
kops edit ig nodes change minSize and maxSize to 0 \
   kops get ig- to get master node name \
   kops edit ig - change min and max size to 0 \
   kops update cluster --yes \
