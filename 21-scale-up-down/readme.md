# Scale up and down

This document explains about what happens when you scale up and down.


## 1. Before Scale up/down

The pods and status in the existing setup.

 ```
gandhi@Jeyas-MacBook-Pro files % kubectl get pods -n apiconnect
NAME                                                    READY   STATUS      RESTARTS   AGE
analytics-director-6dd585d56c-6fzf2                     1/1     Running     0          4h21m
analytics-ingestion-0                                   1/1     Running     0          39h
analytics-mtls-gw-6477f4564f-4k48g                      1/1     Running     0          4h21m
analytics-oscron-29550870-zqjwq                         0/1     Completed   0          12m
analytics-osinit-f48gp                                  0/1     Completed   0          39h
analytics-storage-0                                     2/2     Running     0          39h
analytics-system-check-29550850-tgfsb                   0/1     Completed   0          32m
datapower-operator-85656f778c-cp8ps                     1/1     Running     0          4h21m
datapower-operator-conversion-webhook-dc9444944-j4m22   1/1     Running     0          2d19h
edb-operator-5bdd897d44-kb4n6                           1/1     Running     0          4h21m
gwv6-0                                                  1/1     Running     0          4h21m
gwv6-system-check-29550860-w6fc9                        0/1     Completed   0          22m
ibm-apiconnect-69699bfd99-9wmrb                         1/1     Running     0          2d4h
management-8913d321-db-1                                1/1     Running     0          2d1h
management-analytics-proxy-75ccdb88c-2stnz              1/1     Running     0          4h21m
management-analytics-push-29550855-hzqq8                0/1     Completed   0          27m
management-analytics-ui-7f8bb65764-swkxj                1/1     Running     0          4h21m
management-apim-74b85ccffb-8dprw                        1/1     Running     0          4h21m
management-audit-logging-cd49969d7-qfmbn                1/1     Running     0          4h21m
management-client-downloads-server-fb54c7598-dsszb      1/1     Running     0          2d1h
management-consumer-catalog-55bdf895f7-sfzrp            1/1     Running     0          4h21m
management-juhu-7485bff6dd-7lsb6                        1/1     Running     0          4h21m
management-ldap-768d797b98-tq4xq                        1/1     Running     0          4h21m
management-lur-78c84798c9-9wpzf                         1/1     Running     0          4h21m
management-natscluster-0                                1/1     Running     0          4h21m
management-portal-proxy-58b9b9d465-h2fqv                1/1     Running     0          4h21m
management-s3proxy-0                                    1/1     Running     0          4h21m
management-system-check-29550845-qqsrc                  0/1     Completed   0          37m
management-taskmanager-78c7698578-sss4d                 1/1     Running     0          40h
management-ui-6957fc5669-ldsz9                          2/2     Running     0          4h21m
management-websocket-proxy-7ff95b8d7d-gvkz7             1/1     Running     0          4h21m
portal-0fbf20f0-db-0                                    2/2     Running     0          39h
portal-0fbf20f0-www-0                                   2/2     Running     0          4h20m
portal-nginx-0                                          1/1     Running     0          4h19m
portal-system-check-29550855-b52dq                      0/1     Completed   0          27m
 ```

## 2. Scale down

### 2.1 Scale down - deployment

Run the below command.
 ```
 kubectl scale deployment --all --replicas=0 -n apiconnect
```

The output of the command would be this. You can see what are all the deployments affected.

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

