# splunk_ezmeral
Specifications used to deploy a Splunk Cluster on the Ezmeral Container Platform using the splunk operator
- OpenEBS-hostpath is used as the Persistent Volume provider
- Istio is used to handle ingress

To launch the Splunk Cluster:
- Launch the operator (kubectl apply -f https://github.com/splunk/splunk-operator/releases/download/1.0.1/splunk-operator-install.yaml)
- Launch the splunk instance (kubectl apply -f ./deploy-splunk.yml)

__Some handy tips that are not captured in the deployment manifest:__

Create a Splunk app to encode the region associated with the S3 storage:
    tar -cvzf smartstore.tgz smartstore
    kubectl create cm smartstore-app --from-file=./smartstore.tgz

Create a configmap for licenses (supports stacking):
- kubectl create configmap splunk-licenses --from-file=./Splunk1.license --from-file=./Splunk2.license

To retrieve the Splunk admin password:
- kubectl get secret splunk-morgan-splunk-secret -o jsonpath='{.data.password}'  | base64 --decode

Deploying OpenEBS-hostpath as the PV provider:
- kubectl create ns openebs
- kubectl -n openebs apply -f https://openebs.github.io/charts/openebs-operator-lite.yaml
- kubectl -n openebs apply -f https://openebs.github.io/charts/openebs-lite-sc.yaml

Deploying istio for ingress and patching the services/deployment to exploit hostPorts for DNS RR load balancing:
- curl -L https://istio.io/downloadIstio | sh -
- cd istio-1.10.0
- export PATH=$PWD/bin:$PATH
- istioctl install --set profile=demo -y
- kubectl -n istio-system patch svc istio-ingressgateway --patch "$(cat istio_svc_patch.yaml)"
- kubectl -n istio-system patch deployment/istio-ingressgateway --patch "$(cat istio_deploy_patch.json)"
- Remember to disable mTLS in the namespace running the Splunk cluster, else even basic east-west communications don't work.

Create a secret to encode the access keys of the S3 storage (in our case Scality):
- kubectl create secret generic smart-store-secret --from-literal=s3_access_key=<AccessKey> --from-literal=s3_secret_key=<SecretAccessKey>


