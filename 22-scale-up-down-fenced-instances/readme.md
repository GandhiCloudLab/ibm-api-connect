#  Scale down and scale up the applications.

Here are the steps and commands used to scale down and scale up the applications.

We are executing the scale down steps and see the below mentioned probelm is appearing or not.

*When the applications are scaled down and then scaled up again, the StatefulSet/Deployment pods may get scheduled on a different node than the one they were originally running on. Since APIC uses EBS volumes, the volume will detach from the previous node and trying to attach to the new node, which is causing the issue even in EKS Auto Mode.*

## 1. PODs 

PODs status before scalling down.

Command : 

 ```
 kubectl get pods -n apiconnect -o wide
 ```

Result :
 ```
NAME                                                    READY   STATUS      RESTARTS   AGE
NAME                                                    READY   STATUS      RESTARTS   AGE     IP            NODE                          NOMINATED NODE   READINESS GATES
analytics-director-6dd585d56c-4n9x2                     1/1     Running     0          27h     10.0.25.136   ip-10-0-28-124.ec2.internal   <none>           <none>
analytics-ingestion-0                                   1/1     Running     0          27h     10.0.8.1      ip-10-0-2-131.ec2.internal    <none>           <none>
analytics-mtls-gw-6477f4564f-cgl66                      1/1     Running     0          27h     10.0.16.114   ip-10-0-28-124.ec2.internal   <none>           <none>
analytics-oscron-29552550-7g4lt                         0/1     Completed   0          3m41s   10.0.17.13    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-osinit-f48gp                                  0/1     Completed   0          2d19h   10.0.8.1      ip-10-0-2-131.ec2.internal    <none>           <none>
analytics-reindex-29551680-z5r7g                        0/1     Completed   0          14h     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-storage-0                                     2/2     Running     0          27h     10.0.2.251    ip-10-0-8-66.ec2.internal     <none>           <none>
analytics-system-check-29552530-qnnvz                   0/1     Completed   0          23m     10.0.24.106   ip-10-0-21-67.ec2.internal    <none>           <none>
datapower-operator-85656f778c-fpzbv                     1/1     Running     0          27h     10.0.28.67    ip-10-0-21-67.ec2.internal    <none>           <none>
datapower-operator-conversion-webhook-dc9444944-m4tt9   1/1     Running     0          27h     10.0.17.101   ip-10-0-28-124.ec2.internal   <none>           <none>
edb-operator-5bdd897d44-z8nr5                           1/1     Running     0          27h     10.0.18.188   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-0                                                  1/1     Running     0          26h     10.0.21.113   ip-10-0-21-67.ec2.internal    <none>           <none>
gwv6-1                                                  1/1     Running     0          26h     10.0.22.185   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-2                                                  1/1     Running     0          26h     10.0.7.246    ip-10-0-2-131.ec2.internal    <none>           <none>
gwv6-system-check-29552540-b7lfw                        0/1     Completed   0          13m     10.0.20.218   ip-10-0-21-67.ec2.internal    <none>           <none>
ibm-apiconnect-69699bfd99-wtvrx                         1/1     Running     0          27h     10.0.24.67    ip-10-0-21-67.ec2.internal    <none>           <none>
management-8913d321-db-1                                1/1     Running     0          3d5h    10.0.2.218    ip-10-0-8-66.ec2.internal     <none>           <none>
management-analytics-proxy-75ccdb88c-s2pdf              1/1     Running     0          27h     10.0.25.224   ip-10-0-28-124.ec2.internal   <none>           <none>
management-analytics-push-29552535-sh484                0/1     Completed   0          18m     10.0.24.106   ip-10-0-21-67.ec2.internal    <none>           <none>
management-analytics-ui-7f8bb65764-r8fbb                1/1     Running     0          27h     10.0.27.99    ip-10-0-21-67.ec2.internal    <none>           <none>
management-apim-74b85ccffb-nqkg6                        1/1     Running     0          27h     10.0.16.80    ip-10-0-28-124.ec2.internal   <none>           <none>
management-audit-logging-cd49969d7-nl464                1/1     Running     0          27h     10.0.24.22    ip-10-0-21-67.ec2.internal    <none>           <none>
management-client-downloads-server-fb54c7598-rzlcc      1/1     Running     0          27h     10.0.0.68     ip-10-0-2-131.ec2.internal    <none>           <none>
management-consumer-catalog-55bdf895f7-f6kx8            1/1     Running     0          27h     10.0.18.84    ip-10-0-21-67.ec2.internal    <none>           <none>
management-juhu-7485bff6dd-rd6dr                        1/1     Running     0          27h     10.0.17.193   ip-10-0-28-124.ec2.internal   <none>           <none>
management-ldap-768d797b98-24vr9                        1/1     Running     0          27h     10.0.29.65    ip-10-0-21-67.ec2.internal    <none>           <none>
management-lur-78c84798c9-2ch2n                         1/1     Running     0          27h     10.0.22.130   ip-10-0-28-124.ec2.internal   <none>           <none>
management-natscluster-0                                1/1     Running     0          27h     10.0.19.223   ip-10-0-28-124.ec2.internal   <none>           <none>
management-portal-proxy-58b9b9d465-w8zbq                1/1     Running     0          27h     10.0.4.252    ip-10-0-2-131.ec2.internal    <none>           <none>
management-s3proxy-0                                    1/1     Running     0          27h     10.0.25.200   ip-10-0-28-124.ec2.internal   <none>           <none>
management-system-check-29552525-gp4f8                  0/1     Completed   0          28m     10.0.17.13    ip-10-0-21-67.ec2.internal    <none>           <none>
management-taskmanager-78c7698578-j7hqg                 1/1     Running     0          27h     10.0.31.101   ip-10-0-28-124.ec2.internal   <none>           <none>
management-ui-6957fc5669-bvxrl                          2/2     Running     0          27h     10.0.21.129   ip-10-0-28-124.ec2.internal   <none>           <none>
management-websocket-proxy-7ff95b8d7d-mxx8c             1/1     Running     0          27h     10.0.9.248    ip-10-0-2-131.ec2.internal    <none>           <none>
portal-0fbf20f0-db-0                                    2/2     Running     0          27h     10.0.2.134    ip-10-0-2-131.ec2.internal    <none>           <none>
portal-0fbf20f0-www-0                                   2/2     Running     0          27h     10.0.25.243   ip-10-0-28-124.ec2.internal   <none>           <none>
portal-nginx-0                                          1/1     Running     0          27h     10.0.24.192   ip-10-0-21-67.ec2.internal    <none>           <none>
portal-system-check-29552535-g25wh                      0/1     Completed   0          18m     10.0.19.210   ip-10-0-21-67.ec2.internal    <none>           <none>
 ```

