<h1>Installing kubernetes on ARM (RPi)</h1>

Following: https://blog.hypriot.com/post/setup-kubernetes-raspberry-pi-cluster/

<p>
HypriotOS v1.4: https://github.com/hypriot/image-builder-rpi/releases/tag/v1.4.0
</p>

Gives us older docker:<br/>
`Docker version 17.03.0-ce, build 60ccb22`

Update apt after adding kube repo:<br/>
`$ apt-get update`

Ensure dependency for kubelet is met:<br/>
`$ apt-get install kubernetes-cni=0.6.0-00`

Install kubernetes v1.9.7:<br/>
`$ apt-get install kubeadm=1.9.7-00 kubectl=1.9.7-00 kubelet=1.9.7-00`

Initialise:<br/>
`kubeadm init --pod-network-cidr 10.244.0.0/16`

Once kubeadm enters init stage: `[init] This might take a minute or longer if the control plane images have to be pulled` update kubeapiserver manifest file by running next command:<br/>

`sed -i 's/failureThreshold: 8/failureThreshold: 20/g' /etc/kubernetes/manifests/kube-apiserver.yaml`<br/>

Then kill the current kube-apiserver container (docker kill).</br>
Wait patiently</br>

After service is up and running on cluster, we have an ingress controller in the kube-system namespace that uses the default service account, this account does not have access to our service in the default namespace. So to quickly and lazily give permissions, add the kube-system default service account as a cluser admin:<br/>

`$ kubectl create clusterrolebinding add-on-cluster-admin \`<br/>
`>   --clusterrole=cluster-admin \`<br/>
`>   --serviceaccount=kube-system:default`<br/>
