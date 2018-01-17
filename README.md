# linkerd

we have a cluster

```
kubectl get nodes
NAME                                          STATUS    ROLES     AGE       VERSION
ip-172-20-40-82.us-west-1.compute.internal    Ready     node      2d        v1.7.11
ip-172-20-52-199.us-west-1.compute.internal   Ready     master    2d        v1.7.11
ip-172-20-78-34.us-west-1.compute.internal    Ready     node      2d        v1.7.11
```

step 1 ) install linkerd

```
kubectl apply -f linkerd.yaml
configmap "l5d-config" created
daemonset "l5d" created
service "l5d" created

# in linkerd.yaml
  /srv        => /#/io.l5d.k8s/default/http;

above line means connect to services that have the name http which is defined in hello-world.yaml

ports:
- name: http
port: 7777


INGRESS_LB=$(kubectl get svc l5d -o jsonpath="{.status.loadBalancer.ingress[0].*}")
echo http://$INGRESS_LB:9990

# connect to above port for linkerd dashboard

```

step 2 ) create hello-world

```
kubectl create -f hello-world.yaml

try this command
http_proxy=$INGRESS_LB:4140 curl -s http://hello
http_proxy=$INGRESS_LB:4140 curl -s http://world
```

step 3) vizualization tool for linkerd

```
kubectl create -f linkerd-viz.yaml

VIZ_INGRESS_LB=$(kubectl get svc linkerd-viz -o jsonpath="{.status.loadBalancer.ingress[0].*}")

graphana linkerd vizualization
echo http://$VIZ_INGRESS_LB
```

## look into
# check tracing capability of requests with linkerd
