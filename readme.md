ranjiniganeshan@Ranjinis-MacBook-Pro devsecops % 
```
eksctl utils associate-iam-oidc-provider --cluster dev-secops-cluster --approve
```

2024-10-19 19:18:10 [ℹ]  IAM Open ID Connect provider is already associated with cluster "dev-secops-cluster" in "us-west-2"
ranjiniganeshan@Ranjinis-MacBook-Pro devsecops % 
```
eksctl create iamserviceaccount \
  --name ebs-csi-controller-sa\
  --namespace kube-system \
  --cluster $cluster_name \
  --role-name AmazonEKS_EBS_CSI_DriverRole \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve --role-only 

```
2024-10-19 19:18:40 [ℹ]  1 existing iamserviceaccount(s) (kube-system/ebs-csi-controller-sa) will be excluded
2024-10-19 19:18:40 [ℹ]  1 iamserviceaccount (kube-system/ebs-csi-controller-sa) was excluded (based on the include/exclude rules)
2024-10-19 19:18:40 [!]  serviceaccounts in Kubernetes will not be created or modified, since the option --role-only is used
2024-10-19 19:18:40 [ℹ]  no tasks
ranjiniganeshan@Ranjinis-MacBook-Pro devsecops % 
```
eksctl create iamserviceaccount \
  --name ebs-csi-controller-sa\
  --namespace kube-system \
  --cluster $cluster_name \
  --role-name AmazonEKS_EBS_CSI_DriverRole \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve --role-only
```
2024-10-19 19:20:20 [ℹ]  1 iamserviceaccount (kube-system/ebs-csi-controller-sa) was included (based on the include/exclude rules)
2024-10-19 19:20:20 [!]  serviceaccounts in Kubernetes will not be created or modified, since the option --role-only is used
2024-10-19 19:20:20 [ℹ]  1 task: { create IAM role for serviceaccount "kube-system/ebs-csi-controller-sa" }
2024-10-19 19:20:20 [ℹ]  building iamserviceaccount stack "eksctl-dev-secops-cluster-addon-iamserviceaccount-kube-system-ebs-csi-controller-sa"
2024-10-19 19:20:21 [ℹ]  deploying stack "eksctl-dev-secops-cluster-addon-iamserviceaccount-kube-system-ebs-csi-controller-sa"
2024-10-19 19:20:21 [ℹ]  waiting for CloudFormation stack "eksctl-dev-secops-cluster-addon-iamserviceaccount-kube-system-ebs-csi-controller-sa"
2024-10-19 19:20:52 [ℹ]  waiting for CloudFormation stack "eksctl-dev-secops-cluster-addon-iamserviceaccount-kube-system-ebs-csi-controller-sa"

ranjiniganeshan@Ranjinis-MacBook-Pro devsecops % 
```
export cluster_name="dev-secops-cluster" 

```
ranjiniganeshan@Ranjinis-MacBook-Pro devsecops % 
```
eksctl create iamserviceaccount \        
  --name ebs-csi-controller-sa\
  --namespace kube-system \
  --cluster $cluster_name \
  --role-name AmazonEKS_EBS_CSI_DriverRole \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve --role-only
```
2024-10-19 19:22:53 [ℹ]  1 iamserviceaccount (kube-system/ebs-csi-controller-sa) was included (based on the include/exclude rules)
2024-10-19 19:22:53 [!]  serviceaccounts in Kubernetes will not be created or modified, since the option --role-only is used
2024-10-19 19:22:53 [ℹ]  1 task: { create IAM role for serviceaccount "kube-system/ebs-csi-controller-sa" }
2024-10-19 19:22:53 [ℹ]  building iamserviceaccount stack "eksctl-dev-secops-cluster-addon-iamserviceaccount-kube-system-ebs-csi-controller-sa"
2024-10-19 19:22:54 [ℹ]  deploying stack "eksctl-dev-secops-cluster-addon-iamserviceaccount-kube-system-ebs-csi-controller-sa"
2024-10-19 19:22:54 [ℹ]  waiting for CloudFormation stack "eksctl-dev-secops-cluster-addon-iamserviceaccount-kube-system-ebs-csi-controller-sa"
2024-10-19 19:23:26 [ℹ]  waiting for CloudFormation stack "eksctl-dev-secops-cluster-addon-iamserviceaccount-kube-system-ebs-csi-controller-sa"
ranjiniganeshan@Ranjinis-MacBook-Pro devsecops % eksctl create addon --name aws-ebs-csi-driver --cluster $cluster_name --service-account-role-arn arn:aws:iam::909293070315:role/AmazonEKS_EBS_CSI_DriverRole --force
2024-10-19 19:24:09 [ℹ]  Kubernetes version "1.28" in use by cluster "dev-secops-cluster"
2024-10-19 19:24:09 [ℹ]  using provided ServiceAccountRoleARN "arn:aws:iam::909293070315:role/AmazonEKS_EBS_CSI_DriverRole"
2024-10-19 19:24:09 [ℹ]  creating addon
ranjiniganeshan@Ranjinis-MacBook-Pro devsecops % 
```
k create namespace ci
namespace/ci created
```
ranjiniganeshan@Ranjinis-MacBook-Pro devsecops % 
```
helm install --namespace ci --values jenkins.values.yaml jenkins jenkins/jenkins

```

