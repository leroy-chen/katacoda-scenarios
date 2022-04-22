Pixie's CLI is the fastest and easiest way to deploy Pixie. You can also deploy Pixie using YAML or Helm.

To deploy Pixie using the CLI:

If your cluster already has Operator Lifecycle Manager (OLM) deployed, deploy Pixie using the `--deploy_olm=false` flag.
Please refer to Environment-Specific Configurations for other configurations that should be set for your specific Kubernetes environment.
`# Deploy the Pixie Platform in your K8s cluster (No OLM present on cluster).
px deploy --dev_cloud_namespace plc`{{execute}}

`# Deploy the Pixie Platform in your K8s cluster (OLM already exists on cluster).
px deploy  --dev_cloud_namespace plc --deploy_olm=false`{{execute}}

`# Deploy Pixie with a specific memory limit (2Gi is the default, 1Gi is the minimum recommended)
px deploy --dev_cloud_namespace plc --pem_memory_limit=1Gi`{{execute}}


Pixie deploys the following pods to your cluster. Note that the number of vizier-pem pods correlates with the number of nodes in your cluster, so your deployment may contain more PEM pods.

NAMESPACE           NAME
olm                 catalog-operator
olm                 olm-operator
pl                  kelvin
pl                  nats-operator
pl                  pl-nats-1
pl                  vizier-certmgr
pl                  vizier-cloud-connector
pl                  vizier-metadata
pl                  vizier-pem
pl                  vizier-pem
pl                  vizier-proxy
pl                  vizier-query-broker
px-operator         77003c9dbf251055f0bb3e36308fe05d818164208a466a15d27acfddeejt7tq
px-operator         pixie-operator-index
px-operator         vizier-operator
To deploy Pixie to another cluster, change your kubectl config current-context to point to that cluster. Then repeat the same deploy commands shown in this step.

More Deploy Options

### More Deploy Options
For more deploy options that you can specify to configure Pixie, refer to our deploy options.
