## 4. Deploy Pixie 🚀
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

Pixie will deploy pods to the pl, plc, px-operator, and olm(if deploying the OLM) namespaces.

### More Deploy Options
For more deploy options that you can specify to configure Pixie, refer to our deploy options.
