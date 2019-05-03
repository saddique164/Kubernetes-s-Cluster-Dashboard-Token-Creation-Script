# Kubernetes-s-Cluster-Dashboard-Token-Creation-Script
# I have created total three CentOs 7.5 VMs. All of them have kubernetes services and Docker-ce installed. I chose one node as a master.
# here are some important tips for avoiding errors.

1. Don't add nodes in the master untill you have installed flannel and Dashboard on it. Otherwise It will throw terminating and Breakoff errors.
 
 i. for deploying Dashboard on with kubernetes, run following command on your terminal.
      kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
 wait it for comming up in the running state.
 
 2. After Fetching Dashboard containers on master nodes and letting it in running condition, Run (kubectl proxy)  command on your terminal and it will give you 
   a localhost:8001 string. This string is being used by following link to open dashboard.
   
   http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
   
   Note: This link only open inside the VM which is hosting the cluster. You can not open dashbaord on directly on your window unless you configure
         ingress or change scripts But I used another method to open it in my window.
        
         i. I installed MobaXterm instead of putty as it gives GUI when you try to open firefox or any other software template
         ii.  When you open it, you will find two ways to login: Kubernetes config or Token.
         iii. Just use token login
         iv. run my script using this command
           
         kubectl create -f user-dasgboard.yml
         
   3. for verification run this command.
       
        kubectl get sa saddique  -n kube-system
        
        The output will be:
          
           NAME       SECRETS   AGE
           saddique   1         10h 
   4.   Run this for more detail:
        kubectl describe sa saddique  -n kube-system
        
        The output will be:
        
         Name:                saddique
         Namespace:           kube-system
          Labels:              <none>
          Annotations:         <none>
          Image pull secrets:  <none>
          Mountable secrets:   saddique-token-nc26q
          Tokens:              saddique-token-nc26q
           Events:              <none>
           
   5. in last, run this command to get the token for signing in.
   
   kubectl describe secret  saddique-token-nc26q -n kube-system
   
    The output br like:
    
    Name:         saddique-token-nc26q
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: saddique
              kubernetes.io/service-account.uid: c4d38f36-6d89-11e9-be81-000c294f50e3

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJzYWRkaXF1ZS10b2tlbi1uYzI2cSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJzYWRkaXF1ZSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImM0ZDM4ZjM2LTZkODktMTFlOS1iZTgxLam5vbcpu6zpqGdxJSdRffbYQxp_cpxaaZvkcOUM68OKAL717n7fUGP2RtBYKN8YkW3DGorqgDbBipfl3ssOu9JsgxEbOQBvzdwvLhDZSQO6fdkiA5dFGgZZRYwGxPO-3Crvx-kHWjBNF6E9V4lf3DOqXKTjRW8P7D4GQ41MjpiduSPD8riE9-i3Sd2Yhx2i0BZQN4xvO7U6mNMHB3NE0

Use this token to loging in.

I hope this will resolve your issue.