You can see the pods status as of now.
```
gandhi@Jeyas-MacBook-Pro files % kubectl get pods -n apiconnect
NAME                                       READY   STATUS      RESTARTS   AGE
analytics-ingestion-0                      1/1     Running     0          40h
analytics-oscron-29550900-dddc7            0/1     Completed   0          4m58s
analytics-osinit-f48gp                     0/1     Completed   0          40h
analytics-storage-0                        2/2     Running     0          40h
analytics-system-check-29550850-tgfsb      0/1     Completed   0          54m
gwv6-0                                     1/1     Running     0          4h43m
gwv6-system-check-29550860-w6fc9           0/1     Completed   0          44m
management-8913d321-db-1                   1/1     Running     0          2d2h
management-analytics-push-29550855-hzqq8   0/1     Completed   0          49m
management-natscluster-0                   1/1     Running     0          4h43m
management-s3proxy-0                       1/1     Running     0          4h43m
management-system-check-29550845-qqsrc     0/1     Completed   0          59m
portal-0fbf20f0-db-0                       2/2     Running     0          40h
portal-0fbf20f0-www-0                      2/2     Running     0          4h43m
portal-nginx-0                             1/1     Running     0          4h42m
portal-system-check-29550855-b52dq         0/1     Completed   0          49m
```

### 2.2 Scale down - statefulset

Run the below command.
 ```
 kubectl scale statefulset --all --replicas=0 -n apiconnect
```

The output of the command would be this. You can see what are all the statefulset affected.

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


You can see the pods status as of now.
```
gandhi@Jeyas-MacBook-Pro files % kubectl get pods -n apiconnect
NAME                                       READY   STATUS      RESTARTS   AGE
analytics-oscron-29550900-dddc7            0/1     Completed   0          7m45s
analytics-osinit-f48gp                     0/1     Completed   0          40h
analytics-system-check-29550850-tgfsb      0/1     Completed   0          57m
gwv6-system-check-29550860-w6fc9           0/1     Completed   0          47m
management-8913d321-db-1                   1/1     Running     0          2d2h
management-analytics-push-29550855-hzqq8   0/1     Completed   0          52m
management-system-check-29550905-jwndk     0/1     Completed   0          2m45s
portal-system-check-29550855-b52dq         0/1     Completed   0          52m
```

You can see the pods status after 15 minutes.
```
gandhi@Jeyas-MacBook-Pro files % kubectl get pods -n apiconnect
NAME                                       READY   STATUS      RESTARTS      AGE
analytics-oscron-29550900-dddc7            0/1     Completed   0             15m
analytics-oscron-29550915-qpbw5            0/1     Error       2 (25s ago)   41s
analytics-osinit-f48gp                     0/1     Completed   0             40h
analytics-system-check-29550910-475xs      0/1     Completed   0             5m41s
gwv6-system-check-29550860-w6fc9           0/1     Completed   0             55m
management-8913d321-db-1                   1/1     Running     0             2d2h
management-analytics-push-29550855-hzqq8   0/1     Completed   0             60m
management-analytics-push-29550915-8nfzj   0/1     Completed   0             41s
management-system-check-29550905-jwndk     0/1     Completed   0             10m
portal-system-check-29550915-nhm5w         0/1     Completed   0             41s
```


## 3. Scale up

### 3.1 Scale up - deployment

Run the below command.
 ```
kubectl scale deployment --all --replicas=1 -n apiconnect
```

The output of the command would be this. You can see what are all the deployments affected.

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

