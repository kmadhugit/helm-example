# helm

* install helm
* install tiller => helm init

## check tiller is running 

kubectl get pod --all-namespaces | grep tiller

## create a helm chart

helm create mychart

## Run a helm chart
* helm install --dry-run --debug ./hello-service-chart . 

override new values by passing it . 

* helm install --set appname=apple --set container.port=9000 --set service.port=8000 hello-service-chart

override new values by giving them in a file . 

```
(py3.6) ~/work/kubernetes/helm-chart>cat override_values.yml
appname: orang
container:
  port: 9000
service:
  port: 8000
(py3.6) ~/work/kubernetes/helm-chart>helm install -f override_values.yml hello-service-chart
```

## upgrade a release ( installing a chart creates a release i.e running instane of chart)
(Assuming sweet-chicken is release name auto allocated)
* helm ls ## to get the release name of our chart
* helm get values sweet-chicken
* helm upgrade --set appname=orang --set container.port=9000 --set service.port=8000 sweet-chicken hello-service-chart

## rollback 
view all available release revisions
* helm history sweet-chicken
view the values of particular revision (for example 2).
* helm rollback --dry-run sweet-chicken 2
rollback to revision 2
* helm rollback sweet-chicken 2  
* helm get values sweet-chicken


## packaging

* helm package hello-service-chart # creates hello-service-chart-0.1.0.tgz
* helm install --set appname=apple --set container.port=9000 --set service.port=8000 hello-service-chart-0.1.0.tgz

## publishing

* create a github repo go to settings and enable pages and get the webpage URL. 
* https://kmadhugit.github.io/helm-repository/
* git clone repo
* copy *.tgz to the repository
* helm repo index . --url https://kmadhugit.github.io/helm-repository/
* vi index.yaml
* git commit 
* git push

## add it to helm repo list
* curl https://kmadhugit.github.io/helm-repository/index.yaml
* helm repo add my-helm-repo https://kmadhugit.github.io/helm-repository/
* helm repo list
* helm search hello-service
* helm install my-helm-repo/hello-service-chart


