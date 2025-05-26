sudo su
apt update -y
apt install docker.io -y
systemctl start docker
systemctl enable docker
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
apt update -y  
apt install kubeadm=1.20.0-00 kubectl=1.20.0-00 kubelet=1.20.0-00 -y

=== yaha master node pe ham jab kubernets ko initialize krte he to 4 componentes aa jyenge = (controler manzer, scheduler, API server, etcd) is me actual servies aye gi (contener nhi aye ga)
kubeadm init



sudo kubeadm init

=======  Set Up Local kubeconfig:

mkdir -p "$HOME"/.kube
sudo cp -i /etc/kubernetes/admin.conf "$HOME"/.kube/config
sudo chown "$(id -u)":"$(id -g)" "$HOME"/.kube/config 


=========  Install a Network Plugin (Calico):

kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml


========Generate Join Command:

kubeadm token create --print-join-command                  
                                                       (command chalane ke bad hame yaha 1 token mile ga use worker node pe chalana hoga jis se (node) create kiya he our us node ko master ke saath connect krne se cluster ban gya he & hame end me (--v=5) ye hamara version he or ye version pe hi chalta he  ye ham worker node pe key likhte saye end me lkhe ge then fir (6443) likh denge& hame master me jake security me (6443) chala dena he

                                                       
********                                 Execute on ALL of your Worker Nodes

======Perform pre-flight checks:

sudo kubeadm reset pre-flight checks

========Paste the join command you got from the master node and append --v=5 at the end:

sudo kubeadm join <private-ip-of-control-plane>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash> --cri-socket 
"unix:///run/containerd/containerd.sock" --v=5


======Add sudo at the beginning of the command
======Add --v=5 at the end
======Example format:

sudo <paste-join-command-here> --v=5

                                     Verify Cluster Connection
                                           On Master Node:

kubectl get nodes
