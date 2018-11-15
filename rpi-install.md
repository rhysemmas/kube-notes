<h1>Installing kubernetes on ARM (RPi)</h1>

Following: https://blog.hypriot.com/post/setup-kubernetes-raspberry-pi-cluster/

<p>
HypriotOS v1.4: https://github.com/hypriot/image-builder-rpi/releases/tag/v1.4.0
</p>

Gives us older docker:<br/>
`Docker version 17.03.0-ce, build 60ccb22`

Install kubernetes v1.9.7:<br/>
`$ apt-get install kubeadm=1.9.7-00 kubectl=1.9.7-00 kubelet=1.9.7-00`

Initialise:<br/>
`kubeadm init --pod-network-cidr 10.244.0.0/16`

Once kubeadm enters init stage: `[init] This might take a minute or longer if the control plane images have to be pulled` update kube apiserver manifest file by running next command:<br/>

`sed -i 's/failureThreshold: 8/failureThreshold: 20/g' /etc/kubernetes/manifests/kube-apiserver.yaml`<br/>

Then kill the current kube-apiserver container (`docker kill`).</br>
Wait patiently</br>