## 2. Scale down

### 2.1 Scale down the statefulset

Scale down the statefulset to 0.
 ```
kubectl scale statefulset --all --replicas=0 -n apiconnect
 ```
``` 
gandhi@Jeyas-MacBook-Pro files % kubectl scale statefulset --all --replicas=0 -n apiconnect
statefulset.apps/analytics-ingestion scaled
statefulset.apps/analytics-storage scaled
statefulset.apps/gwv6 scaled
statefulset.apps/management-natscluster scaled
statefulset.apps/management-s3proxy scaled
statefulset.apps/portal-0fbf20f0-db scaled
statefulset.apps/portal-0fbf20f0-www scaled
statefulset.apps/portal-nginx scaled
```

#### PODs now

The pod status shows that several pods have been deleted.

 ```
 NAME                                                    READY   STATUS      RESTARTS   AGE     IP            NODE                          NOMINATED NODE   READINESS GATES
analytics-director-6dd585d56c-4n9x2                     1/1     Running     0          29h     10.0.25.136   ip-10-0-28-124.ec2.internal   <none>           <none>
analytics-mtls-gw-6477f4564f-cgl66                      1/1     Running     0          29h     10.0.16.114   ip-10-0-28-124.ec2.internal   <none>           <none>
analytics-oscron-29552670-tbxcg                         0/1     Completed   0          9m53s   10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-osinit-f48gp                                  0/1     Completed   0          2d21h   10.0.8.1      ip-10-0-2-131.ec2.internal    <none>           <none>
analytics-reindex-29551680-z5r7g                        0/1     Completed   0          16h     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-system-check-29552650-66l7m                   0/1     Completed   0          29m     10.0.21.21    ip-10-0-21-67.ec2.internal    <none>           <none>
datapower-operator-85656f778c-fpzbv                     1/1     Running     0          29h     10.0.28.67    ip-10-0-21-67.ec2.internal    <none>           <none>
datapower-operator-conversion-webhook-dc9444944-m4tt9   1/1     Running     0          29h     10.0.17.101   ip-10-0-28-124.ec2.internal   <none>           <none>
edb-operator-5bdd897d44-z8nr5                           1/1     Running     0          29h     10.0.18.188   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-0                                                  1/1     Running     0          28h     10.0.21.113   ip-10-0-21-67.ec2.internal    <none>           <none>
gwv6-1                                                  0/1     Running     0          68s     10.0.26.214   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-system-check-29552660-8lj2p                        0/1     Completed   0          19m     10.0.28.150   ip-10-0-21-67.ec2.internal    <none>           <none>
ibm-apiconnect-69699bfd99-wtvrx                         1/1     Running     0          29h     10.0.24.67    ip-10-0-21-67.ec2.internal    <none>           <none>
management-8913d321-db-1                                1/1     Running     0          3d7h    10.0.2.218    ip-10-0-8-66.ec2.internal     <none>           <none>
management-analytics-proxy-75ccdb88c-s2pdf              1/1     Running     0          29h     10.0.25.224   ip-10-0-28-124.ec2.internal   <none>           <none>
management-analytics-push-29552655-z65n4                0/1     Completed   0          24m     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
management-analytics-ui-7f8bb65764-r8fbb                1/1     Running     0          29h     10.0.27.99    ip-10-0-21-67.ec2.internal    <none>           <none>
management-apim-74b85ccffb-nqkg6                        1/1     Running     0          29h     10.0.16.80    ip-10-0-28-124.ec2.internal   <none>           <none>
management-audit-logging-cd49969d7-nl464                1/1     Running     0          29h     10.0.24.22    ip-10-0-21-67.ec2.internal    <none>           <none>
management-client-downloads-server-fb54c7598-rzlcc      1/1     Running     0          29h     10.0.0.68     ip-10-0-2-131.ec2.internal    <none>           <none>
management-consumer-catalog-55bdf895f7-f6kx8            1/1     Running     0          29h     10.0.18.84    ip-10-0-21-67.ec2.internal    <none>           <none>
management-juhu-7485bff6dd-rd6dr                        1/1     Running     0          29h     10.0.17.193   ip-10-0-28-124.ec2.internal   <none>           <none>
management-ldap-768d797b98-24vr9                        1/1     Running     0          29h     10.0.29.65    ip-10-0-21-67.ec2.internal    <none>           <none>
management-lur-78c84798c9-2ch2n                         1/1     Running     0          29h     10.0.22.130   ip-10-0-28-124.ec2.internal   <none>           <none>
management-portal-proxy-58b9b9d465-w8zbq                1/1     Running     0          29h     10.0.4.252    ip-10-0-2-131.ec2.internal    <none>           <none>
management-system-check-29552645-s2k9n                  0/1     Completed   0          34m     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
management-taskmanager-78c7698578-j7hqg                 1/1     Running     0          29h     10.0.31.101   ip-10-0-28-124.ec2.internal   <none>           <none>
management-ui-6957fc5669-bvxrl                          2/2     Running     0          29h     10.0.21.129   ip-10-0-28-124.ec2.internal   <none>           <none>
management-websocket-proxy-7ff95b8d7d-mxx8c             1/1     Running     0          29h     10.0.9.248    ip-10-0-2-131.ec2.internal    <none>           <none>
portal-system-check-29552655-nzqvh                      0/1     Completed   0          24m     10.0.18.113   ip-10-0-21-67.ec2.internal    <none>           <none>
 ```