You can see the pods status as of now.
```
gandhi@Jeyas-MacBook-Pro files % kubectl get pods -n apiconnect
NAME                                                    READY   STATUS      RESTARTS   AGE
analytics-director-6dd585d56c-4n9x2                     1/1     Running     0          2m43s
analytics-mtls-gw-6477f4564f-cgl66                      1/1     Running     0          2m43s
analytics-oscron-29550900-dddc7                         0/1     Completed   0          24m
analytics-osinit-f48gp                                  0/1     Completed   0          40h
analytics-system-check-29550910-475xs                   0/1     Completed   0          14m
datapower-operator-85656f778c-fpzbv                     1/1     Running     0          2m43s
datapower-operator-conversion-webhook-dc9444944-m4tt9   1/1     Running     0          2m42s
edb-operator-5bdd897d44-z8nr5                           1/1     Running     0          2m42s
gwv6-0                                                  1/1     Running     0          2m39s
gwv6-system-check-29550920-f84gj                        0/1     Completed   0          4m16s
ibm-apiconnect-69699bfd99-wtvrx                         1/1     Running     0          2m42s
management-8913d321-db-1                                1/1     Running     0          2d2h
management-analytics-proxy-75ccdb88c-s2pdf              1/1     Running     0          2m41s
management-analytics-push-29550915-8nfzj                0/1     Completed   0          9m16s
management-analytics-ui-7f8bb65764-r8fbb                1/1     Running     0          2m41s
management-apim-74b85ccffb-nqkg6                        1/1     Running     0          2m41s
management-audit-logging-cd49969d7-nl464                1/1     Running     0          2m40s
management-client-downloads-server-fb54c7598-rzlcc      1/1     Running     0          2m40s
management-consumer-catalog-55bdf895f7-f6kx8            1/1     Running     0          2m40s
management-juhu-7485bff6dd-rd6dr                        1/1     Running     0          2m39s
management-ldap-768d797b98-24vr9                        1/1     Running     0          2m39s
management-lur-78c84798c9-2ch2n                         1/1     Running     0          2m39s
management-portal-proxy-58b9b9d465-w8zbq                1/1     Running     0          2m38s
management-system-check-29550905-jwndk                  0/1     Completed   0          19m
management-taskmanager-78c7698578-j7hqg                 1/1     Running     0          2m38s
management-ui-6957fc5669-bvxrl                          2/2     Running     0          2m37s
management-websocket-proxy-7ff95b8d7d-mxx8c             1/1     Running     0          2m37s
portal-system-check-29550915-nhm5w                      0/1     Completed   0          9m16s
```

## 3.2 Scale up - statefulset

Run the below command.
 ```
 kubectl scale statefulset --all --replicas=1 -n apiconnect
```

The output of the command would be this. You can see what are all the statefulset affected.

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

You can see the pods status as of now.
```
gandhi@Jeyas-MacBook-Pro files % kubectl get pods -n apiconnect
NAME                                                    READY   STATUS      RESTARTS   AGE
analytics-director-6dd585d56c-4n9x2                     1/1     Running     0          5m21s
analytics-ingestion-0                                   0/1     Running     0          91s
analytics-mtls-gw-6477f4564f-cgl66                      1/1     Running     0          5m21s
analytics-oscron-29550900-dddc7                         0/1     Completed   0          26m
analytics-osinit-f48gp                                  0/1     Completed   0          40h
analytics-storage-0                                     2/2     Running     0          90s
analytics-system-check-29550910-475xs                   0/1     Completed   0          16m
datapower-operator-85656f778c-fpzbv                     1/1     Running     0          5m21s
datapower-operator-conversion-webhook-dc9444944-m4tt9   1/1     Running     0          5m20s
edb-operator-5bdd897d44-z8nr5                           1/1     Running     0          5m20s
gwv6-0                                                  1/1     Running     0          5m17s
gwv6-system-check-29550920-f84gj                        0/1     Completed   0          6m54s
ibm-apiconnect-69699bfd99-wtvrx                         1/1     Running     0          5m20s
management-8913d321-db-1                                1/1     Running     0          2d2h
management-analytics-proxy-75ccdb88c-s2pdf              1/1     Running     0          5m19s
management-analytics-push-29550915-8nfzj                0/1     Completed   0          11m
management-analytics-ui-7f8bb65764-r8fbb                1/1     Running     0          5m19s
management-apim-74b85ccffb-nqkg6                        1/1     Running     0          5m19s
management-audit-logging-cd49969d7-nl464                1/1     Running     0          5m18s
management-client-downloads-server-fb54c7598-rzlcc      1/1     Running     0          5m18s
management-consumer-catalog-55bdf895f7-f6kx8            1/1     Running     0          5m18s
management-juhu-7485bff6dd-rd6dr                        1/1     Running     0          5m17s
management-ldap-768d797b98-24vr9                        1/1     Running     0          5m17s
management-lur-78c84798c9-2ch2n                         1/1     Running     0          5m17s
management-natscluster-0                                1/1     Running     0          90s
management-portal-proxy-58b9b9d465-w8zbq                1/1     Running     0          5m16s
management-s3proxy-0                                    1/1     Running     0          89s
management-system-check-29550905-jwndk                  0/1     Completed   0          21m
management-taskmanager-78c7698578-j7hqg                 1/1     Running     0          5m16s
management-ui-6957fc5669-bvxrl                          2/2     Running     0          5m15s
management-websocket-proxy-7ff95b8d7d-mxx8c             1/1     Running     0          5m15s
portal-0fbf20f0-db-0                                    2/2     Running     0          89s
portal-0fbf20f0-www-0                                   2/2     Running     0          89s
portal-nginx-0                                          1/1     Running     0          88s
portal-system-check-29550915-nhm5w                      0/1     Completed   0          11m
```

