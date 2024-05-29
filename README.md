# k8s-essentials

# helpful random command
```

k run mypod --image=nginx 

apt install -y iproute2
ip link show
ip addr add 192.168.0.10/24 dev eth0
ip addr show eth0

k create deployment nginx --image=nginx
k expose deployment ngnix --port=80
k run busybox --rm -it --image=busybox:1.28 -- /bin/bash
now inside container > wget --spider --timeout=1 nginx
k get networkpolicy

k run busybox --rm -it --labels="access=true" --image=busybox:1.28 -- /bin/bash
```

# something on secret

```
echo -n "password123" | base64 -i -
k create secret generic my-secret --from-file=cert=/path/to/cert/file
k create secret generic my-secret --from-literal=API_TOKEN=123
env:
  -name: API_TOKEN
   valueFrom:
      secretKeyRef:
        name: my-secret
        key: API_TOKEN


        or

        valueFrom:
          configMapKeyRef:
            name: test-cm
            key: db-port
```
# check ur existing default token
```
TOKEN=`cat /var/run/secrets/kubernetes.io/token`
curl https://kubernetes -k --header "Authorization: Bearer $TOKEN"
role binding is when u want to attach a role with sa 

curl https://kubernetes/api/v1/namespaces/default/pods -k --header "Authorization: Bearer $TOKEN"
```
# little on role binding 
```
attribute base access control
role based access cotrol
role is namespace based object
k get rolebinding
k auth can-i get pod --as jack
rolebinding yaml will contai always subject containing who want permission like particular user and roleref which will refer to role

role binding we do mostly for pod resource etc and cluster role we do for secret read,list or watch or create etc

autoMountServiceAccountToke :false
k create token mysa

authen+authorization both r needed
so creating sa is like creating. usrename and then attaching toke meaning creating passord to that. so authentication done.
now u eed authorization so create role and role bindinng
then u have to attach sa to pod for that in spec just add serviceaccount ame key and value

kubectl auth can-i get pod --as=system:serviceaccount:default:mysa
```

# taint and toleration
```
taint on node:
k taint nodes controlpane run=mypod:NoSchedule
tolerate on pod:
k run toleration-pod --image=nginx --restart=Never --dry-run -o yaml > pod.yaml
k get pod -o wide ---> this will gine node info as well
spec:
 tolerations:
 - key: "run"
   operator: "Equal"
   value: "mypod"
   effect: "NoSchedule" 

to remove tain:
k taint nodes controlplane run=mypod:NoSchedule- 
```