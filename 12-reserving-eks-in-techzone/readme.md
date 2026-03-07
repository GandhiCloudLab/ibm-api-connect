# How to reserve and access EKS in Techzone

## 1. Reserver Techzone instance
 1. Search for **EKS** in techzone
  
 2. Click on **AWS Environment** in the **AWS EKS Cluster** tile (OR) Open the url in the browser https://techzone.ibm.com/my/reservations/create/651c2ff545a5d900171d94d2
  <img src="images/img01.png" >

 3. Enter the required details
 
 4. Click on **Submit** button
  <img src="images/img02.png" >
 
 5. Open the **My Reservation** Page

 6. Click on **Open this environment** button

  <img src="images/img03.png" >
 
 7. Copy all the 3 informations.

    - AWS_ACCESS_KEY_ID
    - AWS_SECRET_ACCESS_KEY
    - Download Kubernetes config by AWS CLI

  <img src="images/img04.png" >
  <img src="images/img05.png" >


## Download Kubernetes config

 1. Run the command to download kube config from EKS
 
 ```
 aws configure
 ```
 
 2. Enter the below 4 values. (use the values we copied in the previous section)

 ```
 AWS Access Key ID [****************JGIW]: AKIAQAKLJF75EBQCJGIW
 AWS Secret Access Key [****************VCDk]: xxxxxxxxxxxxxx
 Default region name [us-east-1]: us-east-1
 Default output format [json]: json
```

 3. Run the below command to Download Kubernetes config by AWS CLI (use the values we copied in the previous section)

 ```
 aws eks update-kubeconfig --region us-east-1 --name itzeks-5500028307-stzsp57g
 ```

you may get the output like this.
```
Updated context arn:aws:eks:us-east-1:000694759418:cluster/itzeks-5500028307-stzsp57g in /Users/gandhi/.kube/config
```

4. Run the below code to check the nodes count to confirm whether kubectl points to the right EKS.

```
    kubectl get nodes
```

You may get the output like this.
```
NAME                          STATUS   ROLES    AGE   VERSION
ip-10-0-2-131.ec2.internal    Ready    <none>   29h   v1.33.8-eks-efcacff
ip-10-0-21-67.ec2.internal    Ready    <none>   29h   v1.33.8-eks-efcacff
ip-10-0-28-124.ec2.internal   Ready    <none>   14h   v1.33.8-eks-efcacff
ip-10-0-8-66.ec2.internal     Ready    <none>   29h   v1.33.8-eks-efcacff
```

Now you can access the EKS via Kubectl.