### 2.2 Fenced Instances

#### 2.2.1 Meaning of k8s.enterprisedb.io/fencedInstances

Fencing means temporarily isolating or disabling a database instance so that it does not participate in the PostgreSQL cluster.

When an instance is fenced:
- The database pod is isolated
- It does not accept traffic
- It does not participate in replication
- It is effectively taken out of the cluster safely

#### 2.2.2 Get the cluster name

Command : 

 ```
 kubectl get clusters.postgresql.k8s.enterprisedb.io -n apiconnect
 ```

Result :
 ```
NAME                     AGE    INSTANCES   READY   STATUS                     PRIMARY
management-8913d321-db   3d4h   1           1       Cluster in healthy state   management-8913d321-db-1
 ```

#### 2.2.3 Cluster Yaml


Command : 

 ```
kubectl get cluster management-8913d321-db -o yaml -n apiconnect
 ```

Result :

 ```
 .....
 .....
 .....
apiVersion: postgresql.k8s.enterprisedb.io/v1
kind: Cluster
metadata:
  annotations:
    apiconnect-operator/hash: b0a21b85ab6846f85ac6d2b547d0e1a9b13d0da8c1adfd89c5078df46587c581
    k8s.enterprisedb.io/addons: '["velero"]'
    k8s.enterprisedb.io/backupInstance: management-8913d321-db-1
    k8s.enterprisedb.io/fencedInstances: '["*"]'
    k8s.enterprisedb.io/snapshotAllowColdBackupOnPrimary: enabled
  creationTimestamp: "2026-03-07T08:46:50Z"
  .....
  .....
  .....
 ``` 

See the complete file here [here](./files/cluster.yaml) from this repo.

#### 2.2.4 fence the Instances

You can fence all as there is only one db pod as of now.

 ``` 
kubectl annotate cluster management-8913d321-db k8s.enterprisedb.io/fencedInstances='["*"]' -n apiconnect  --overwrite
 ``` 

 ``` 
 gandhi@Jeyas-MacBook-Pro files % kubectl annotate cluster \
 management-8913d321-db k8s.enterprisedb.io/fencedInstances='["*"]' -n apiconnect  --overwrite

cluster.postgresql.k8s.enterprisedb.io/management-8913d321-db annotated
``` 


#### 2.2.5 PODs now

