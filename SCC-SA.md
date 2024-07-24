# Running Deployment with Custom SCC and SA

## Introduction

In OpenShift, Security Context Constraints (SCCs) control the permissions and capabilities granted to pods. By default, OpenShift uses the `default` Service Account (SA) for deployments. However, sometimes you need to use custom SCCs and SAs to meet specific security requirements or application needs.

## Rover UID and GID:

- UID: `1002`
- GID: `1000`

## Default SCCs and SAs:

- OpenShift local defines 8 SCCs and 3 SAs per project/namespace
- If a custom service account is not provided, OpenShift uses the `default` service account.

## Challenge

The goal is to run a deployment with the user ID `1002` which is different from the default ID assigned by OpenShift (random UIDs assigned from a range of IDs allocated to Pod at runtime by default).
Rover requires specific UID and GID to start successfully.

## Solution

To achieve this, follow these steps:

1. Create a Custom SCC:

- Create a custom SCC that allows the necessary permissions, SCC for rover is defined in file: `file-path-goes-here` and to create this SCC run below command from CLI:

```sh
oc create -f example_objects/rover-scc.yaml
```

2. Create a Custom Service Account

- Create a new SA in your project/namespace that is to be used for deployment:

```sh
oc create sa <service-acc-name>
eg: oc create sa rover-sa
```

3. Assign the SCC to the Service Account

Associate the custom SCC with newly create service account:

```sh
oc adm policy add-scc-to-user <custom-scc-name> -z <custom-sa-name> -n <project-name>
eg: oc adm policy add-scc-to-user rover-scc -z rover-sa -n rover-project
```
Replace placeholders with custom scc, sa and project names respectively.

4. Run the Deployment

Deploy your application using the custom service account and with the following command:

```sh
oc create -f example_objects/rover-deployment.yaml
```

## REFERENCES:

- [UID-SCC-SA-DEPLOY](https://www.redhat.com/en/blog/a-guide-to-openshift-and-uids)

## NOTE:

- In order to create SCC, SA and even attaching policy one has to have necessary permissions/privileges
in this case user `developer` is the cluster owner see `README.md`

- It is generally advisable to do deployments with SAs instead of creating PODs, pod will be created as part of deployment.
