# splunk_ezmeral
Specifications used to deploy a Splunk Cluster on the Ezmeral Container Platform using the splunk operator
- OpenEBS-hostpath is used as the Persistent Volume provider
- Istio is used to handle ingress

To launch the Splunk Cluster:
- Launch the operator (kubectl apply -f https://github.com/splunk/splunk-operator/releases/download/1.0.1/splunk-operator-install.yaml)
- Launch the splunk instance (kubectl apply -f ./deploy-splunk.yml)