Here is the status of the pods now.

 ```
 NAME                                                    READY   STATUS      RESTARTS       AGE     IP            NODE                          NOMINATED NODE   READINESS GATES
analytics-director-6dd585d56c-4n9x2                     1/1     Running     0              29h     10.0.25.136   ip-10-0-28-124.ec2.internal   <none>           <none>
analytics-mtls-gw-6477f4564f-cgl66                      1/1     Running     0              29h     10.0.16.114   ip-10-0-28-124.ec2.internal   <none>           <none>
analytics-oscron-29552670-tbxcg                         0/1     Completed   0              14m     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-osinit-f48gp                                  0/1     Completed   0              2d21h   10.0.8.1      ip-10-0-2-131.ec2.internal    <none>           <none>
analytics-reindex-29551680-z5r7g                        0/1     Completed   0              16h     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-system-check-29552650-66l7m                   0/1     Completed   0              34m     10.0.21.21    ip-10-0-21-67.ec2.internal    <none>           <none>
datapower-operator-85656f778c-fpzbv                     1/1     Running     0              29h     10.0.28.67    ip-10-0-21-67.ec2.internal    <none>           <none>
datapower-operator-conversion-webhook-dc9444944-m4tt9   1/1     Running     0              29h     10.0.17.101   ip-10-0-28-124.ec2.internal   <none>           <none>
edb-operator-5bdd897d44-z8nr5                           1/1     Running     0              29h     10.0.18.188   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-0                                                  1/1     Running     0              28h     10.0.21.113   ip-10-0-21-67.ec2.internal    <none>           <none>
gwv6-1                                                  1/1     Running     0              6m      10.0.26.214   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-2                                                  1/1     Running     0              3m46s   10.0.0.210    ip-10-0-2-131.ec2.internal    <none>           <none>
gwv6-system-check-29552660-8lj2p                        0/1     Completed   0              24m     10.0.28.150   ip-10-0-21-67.ec2.internal    <none>           <none>
ibm-apiconnect-69699bfd99-wtvrx                         1/1     Running     0              29h     10.0.24.67    ip-10-0-21-67.ec2.internal    <none>           <none>
management-8913d321-db-1                                0/1     Running     0              3d7h    10.0.2.218    ip-10-0-8-66.ec2.internal     <none>           <none>
management-analytics-proxy-75ccdb88c-s2pdf              1/1     Running     0              29h     10.0.25.224   ip-10-0-28-124.ec2.internal   <none>           <none>
management-analytics-push-29552655-z65n4                0/1     Completed   0              29m     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
management-analytics-ui-7f8bb65764-r8fbb                1/1     Running     0              29h     10.0.27.99    ip-10-0-21-67.ec2.internal    <none>           <none>
management-apim-74b85ccffb-nqkg6                        0/1     Running     1 (104s ago)   29h     10.0.16.80    ip-10-0-28-124.ec2.internal   <none>           <none>
management-audit-logging-cd49969d7-nl464                1/1     Running     0              29h     10.0.24.22    ip-10-0-21-67.ec2.internal    <none>           <none>
management-client-downloads-server-fb54c7598-rzlcc      1/1     Running     0              29h     10.0.0.68     ip-10-0-2-131.ec2.internal    <none>           <none>
management-consumer-catalog-55bdf895f7-f6kx8            1/1     Running     0              29h     10.0.18.84    ip-10-0-21-67.ec2.internal    <none>           <none>
management-juhu-7485bff6dd-rd6dr                        1/1     Running     0              29h     10.0.17.193   ip-10-0-28-124.ec2.internal   <none>           <none>
management-ldap-768d797b98-24vr9                        1/1     Running     0              29h     10.0.29.65    ip-10-0-21-67.ec2.internal    <none>           <none>
management-lur-78c84798c9-2ch2n                         0/1     Running     2 (36s ago)    29h     10.0.22.130   ip-10-0-28-124.ec2.internal   <none>           <none>
management-portal-proxy-58b9b9d465-w8zbq                1/1     Running     0              29h     10.0.4.252    ip-10-0-2-131.ec2.internal    <none>           <none>
management-system-check-29552645-s2k9n                  0/1     Completed   0              39m     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
management-taskmanager-78c7698578-j7hqg                 0/1     Running     0              29h     10.0.31.101   ip-10-0-28-124.ec2.internal   <none>           <none>
management-ui-6957fc5669-bvxrl                          2/2     Running     0              29h     10.0.21.129   ip-10-0-28-124.ec2.internal   <none>           <none>
management-websocket-proxy-7ff95b8d7d-mxx8c             1/1     Running     0              29h     10.0.9.248    ip-10-0-2-131.ec2.internal    <none>           <none>
portal-system-check-29552655-nzqvh                      0/1     Completed   0              29m     10.0.18.113   ip-10-0-21-67.ec2.internal    <none>           <none>
 ```
#### 2.2.6 Describe management-db

You can note the management-8913d321-db-1  is in running and container not ready status.

See the describe of the pod  here [desc.txt](./files/desc.txt) from this repo.

### 2.3 Scale down the deployments

Scale down the deployments to 0

 ``` 
kubectl scale deployment --all --replicas=0 -n apiconnect
 ``` 

```
gandhi@Jeyas-MacBook-Pro files % kubectl scale deployment --all --replicas=0 -n apiconnect

deployment.apps/analytics-director scaled
deployment.apps/analytics-mtls-gw scaled
deployment.apps/datapower-operator scaled
deployment.apps/datapower-operator-conversion-webhook scaled
deployment.apps/edb-operator scaled
deployment.apps/ibm-apiconnect scaled
deployment.apps/management-analytics-proxy scaled
deployment.apps/management-analytics-ui scaled
deployment.apps/management-apim scaled
deployment.apps/management-audit-logging scaled
deployment.apps/management-client-downloads-server scaled
deployment.apps/management-consumer-catalog scaled
deployment.apps/management-juhu scaled
deployment.apps/management-ldap scaled
deployment.apps/management-lur scaled
deployment.apps/management-portal-proxy scaled
deployment.apps/management-taskmanager scaled
deployment.apps/management-ui scaled
deployment.apps/management-websocket-proxy scaled
 ```

