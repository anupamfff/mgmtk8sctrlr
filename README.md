### Assumptions and Prerequisites
1. You are deploying mgmtk8sctrlr in the "infiotmgmtproxy" namespace for the MSP tenant "acme". 
2. You have already created "infiotmgmtproxy" namespace in your k8s cluster.
The MSP tenant portal is live.
3. You have created a node-pool dedicated for hosting Infiot's mgmtproxy gateway with following labels and taints
```
Taints:             infiot.com/mgmtproxy=mgmtgw-nodes:NoExecute
                    infiot.com/mgmtproxy=mgmtgw-nodes:NoSchedule
Labels:             infiot.com/mgmtproxy=mgmtgw-nodes
```

### Deploying Infiot's mgmtk8sctrlr

1. Login into the MSP tenant (https://acme.infiot.net) as system administrator and create a token with following privileges.
```
[
  {
    "rap_resource": "",
    "rap_privs": [
      "privTenantRead",
      "privTenantMSP",
      "privSiteCreate",
      "privSiteRead",
      "privSiteWrite",
      "privSiteDelete",
      "privSiteToken",
      "privSiteRestart",
      "privSiteOpsRead",
      "privSiteOpsWrite",
      "privSiteName",
      "privAuditRecordCreate"
    ]
  }
]
```

2. Create or update the secret using following command sequence
```
kubectl -n infiotmgmtproxy \
    create secret generic store-mgmtk8sctrlr \
    --from-literal=token=<SECRET_FROM_STEP_1>
```

3. Deploy the Traefik load balancer in the "infiotmgmtproxy" namespace

```
kubectl -n infiotmgmtproxy apply -f https://raw.githubusercontent.com/anupamfff/mgmtk8sctrlr/master/traefik-crd.yaml

kubectl -n infiotmgmtproxy apply -f https://raw.githubusercontent.com/anupamfff/mgmtk8sctrlr/master/traefik-deployment.yaml

```
