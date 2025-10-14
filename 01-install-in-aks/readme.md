# Installing API Connect in AKS

# 1. Kubernetes ingress controller prerequisites

Follow this : 
https://www.ibm.com/docs/en/api-connect/10.0.x_cd?topic=deployment-kubernetes-ingress-controller-prerequisites

# 2. Obtaining product files

Follow this : 
https://www.ibm.com/docs/en/api-connect/10.0.x_cd?topic=procedures-obtaining-product-files


# 3. Deploying operators in a single-namespace API Connect cluster

Refer the product documentation [here](https://www.ibm.com/docs/en/api-connect/10.0.8_lts?topic=docm-deploying-operators-in-single-namespace-api-connect-cluster) for more detailed info


### 3.1. Create Namespace

1. Login into the Kubernetes cluster via kubectl cli.

2. Run the following command to create namespace

```
kubectl create ns apiconnect
```

### 3.2. Install Certificate Manager

Need to Get a certificate manager. API Connect uses cert-manager v1.18.1 of cert-manager, which is a native Kubernetes certificate management controller.

You can obtain `cert-manager v1.18.1` from the API Connect v10 distribution `helper_files.zip` archive, or download it from https://github.com/cert-manager/cert-manager/releases/tag/v1.18.1

1. But the file is available [here](./files/cert-manager.yaml) in this repo. Download this.

2. Run the following command to apply the CR

```
kubectl apply -f cert-manager.yaml
```

3. Wait for `cert-manager` pods to enter `Running 1/1` status before proceeding. To check the status, run the below command.
```
kubectl get po -n cert-manager 
```

### 3.3. Create registry secret

Need to create a registry secret with credentials to be used to pull down product images.

1. Replace the values of the following variables as required
2. Run the below commands.
```
export DOCKER_SERVER=docker.io
export DOCKER_USERNAME=test_user
export DOCKER_PASSWORD=test_password
export DOCKER_EMAIL="none@none.com"
```
3. Run the following commmands to create 3 secrets

```
kubectl create secret docker-registry apic-registry-secret --docker-server=$DOCKER_SERVER --docker-username=$DOCKER_USERNAME --docker-password=$DOCKER_PASSWORD --docker-email=$DOCKER_EMAIL  -n apiconnect

kubectl create secret docker-registry datapower-docker-local-cred --docker-server=$DOCKER_SERVER --docker-username=$DOCKER_USERNAME --docker-password=$DOCKER_PASSWORD --docker-email=$DOCKER_EMAIL  -n apiconnect

kubectl create secret generic datapower-admin-credentials --from-literal=password=admin -n apiconnect
```

### 3.4. Install API Connect CRDs

1. Take the file  [ibm-apiconnect-crds.yaml](./files/ibm-apiconnect-crds.yaml) in this repo. Download this.

2. Run the following command to deploy the same

```
kubectl apply -f ibm-apiconnect-crds.yaml -n apiconnect
```

### 3.5. Install API Connect Operator

1. Take the file  [ibm-apiconnect.yaml](./files/ibm-apiconnect.yaml) in this repo. Download this.

2. In the downloaded file, replace the text `docker.io/test_user` with docker registry name and user name.

3. Run the following command to deploy the same

```
kubectl apply -f ibm-apiconnect.yaml -n apiconnect

```

### 3.6. Install ingress-ca Issuer to be used by cert-manager

1. Take the file  [ingress-issuer-v1.yaml](./files/ingress-issuer-v1.yaml) in this repo. Download this.

2. Run the following command to deploy the same

```
kubectl apply -f ingress-issuer-v1.yaml -n apiconnect

```

3. Validate that the command succeeded by running the below command.


```
kubectl get certificates -n apiconnect

```

You may get the output like this
```
NAME                         READY   SECRET                       AGE
analytics-ingestion-client   True    analytics-ingestion-client   70s
gateway-peering              True    gateway-peering              69s
gateway-service              True    gateway-service              69s
ingress-ca                   True    ingress-ca                   71s
portal-admin-client          True    portal-admin-client          71s
portal-tunnel-client         True    portal-tunnel-client         70s
```

# 4. Deploying SubSystems - Installing the management subsystem

Refer the product documentation [here](https://www.ibm.com/docs/en/api-connect/10.0.x_cd?topic=subsystems-installing-management-subsystem) for more detailed info.

### 4.1 Download the file

1. Take the file  [management_cr.yaml](./files/management_cr.yaml) in this repo. Download this.

### 4.2 Update the info (Must do)

In the downloaded file, replace the following info only if required.

1. Update the host name of the Docker Registry to which you uploaded the installation images.
```
imageRegistry: docker.io/test_user
```
2. Update the desired ingress subdomain for the API Connect stack. 

Find and replace the `111.222.333.444.nip.io` with the actual end point of your cluster.

### 4.3Update the info (as per your need)

In the downloaded file, replace the following info only if required.

1. Update the API Connect application version.
```
version: 10.0.8.4
```

2. Update the Specify your management subsystem profile,
```
profile: n1xc2.m16
```

3. Update the stroage class name.
```
storageClassName: managed-premium
```

4. Update the License info
```
  license:
    accept: true
    use: nonproduction
    license: L-HTFS-UAXYM3
```

### 4.4 Install the management CR

1. Run the following command to deploy the same

```
kubectl apply -f management_cr.yaml -n apiconnect
```

2. Verify that the management subsystem is fully installed:

```
kubectl get ManagementCluster -n apiconnect
```

The installation has completed when the READY status is True, and the SUMMARY reports that all services are online (e.g. 16/16). For example:
```
NAME         READY   SUMMARY   VERSION    RECONCILED VERSION   AGE
management   True   16/16       <version>   <version-build>       7m17s
```

3. Check your connection to the Cloud Manager user interface on the management subsystem on your Cloud Manager endpoint.

Replace the "111.222.333.444.nip.io" with the actual end point below and open the url in the browser. 

https://admin.111.222.333.444.nip.io/admin



# 5. Deploying SubSystems - Installing the gateway subsystem

Refer the product documentation [here](https://www.ibm.com/docs/en/api-connect/10.0.x_cd?topic=subsystems-installing-gateway-subsystem) for more detailed info.

### 5.1 Download the file

1. Take the file  [apigateway_cr.yaml](./files/apigateway_cr.yaml) in this repo. Download this.

### 5.2 Update the info (Must do)

In the downloaded file, replace the following info only if required.

1. Update the host name of the Docker Registry to which you uploaded the installation images.
```
imageRegistry: docker.io/test_user
```
2. Update the desired ingress subdomain for the API Connect stack. 

Find and replace the `111.222.333.444.nip.io` with the actual end point of your cluster.

### 5.3 Update the info (as per your need)

In the downloaded file, replace the following info only if required.

1. Update the API Connect application version.
```
version: 10.0.8.4
```

2. Update the Specify your management subsystem profile,
```
profile: n1xc4.m8
```

3. Update the stroage class name.
```
storageClassName: managed-premium
```

4. Update the License info
```
  license:
    accept: true
    use: nonproduction
    license: L-HTFS-UAXYM3
```

### 5.4 Install the Gateway CR

1. Run the following command to deploy the same

```
kubectl apply -f apigateway_cr.yaml -n apiconnect
```

2. Verify that the management subsystem is fully installed:

```
kubectl get GatewayCluster -n apiconnect
```

The installation has completed when the READY status is True, and the SUMMARY reports that all services are online (e.g. 16/16). For example:
```
NAME   READY   SUMMARY   VERSION    RECONCILED VERSION   AGE
gwv5   True    2/2       <version>  <version-build>      7m31s
gwv6   True    2/2       <version>  <version-build>      7m32s
```