#### PODs now

The pod status shows that almost all the pods have been deleted.

 ```
 NAME                                       READY   STATUS             RESTARTS       AGE     IP            NODE                          NOMINATED NODE   READINESS GATES
analytics-oscron-29552670-tbxcg            0/1     Completed          0              35m     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-oscron-29552700-2s2tz            0/1     CrashLoopBackOff   5 (2m1s ago)   5m24s   10.0.0.14     ip-10-0-8-66.ec2.internal     <none>           <none>
analytics-osinit-f48gp                     0/1     Completed          0              2d22h   10.0.8.1      ip-10-0-2-131.ec2.internal    <none>           <none>
analytics-reindex-29551680-z5r7g           0/1     Completed          0              17h     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-system-check-29552650-66l7m      0/1     Completed          0              55m     10.0.21.21    ip-10-0-21-67.ec2.internal    <none>           <none>
gwv6-0                                     1/1     Running            0              29h     10.0.21.113   ip-10-0-21-67.ec2.internal    <none>           <none>
gwv6-1                                     1/1     Running            0              26m     10.0.26.214   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-2                                     1/1     Running            0              24m     10.0.0.210    ip-10-0-2-131.ec2.internal    <none>           <none>
gwv6-system-check-29552660-8lj2p           0/1     Completed          0              45m     10.0.28.150   ip-10-0-21-67.ec2.internal    <none>           <none>
management-8913d321-db-1                   0/1     Running            0              3d8h    10.0.2.218    ip-10-0-8-66.ec2.internal     <none>           <none>
management-analytics-push-29552655-z65n4   0/1     Completed          0              50m     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
management-system-check-29552705-7gz8j     0/1     Completed          0              24s     10.0.12.58    ip-10-0-8-66.ec2.internal     <none>           <none>
portal-system-check-29552655-nzqvh         0/1     Completed          0              50m     10.0.18.113   ip-10-0-21-67.ec2.internal    <none>           <none>

 ```
### 2.4 Access the APIC 

If you access the APIC admin UI or any other subsytems they are not running.

## 3. Scale up

Lets scale up again to the previous state.

### 3.1 Scale up the statefulset

Scale up the statefulset to 1
 ```
kubectl scale statefulset --all --replicas=1 -n apiconnect
 ```

 ```
 gandhi@Jeyas-MacBook-Pro files % kubectl scale statefulset --all --replicas=1 -n apiconnect

statefulset.apps/analytics-ingestion scaled
statefulset.apps/analytics-storage scaled
statefulset.apps/gwv6 scaled
statefulset.apps/management-natscluster scaled
statefulset.apps/management-s3proxy scaled
statefulset.apps/portal-0fbf20f0-db scaled
statefulset.apps/portal-0fbf20f0-www scaled
statefulset.apps/portal-nginx scaled
 ```

#### PODs now

The pod status shows that few pods have been created.


 ```
 gandhi@Jeyas-MacBook-Pro files %  kubectl get pods -n apiconnect -o wide
NAME                                       READY   STATUS      RESTARTS   AGE     IP            NODE                          NOMINATED NODE   READINESS GATES
analytics-ingestion-0                      1/1     Running     0          7m20s   10.0.6.196    ip-10-0-8-66.ec2.internal     <none>           <none>
analytics-oscron-29552670-tbxcg            0/1     Completed   0          43m     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-osinit-f48gp                     0/1     Completed   0          2d22h   10.0.8.1      ip-10-0-2-131.ec2.internal    <none>           <none>
analytics-reindex-29551680-z5r7g           0/1     Completed   0          17h     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-storage-0                        2/2     Running     0          7m20s   10.0.2.251    ip-10-0-8-66.ec2.internal     <none>           <none>
analytics-system-check-29552710-qqzg9      0/1     Completed   0          3m32s   10.0.26.214   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-0                                     1/1     Running     0          29h     10.0.21.113   ip-10-0-21-67.ec2.internal    <none>           <none>
gwv6-system-check-29552660-8lj2p           0/1     Completed   0          53m     10.0.28.150   ip-10-0-21-67.ec2.internal    <none>           <none>
management-8913d321-db-1                   0/1     Running     0          3d8h    10.0.2.218    ip-10-0-8-66.ec2.internal     <none>           <none>
management-analytics-push-29552655-z65n4   0/1     Completed   0          58m     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
management-natscluster-0                   1/1     Running     0          7m20s   10.0.4.252    ip-10-0-2-131.ec2.internal    <none>           <none>
management-s3proxy-0                       1/1     Running     0          7m19s   10.0.28.202   ip-10-0-28-124.ec2.internal   <none>           <none>
management-system-check-29552705-7gz8j     0/1     Completed   0          8m32s   10.0.12.58    ip-10-0-8-66.ec2.internal     <none>           <none>
portal-0fbf20f0-db-0                       2/2     Running     0          7m19s   10.0.8.138    ip-10-0-2-131.ec2.internal    <none>           <none>
portal-0fbf20f0-www-0                      2/2     Running     0          7m19s   10.0.21.129   ip-10-0-28-124.ec2.internal   <none>           <none>
portal-nginx-0                             1/1     Running     0          7m18s   10.0.19.156   ip-10-0-28-124.ec2.internal   <none>           <none>
portal-system-check-29552655-nzqvh         0/1     Completed   0          58m     10.0.18.113   ip-10-0-21-67.ec2.internal    <none>           <none>
 ```

