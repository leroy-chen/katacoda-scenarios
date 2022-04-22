## 2. Deploy Pixie Cloud
Pixie also offers a free account with Pixie Community Cloud to make getting started even easier and faster. To get Pixie Cloud, check out the community cloud Install Guide.
There is a known issue with login on self-managed Pixie Cloud on Safari and Firefox. For now, use Chrome.

### Clone the Pixie repo.

`git clone https://github.com/pixie-io/pixie.git
cd pixie`{{execute}}


### Install mkcert. 
Pixie uses SSL to securely communicate between Pixie Cloud and the UI. Self-managed Pixie Cloud requires managing your own certificates. mkcert is a simple tool to create and install a local certificate authority (CA) in the system root store in order to generate locally-trusted certificates.

`apt install libnss3-tools -y
cd ~ && git clone https://github.com/FiloSottile/mkcert && cd mkcert
go build -ldflags "-X main.Version=$(git describe --tags)"
export PATH=$PATH:/root/mkcert
`{{execute}}

### Start mkcert. 
This command will set up local CA and create a root certificate that Chrome and your CLI will now trust. To access Pixie Cloud from different machine that the one it was set up on, you will need to install this certificate there as well.

`cd ~/pixie && mkcert -install`{{execute}}

### Create the plc namespace. 
This namespace is not currently configurable. Several of the install scripts expect Pixie Cloud to be deployed to the plc namespace.

`kubectl create namespace plc`{{execute}}

### Create the Pixie Cloud secrets. 
From the top level pixie/ directory, run:

`cd ~/pixie && ./scripts/create_cloud_secrets.sh`{{execute}}

### Install kustomize.
use binary install 

`cd ~/ && curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash
export PATH=$PATH:/root/`{{execute}}

use source install( need go 1.16+)

`GOBIN=$(pwd)/ GO111MODULE=on go get sigs.k8s.io/kustomize/kustomize/v4`{{execute}}

### Deploy Pixie Cloud dependencies.
 wait for all pods within the plc namespace to become ready and available before proceeding to the next step. If there is an error, you may need to retry this step.

`kustomize build k8s/cloud_deps/base/elastic/operator | kubectl apply -f -
kustomize build k8s/cloud_deps/public | kubectl apply -f -`{{execute}}

### Deploy Pixie Cloud.

`kustomize build k8s/cloud/public/ | kubectl apply -f -`{{execute}}

Wait for all pods within the plc namespace to become ready and available. Note that you may have one or more create-hydra-client-job pod errors, but as long as long as another instance of that pod successfully completes, that is ok.

`kubectl get pods -n plc`{{execute}}

## Set up DNS
Ensure that the cloud-proxy-service and vzconn-service LoadBalancer services have External IPs assigned. If you are running Pixie Cloud on minikube, you likely need to run minikube tunnel before continuing with this setup.
minikube tunnel # if running on minikube

`kubectl get service cloud-proxy-service -n plc
kubectl get service vzconn-service -n plc`{{execute}}

Setup your DNS. This produces a dev_dns_updater binary in the top level pixie directory.

`cd ~/pixie && go build src/utils/dev_dns_updater/dev_dns_updater.go`{{execute}}

You'll need to hardcode in your kube config. Leave this tab open.

`./dev_dns_updater --domain-name="dev.withpixie.dev"  --kubeconfig=$HOME/.kube/config --n=plc`{{execute}}

Navigate to dev.withpixie.dev in your browser. Make sure that the network you are on can access your cluster.

## Authentication using Kratos / Hydra
Self-managed Pixie Cloud only supports one organization.

To setup the default admin account, check the logs for the create-admin-job pod by running:
`kubectl logs create-admin-job-<pod_string> -n plc`{{execute}}

Open the URL from the pod's logs to set the password for the admin@default.com user.
If you've visited dev.withpixie.dev before, make sure to clear the cookies for this site or you'll get a login error.
Once the password has been set, login using admin@default.com for the identifier and your new password.
There is a known issue with login on self-managed Pixie Cloud on Safari and Firefox. For now, use Chrome.
Invite others to your organization (optional)
Add users to your organization to share access to Pixie Live Views, query running clusters, and deploy new Pixie clusters. For instructions, see the User Management & Sharing reference docs.
