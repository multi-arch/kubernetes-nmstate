# Prerequisites
- git
- golang
- make
- [dep](https://github.com/nmstate/kubernetes-nmstate.git) - if you want to change any dependencies and run ```make dep```
- docker - if you want to build docker images, or run the dockerized tests
- Should be able to communicate with a Kubernetes cluster for testing. Even if tests are run locally, it is needed, since the client needs to connect to Kubernetes in order to get/set the CRDs. Simple solution for that would be to run with a local [minikube](https://kubernetes.io/docs/setup/minikube/) cluster
- [nmstate](https://nmstate.github.io/) - should be installed on the host to run any tests locally
# Build
First step is to clone this repo: ```git clone https://github.com/nmstate/kubernetes-nmstate.git```
Then run ```make``` to build all binaries. 
Some other ```make``` targets:
 - ```generate``` - call this after changes to the CRDs (```pkg/apis/nmstate.io/v1/types.go```). It will generate yaml files under ```manifests/generated```; generate client/controller code under ```pkg/client```; and generate deep-copy code for the CRD objects. ```clean-generate``` will delete the above files
 - ```dep``` - will update the ```vendors``` directory and ```Gopkg.lock``` file. ```clean-dep``` will delete them
 - ```test``` - will execute integrations tests
 - ```docker``` - will build docker images for all binaries
 - ```docker-push``` will upload them into a repo (by default it would be ```docker.io/nmstate```, so proper permissions will be needed)
 # Project Directory Structure
 ```
├── cmd                   # location of binaries main functions  as well as Dockerfiles
│   ├── policy-handler
│   └── state-handler
├── docs                  # project documentation
├── hack                  # project scripts
├── manifests             # yaml files
│   ├── generated         # generated yaml files - CRD definitions and examples
├── pkg                   # libraries used by the binaries in cmd
│   ├── apis              # CRD definitions
│   ├── client            # generated code for client, informers and listers
│   ├── nmstatectl        # conversion between CRD and nmstatectl input/output
│   ├── policy-controller # main logic of policy handler running as a daemon
│   ├── state-controller  # main logic of state handler running as a daemon
│   └── utils             # general utility functions
├── tools                 # tools for yaml generation
 ```