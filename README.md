# helm-charts

Helm charts for appbase.io, an API gateway for a supercharged Elasticsearch experience.
Via this Helm Chart you can localize Appbaseio on your kubernetes engine.

## Requirements
You should have your **Kubernetes** cluster installed and configured and then you should install [helm]("https://helm.sh/docs/intro/install/")

As Arc is an API gateway for your Elasticsearch, make sure that you already have an Elasticsearch cluster with it's basic credentials.

If you don't have an Elasticsearch cluster, you can use this [guid]("https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-quickstart.html")

## Quick start
1- Clone this repositoy: 

`git clone git@github.com:appbaseio/helm-charts.git` 

and then head to `helm-charts` folder.


2- Now you should config helm chart to connect appbaseio to your elasticsearch: 

open **values.yaml** from `helm-charts/appbaseio/values.yaml` and fill the values you might need.
These are required values you must fill: 

- **Elasticsearch Domain**: This can be your Kubernetes service name or a valid domain which is reachble from your kubernetese cluster
- **Elasticsearch Username**: by default this username is `elastic` but if you have changed your ES_USERNAME, should change it here
- **Elasticsearch Password**: this is your decrypted password which you can leave empty here as we can change it while installing our helm chart. we will see that in following.
- **APPBASE_ID**: to get this, head to [Appbase]("https://arc-dashboard.appbase.io/install") and enter your email, You will receive an OTP on an entered email address. Enter OTP to verify the email address and then `APPBASE_ID` will be sent to our email.

- **APPBASE_USERNAME**: This will be used while using appbaseio so you can choose your desired username by filling the variable
- **APPBASE_PASSWORD**: This will be used while using appbaseio so you can choose your desired password by filling the variable

you can leave other settings as their default value but if you want more setting like use your own volume as storage, use SSL certificate for your Appbase cluster and so on, modify the related part.

**Note:**
- handle arc version in Chart.yaml as **appVersion**

3- We can set above values while running helm chart install command, the value will be replaced with what we have in values.yaml for example here we install appbase chart with setting the ES_PASSWORD 

- If you have a Kubernetes cluster, run below command :

`PASSWORD=$(kubectl get secret <your elasticsearch service name> -o go-template='{{.data.elastic | base64decode}}')`
    
    This Command will get ES_PASSWORD from ES secret, make sure to change your elasticsearch service name

Now it's time to install our chart: 

`helm install appbaseio appbaseio/ --values appbaseio/values.yaml --set elasticsearch.esPassword=$PASSWORD `

To get more information about set value in helm install, take a look at this [page]("https://helm.sh/docs/helm/helm_install/")
4- Wait until your pods are in "Running" stat (see their status by `kubectl get pods`) and the LoadBalancer service is up and have an external IP (use this command: `kubectl get svc --namespace ingress-nginx`) 

When everything is OK you can access your Appbase with LoadBalancer IP and it's Port then you can go to [Arc dashboard]("https://arc-dashboard.appbase.io/login") to handle your elasticsearch visually