You can see the pods status after 15 minutes.
```
andhi@Jeyas-MacBook-Pro files % kubectl get pods -n apiconnect
NAME                                                    READY   STATUS      RESTARTS   AGE
analytics-director-6dd585d56c-4n9x2                     1/1     Running     0          23m
analytics-ingestion-0                                   1/1     Running     0          19m
analytics-mtls-gw-6477f4564f-cgl66                      1/1     Running     0          23m
analytics-oscron-29550930-9kh9x                         0/1     Completed   0          14m
analytics-osinit-f48gp                                  0/1     Completed   0          40h
analytics-storage-0                                     2/2     Running     0          19m
analytics-system-check-29550910-475xs                   0/1     Completed   0          34m
datapower-operator-85656f778c-fpzbv                     1/1     Running     0          23m
datapower-operator-conversion-webhook-dc9444944-m4tt9   1/1     Running     0          23m
edb-operator-5bdd897d44-z8nr5                           1/1     Running     0          23m
gwv6-0                                                  1/1     Running     0          23m
gwv6-system-check-29550920-f84gj                        0/1     Completed   0          24m
ibm-apiconnect-69699bfd99-wtvrx                         1/1     Running     0          23m
management-8913d321-db-1                                1/1     Running     0          2d2h
management-analytics-proxy-75ccdb88c-s2pdf              1/1     Running     0          23m
management-analytics-push-29550915-8nfzj                0/1     Completed   0          29m
management-analytics-ui-7f8bb65764-r8fbb                1/1     Running     0          23m
management-apim-74b85ccffb-nqkg6                        1/1     Running     0          23m
management-audit-logging-cd49969d7-nl464                1/1     Running     0          23m
management-client-downloads-server-fb54c7598-rzlcc      1/1     Running     0          23m
management-consumer-catalog-55bdf895f7-f6kx8            1/1     Running     0          23m
management-juhu-7485bff6dd-rd6dr                        1/1     Running     0          23m
management-ldap-768d797b98-24vr9                        1/1     Running     0          23m
management-lur-78c84798c9-2ch2n                         1/1     Running     0          23m
management-natscluster-0                                1/1     Running     0          19m
management-portal-proxy-58b9b9d465-w8zbq                1/1     Running     0          23m
management-s3proxy-0                                    1/1     Running     0          19m
management-system-check-29550905-jwndk                  0/1     Completed   0          39m
management-taskmanager-78c7698578-j7hqg                 1/1     Running     0          23m
management-ui-6957fc5669-bvxrl                          2/2     Running     0          23m
management-websocket-proxy-7ff95b8d7d-mxx8c             1/1     Running     0          23m
portal-0fbf20f0-db-0                                    2/2     Running     0          19m
portal-0fbf20f0-www-0                                   2/2     Running     0          19m
portal-nginx-0                                          1/1     Running     0          19m
portal-system-check-29550915-nhm5w                      0/1     Completed   0          29m
```