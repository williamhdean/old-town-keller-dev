# old-town-keller-dev
Development environment for Old Town Keller Website

How to launch:

> helm install otk-website ./drupal-14.1.2.tgz -f values.yaml


NAME: otk-website
LAST DEPLOYED: Sat May 27 07:05:51 2023
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: drupal
CHART VERSION: 14.1.2
APP VERSION: 10.0.9** Please be patient while the chart is being deployed **

1. Get the Drupal URL:

  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        Watch the status with: 'kubectl get svc --namespace default -w otk-website-drupal'

  export SERVICE_IP=$(kubectl get svc --namespace default otk-website-drupal --template "{{ range (index .status.loadBalancer.ingress 0) }}{{ . }}{{ end }}")
  echo "Drupal URL: http://$SERVICE_IP/"

2. Get your Drupal login credentials by running:

  echo Username: admin
  echo Password: $(kubectl get secret --namespace default otk-website-drupal -o jsonpath="{.data.drupal-password}" | base64 -d)