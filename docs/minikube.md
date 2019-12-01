# Installation Minikube

1. Install kubectl (**v1.7.0** is latest): [Instructions here](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
2. Install minikube (**v0.21.0** is latest): [Instructions here](https://kubernetes.io/docs/tasks/tools/install-minikube/)
3. Configure your cluster:

    ```
    $ minikube config set memory 4096
    $ minikube config set cpus 2
    $ minikube config set disk-size 30g
    ```  
    Set `--memory`, `--cpus`, `--disk-size` to values acceptable to you and resources available on your hosting environment. The above should be considered **minimums.**
    
    `minikube start --cpus 4 --memory 8192  --disk-size=80GB`

4.  Configure your hypervisor

    5.1 For MacOS/VirtualBox

        $ minikube config set vm-driver virtualbox

    5.2 For Linux/KVM:

        $ minikube config set vm-driver kvm

    **Note that `--vm-driver` is hosting OS specific.**  For MacOS use one of `virtualbox` or `xhyve`.

5. Start your cluster:
    ```
    $ minikube start
    ```

6. (Optional) Enable heapster for memory & cpu usage statistics:
    
    ` minikube addons enable heapster`

7. Install helm

    8.1 For MacOS
        
     `brew install kubernetes-helm`
     
    8.2 For Linux
    
    `sudo snap install helm --classic`
        
    8.3 Run Helm and wait for kube-system pods up
    
    `helm init`

8. Impartant commands

    `eval $(minikube docker-env)`
    
    **Important note**: You have to run eval $(minikube docker-env) on each terminal you want to use, since it only sets the environment variables for the current shell session.