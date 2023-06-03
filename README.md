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



Then to make the PVCs permanent....

> kubectl patch pv pvc-500ef990-6a50-4751-9aaa-6080d6a64163 -p '{"spec":{"persistentVolumeReclaimPolicy":"Retain"}}'

> kubectl patch pv pvc-ca79c437-37f7-40f4-85e4-6d1ce0027298 -p '{"spec":{"persistentVolumeReclaimPolicy":"Retain"}}'

And edit values.yaml file to add Existing PVCs.

Line 233 - existingClaim: "otk-website-drupal-drupal"

Line 613 - existingClaim: "data-otk-website-mariadb-0"

Then DELETE helm and REINSTALL

> helm install otk-website ./drupal-14.1.2.tgz -f values.yaml

BUT THIS WILL REBUILD IF NEEDED.