### 3.2 Scale up the deployment to original count

Scale up the deployments to 1

 ``` 
kubectl scale deployment --all --replicas=1 -n apiconnect
 ``` 

```
gandhi@Jeyas-MacBook-Pro files % kubectl scale deployment --all --replicas=1 -n apiconnect

deployment.apps/analytics-director scaled
deployment.apps/analytics-mtls-gw scaled
deployment.apps/datapower-operator scaled
deployment.apps/datapower-operator-conversion-webhook scaled
deployment.apps/edb-operator scaled
deployment.apps/ibm-apiconnect scaled
deployment.apps/management-analytics-proxy scaled
deployment.apps/management-analytics-ui scaled
deployment.apps/management-apim scaled
deployment.apps/management-audit-logging scaled
deployment.apps/management-client-downloads-server scaled
deployment.apps/management-consumer-catalog scaled
deployment.apps/management-juhu scaled
deployment.apps/management-ldap scaled
deployment.apps/management-lur scaled
deployment.apps/management-portal-proxy scaled
deployment.apps/management-taskmanager scaled
deployment.apps/management-ui scaled
deployment.apps/management-websocket-proxy scaled
```

#### PODs now

The pod status shows that most of the pods have been created.


 ```
gandhi@Jeyas-MacBook-Pro files %  kubectl get pods -n apiconnect -o wide
NAME                                                    READY   STATUS      RESTARTS   AGE     IP            NODE                          NOMINATED NODE   READINESS GATES
analytics-director-6dd585d56c-twhzm                     1/1     Running     0          81s     10.0.24.75    ip-10-0-28-124.ec2.internal   <none>           <none>
analytics-ingestion-0                                   1/1     Running     0          9m8s    10.0.6.196    ip-10-0-8-66.ec2.internal     <none>           <none>
analytics-mtls-gw-6477f4564f-5fmph                      1/1     Running     0          81s     10.0.22.130   ip-10-0-28-124.ec2.internal   <none>           <none>
analytics-oscron-29552715-8f8t8                         0/1     Completed   0          20s     10.0.8.91     ip-10-0-2-131.ec2.internal    <none>           <none>
analytics-osinit-f48gp                                  0/1     Completed   0          2d22h   10.0.8.1      ip-10-0-2-131.ec2.internal    <none>           <none>
analytics-reindex-29551680-z5r7g                        0/1     Completed   0          17h     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-storage-0                                     2/2     Running     0          9m8s    10.0.2.251    ip-10-0-8-66.ec2.internal     <none>           <none>
analytics-system-check-29552710-qqzg9                   0/1     Completed   0          5m20s   10.0.26.214   ip-10-0-28-124.ec2.internal   <none>           <none>
datapower-operator-85656f778c-kr4nf                     1/1     Running     0          80s     10.0.21.202   ip-10-0-28-124.ec2.internal   <none>           <none>
datapower-operator-conversion-webhook-dc9444944-2bpjb   1/1     Running     0          80s     10.0.11.4     ip-10-0-2-131.ec2.internal    <none>           <none>
edb-operator-5bdd897d44-qbgb5                           1/1     Running     0          80s     10.0.25.216   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-0                                                  1/1     Running     0          29h     10.0.21.113   ip-10-0-21-67.ec2.internal    <none>           <none>
gwv6-1                                                  0/1     Running     0          77s     10.0.26.214   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-system-check-29552660-8lj2p                        0/1     Completed   0          55m     10.0.28.150   ip-10-0-21-67.ec2.internal    <none>           <none>
ibm-apiconnect-69699bfd99-67vz2                         1/1     Running     0          79s     10.0.4.133    ip-10-0-2-131.ec2.internal    <none>           <none>
management-8913d321-db-1                                0/1     Running     0          3d8h    10.0.2.218    ip-10-0-8-66.ec2.internal     <none>           <none>
management-analytics-proxy-75ccdb88c-4b8zk              1/1     Running     0          79s     10.0.14.141   ip-10-0-2-131.ec2.internal    <none>           <none>
management-analytics-push-29552655-z65n4                0/1     Completed   0          60m     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
management-analytics-push-29552715-wjpjt                1/1     Running     0          20s     10.0.0.210    ip-10-0-2-131.ec2.internal    <none>           <none>
management-analytics-ui-7f8bb65764-m8zq2                1/1     Running     0          79s     10.0.17.193   ip-10-0-28-124.ec2.internal   <none>           <none>
management-apim-74b85ccffb-mcjr6                        0/1     Running     0          79s     10.0.19.223   ip-10-0-28-124.ec2.internal   <none>           <none>
management-audit-logging-cd49969d7-jpzct                1/1     Running     0          78s     10.0.14.152   ip-10-0-2-131.ec2.internal    <none>           <none>
management-client-downloads-server-fb54c7598-wwwwr      1/1     Running     0          78s     10.0.4.239    ip-10-0-2-131.ec2.internal    <none>           <none>
management-consumer-catalog-55bdf895f7-9d5v6            1/1     Running     0          78s     10.0.16.80    ip-10-0-28-124.ec2.internal   <none>           <none>
management-juhu-7485bff6dd-khvjz                        1/1     Running     0          77s     10.0.8.1      ip-10-0-2-131.ec2.internal    <none>           <none>
management-ldap-768d797b98-5crdw                        1/1     Running     0          77s     10.0.22.225   ip-10-0-28-124.ec2.internal   <none>           <none>
management-lur-78c84798c9-sp6zv                         0/1     Running     0          77s     10.0.0.68     ip-10-0-2-131.ec2.internal    <none>           <none>
management-natscluster-0                                1/1     Running     0          9m8s    10.0.4.252    ip-10-0-2-131.ec2.internal    <none>           <none>
management-portal-proxy-58b9b9d465-mdstl                1/1     Running     0          76s     10.0.3.113    ip-10-0-2-131.ec2.internal    <none>           <none>
management-s3proxy-0                                    1/1     Running     0          9m7s    10.0.28.202   ip-10-0-28-124.ec2.internal   <none>           <none>
management-system-check-29552705-7gz8j                  0/1     Completed   0          10m     10.0.12.58    ip-10-0-8-66.ec2.internal     <none>           <none>
management-taskmanager-78c7698578-66q9b                 0/1     Running     0          76s     10.0.10.138   ip-10-0-2-131.ec2.internal    <none>           <none>
management-ui-6957fc5669-hhf5c                          2/2     Running     0          75s     10.0.14.0     ip-10-0-2-131.ec2.internal    <none>           <none>
management-websocket-proxy-7ff95b8d7d-nxf98             1/1     Running     0          75s     10.0.2.134    ip-10-0-2-131.ec2.internal    <none>           <none>
portal-0fbf20f0-db-0                                    2/2     Running     0          9m7s    10.0.8.138    ip-10-0-2-131.ec2.internal    <none>           <none>
portal-0fbf20f0-www-0                                   2/2     Running     0          9m7s    10.0.21.129   ip-10-0-28-124.ec2.internal   <none>           <none>
portal-nginx-0                                          1/1     Running     0          9m6s    10.0.19.156   ip-10-0-28-124.ec2.internal   <none>           <none>
portal-system-check-29552715-x59hs                      0/1     Completed   0          20s     10.0.8.109    ip-10-0-2-131.ec2.internal    <none>           <none>
 ```