NAME: jenkins
LAST DEPLOYED: Sat Oct 19 19:26:30 2024
NAMESPACE: ci
STATUS: deployed
REVISION: 1
NOTES:
1. Get your 'admin' user password by running:
  kubectl exec --namespace ci -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo
2. Get the Jenkins URL to visit by running these commands in the same shell:
  export NODE_PORT=$(kubectl get --namespace ci -o jsonpath="{.spec.ports[0].nodePort}" services jenkins)
  export NODE_IP=$(kubectl get nodes --namespace ci -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT

3. Login with the password from step 1 and the username: admin
4. Configure security realm and authorization strategy
5. Use Jenkins Configuration as Code by specifying configScripts in your values.yaml file, see documentation: http://$NODE_IP:$NODE_PORT/configuration-as-code and examples: https://github.com/jenkinsci/configuration-as-code-plugin/tree/master/demos

For more information on running Jenkins on Kubernetes, visit:
https://cloud.google.com/solutions/jenkins-on-container-engine

For more information about Jenkins Configuration as Code, visit:
https://jenkins.io/projects/jcasc/


NOTE: Consider using a custom image with pre-installed plugins
ranjiniganeshan@Ranjinis-MacBook-Pro devsecops % 
```
k get pvc -n ci
NAME      STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
jenkins   Bound    pvc-0a52149f-411e-4f9a-a1f8-ad2d0070fbbc   8Gi        RWO            gp2            11s

```

kubectl exec --namespace ci -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/chart-admin-password && echo

```
kubectl exec --namespace ci -it  svc/jenkins -- /bin/bash
Defaulted container "jenkins" out of: jenkins, config-reload, config-reload-init (init), init (init)
jenkins@jenkins-0:/$ cd /run
jenkins@jenkins-0:/run$ ls
adduser  lock  secrets
jenkins@jenkins-0:/run$ cd secrets/
jenkins@jenkins-0:/run/secrets$ ls
additional  kubernetes.io
jenkins@jenkins-0:/run/secrets$ cd additional/
jenkins@jenkins-0:/run/secrets/additional$ ls
chart-admin-password  chart-admin-username
jenkins@jenkins-0:/run/secrets/additional$ cat chart-admin-password 
ZduOzu0QWxB5IzccMXcK5Ojenkins@jenkins-0:/run/secrets/additional$ 
```
### EKS cluster would already be connected with Jenkins. Just got to manage jenkins -> cloud -> kubernetes -> test connection

### Set up personal access token from developer setting in github.



aws eks update-kubeconfig --name $cluster_name --region us-west-2

kubectl create secret -n ci docker-registry regcred --docker-server=https://hub.docker.com --docker-username=ranjiniganeshan@gmail.com --docker-password=Diehard12

stage('Docker BnP') {
 steps {
 container('kaniko') {
 sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd` --insecure --
skip-tls-verify --cache=true --destination=docker.io/ranjini/dso-demo'
 }
 }
 }