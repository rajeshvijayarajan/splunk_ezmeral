# splunk_ezmeral
Specifications used to deploy a Splunk Cluster on the Ezmeral Container Platform using the splunk operator
- OpenEBS-hostpath is used as the Persistent Volume provider
- Istio is used to handle ingress

To launch the Splunk Cluster:
- Launch the operator (kubectl apply -f url-of-splunk-operator-yaml-file)
- Launch the splunk instance (kubectl apply -f ./deploy-splunk.yaml)