### 3.3 fence the Instances

You can fence all or one as there is only one db pod as of now.

 ``` 
kubectl annotate cluster management-8913d321-db k8s.enterprisedb.io/fencedInstances='[""]' \
-n apiconnect --overwrite
 ``` 

 ``` 
 gandhi@Jeyas-MacBook-Pro files % kubectl annotate cluster management-8913d321-db \
 k8s.enterprisedb.io/fencedInstances='[""]' -n apiconnect --overwrite

cluster.postgresql.k8s.enterprisedb.io/management-8913d321-db annotated
 ``` 
#### PODs now

The pod status shows that all the pods have been created.


 ```
 gandhi@Jeyas-MacBook-Pro files %  kubectl get pods -n apiconnect -o wide
NAME                                                    READY   STATUS      RESTARTS      AGE     IP            NODE                          NOMINATED NODE   READINESS GATES
analytics-director-6dd585d56c-twhzm                     1/1     Running     0             4m25s   10.0.24.75    ip-10-0-28-124.ec2.internal   <none>           <none>
analytics-ingestion-0                                   1/1     Running     0             12m     10.0.6.196    ip-10-0-8-66.ec2.internal     <none>           <none>
analytics-mtls-gw-6477f4564f-5fmph                      1/1     Running     0             4m25s   10.0.22.130   ip-10-0-28-124.ec2.internal   <none>           <none>
analytics-oscron-29552715-8f8t8                         0/1     Completed   0             3m24s   10.0.8.91     ip-10-0-2-131.ec2.internal    <none>           <none>
analytics-osinit-f48gp                                  0/1     Completed   0             2d22h   10.0.8.1      ip-10-0-2-131.ec2.internal    <none>           <none>
analytics-reindex-29551680-z5r7g                        0/1     Completed   0             17h     10.0.21.39    ip-10-0-21-67.ec2.internal    <none>           <none>
analytics-storage-0                                     2/2     Running     0             12m     10.0.2.251    ip-10-0-8-66.ec2.internal     <none>           <none>
analytics-system-check-29552710-qqzg9                   0/1     Completed   0             8m24s   10.0.26.214   ip-10-0-28-124.ec2.internal   <none>           <none>
datapower-operator-85656f778c-kr4nf                     1/1     Running     0             4m24s   10.0.21.202   ip-10-0-28-124.ec2.internal   <none>           <none>
datapower-operator-conversion-webhook-dc9444944-2bpjb   1/1     Running     0             4m24s   10.0.11.4     ip-10-0-2-131.ec2.internal    <none>           <none>
edb-operator-5bdd897d44-qbgb5                           1/1     Running     0             4m24s   10.0.25.216   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-0                                                  1/1     Running     0             29h     10.0.21.113   ip-10-0-21-67.ec2.internal    <none>           <none>
gwv6-1                                                  1/1     Running     0             4m21s   10.0.26.214   ip-10-0-28-124.ec2.internal   <none>           <none>
gwv6-2                                                  0/1     Running     0             2m8s    10.0.5.112    ip-10-0-2-131.ec2.internal    <none>           <none>
gwv6-system-check-29552660-8lj2p                        0/1     Completed   0             58m     10.0.28.150   ip-10-0-21-67.ec2.internal    <none>           <none>
ibm-apiconnect-69699bfd99-67vz2                         1/1     Running     0             4m23s   10.0.4.133    ip-10-0-2-131.ec2.internal    <none>           <none>
management-8913d321-db-1                                1/1     Running     0             3d8h    10.0.2.218    ip-10-0-8-66.ec2.internal     <none>           <none>
management-analytics-proxy-75ccdb88c-4b8zk              1/1     Running     0             4m23s   10.0.14.141   ip-10-0-2-131.ec2.internal    <none>           <none>
management-analytics-push-29552715-wjpjt                0/1     Completed   0             3m24s   10.0.0.210    ip-10-0-2-131.ec2.internal    <none>           <none>
management-analytics-ui-7f8bb65764-m8zq2                1/1     Running     0             4m23s   10.0.17.193   ip-10-0-28-124.ec2.internal   <none>           <none>
management-apim-74b85ccffb-mcjr6                        1/1     Running     1 (32s ago)   4m23s   10.0.19.223   ip-10-0-28-124.ec2.internal   <none>           <none>
management-audit-logging-cd49969d7-jpzct                1/1     Running     0             4m22s   10.0.14.152   ip-10-0-2-131.ec2.internal    <none>           <none>
management-client-downloads-server-fb54c7598-wwwwr      1/1     Running     0             4m22s   10.0.4.239    ip-10-0-2-131.ec2.internal    <none>           <none>
management-consumer-catalog-55bdf895f7-9d5v6            1/1     Running     0             4m22s   10.0.16.80    ip-10-0-28-124.ec2.internal   <none>           <none>
management-juhu-7485bff6dd-khvjz                        1/1     Running     0             4m21s   10.0.8.1      ip-10-0-2-131.ec2.internal    <none>           <none>
management-ldap-768d797b98-5crdw                        1/1     Running     0             4m21s   10.0.22.225   ip-10-0-28-124.ec2.internal   <none>           <none>
management-lur-78c84798c9-sp6zv                         1/1     Running     1 (3m ago)    4m21s   10.0.0.68     ip-10-0-2-131.ec2.internal    <none>           <none>
management-natscluster-0                                1/1     Running     0             12m     10.0.4.252    ip-10-0-2-131.ec2.internal    <none>           <none>
management-portal-proxy-58b9b9d465-mdstl                1/1     Running     0             4m20s   10.0.3.113    ip-10-0-2-131.ec2.internal    <none>           <none>
management-s3proxy-0                                    1/1     Running     0             12m     10.0.28.202   ip-10-0-28-124.ec2.internal   <none>           <none>
management-system-check-29552705-7gz8j                  0/1     Completed   0             13m     10.0.12.58    ip-10-0-8-66.ec2.internal     <none>           <none>
management-taskmanager-78c7698578-66q9b                 0/1     Running     1 (28s ago)   4m20s   10.0.10.138   ip-10-0-2-131.ec2.internal    <none>           <none>
management-ui-6957fc5669-hhf5c                          2/2     Running     0             4m19s   10.0.14.0     ip-10-0-2-131.ec2.internal    <none>           <none>
management-websocket-proxy-7ff95b8d7d-nxf98             1/1     Running     0             4m19s   10.0.2.134    ip-10-0-2-131.ec2.internal    <none>           <none>
portal-0fbf20f0-db-0                                    2/2     Running     0             12m     10.0.8.138    ip-10-0-2-131.ec2.internal    <none>           <none>
portal-0fbf20f0-www-0                                   2/2     Running     0             12m     10.0.21.129   ip-10-0-28-124.ec2.internal   <none>           <none>
portal-nginx-0                                          1/1     Running     0             12m     10.0.19.156   ip-10-0-28-124.ec2.internal   <none>           <none>
portal-system-check-29552715-x59hs                      0/1     Completed   0             3m24s   10.0.8.109    ip-10-0-2-131.ec2.internal    <none>           <none>
 ```

 ### 3.4 Access the APIC 

If you access the APIC admin UI or any other subsytems they are running properly.