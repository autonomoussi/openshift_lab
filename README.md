# Red Hat OpenShift:

## OpenShift Local Installation on Mac M1:

- [Local Installation](https://blogbypuneeth.medium.com/install-redhat-openshift-local-on-mac-m1-c44bf4639692) instructions in the mentioned link are pretty clear and are upto date to get the local installed.

### Accessing the Web Console:

- Web Console can be accessed at `https://console-openshift-console.apps-crc.testing`.  Admin and developer login credentials are provided as part of the `crc start` output. 

### Using the `oc` Command:

- To use the `oc` command run:

```sh
eval $(crc oc-env)
```

- To log in as a developer from the terminal, use:

```sh
oc login -u developer -p developer https://api.crc.testing:6443
```
The default password for developer is `developer`

### Openshift Local Public Registry

The public registry is located at: `default-route-openshift-image-registry.apps-crc.testing`.

## Terminal Instructions to Create a Project in Cluster

1. Start the cluster:

```sh
crc start
```
this might take upto 5 mins

2. Set up the `oc` environment:

```sh
eval $(crc oc-env)
```

3. Login as developer:

```sh
oc login -u developer -p developer https://api.crc.testing:6443
```

4. Make developer the cluster admin:

```sh
oc adm policy add-cluster-role-to-user cluster-admin developer
```

5. Create a new project:

```sh
oc new-project <project-name>
```

6. Verify you are in newly created project:

```sh
oc projects
```
Command displays list of projects with current project highlighted.

7. Retrieve the CLI SHA token from the OpenShift web console:
   * After logging in as a developer, click on the developer profile icon.
   * Select "Command Line Tools" and retrieve the SHA token or simply do `oc whoami -t` to get token.


## Deploying a Local Docker Image to OpenShift

1. Login to Docker using SHA token as password:

```sh
docker login -u developer -p $(oc whoami -t) default-route-openshift-image-registry.apps-crc.testing
```

2. Create an ImageStream in the namespace/project:

```sh
oc create is <image-name> -n <project-name>
```

3. Tag the Docker image:

```sh
docker tag <image-name>:<tag> default-route-openshift-image-registry.apps-crc.testing/<project-name>/<image-name>:<tag>
eg: docker tag 076052596660.dkr.ecr.us-east-1.amazonaws.com/rover-micro:1.0.6 default-route-openshift-image-registry.apps-crc.testing/rover-project/rover-micro:1.0.6
```

4. Push the tagged docker image to openshift registry:

```sh
docker push default-route-openshift-image-registry.apps-crc.testing/rover-project/rover-test:1.0.6
```

Above steps will create an image stream with the specified tag under the given namespace/
project.


## RUNNING DOCKER IMAGE AS POD / PERFORMING A DEPLOYMENT:

- refer to `SCC-SA.md` in the current directory.

## CREATING PERSISTENT VOLUMES

- refer to `PV-PCC.md` in the current directory.

## EXPOSING ROVER TO OUTSIDE WORLD

- refer to `SERVICE-ROUTE.md` in the current directory.
