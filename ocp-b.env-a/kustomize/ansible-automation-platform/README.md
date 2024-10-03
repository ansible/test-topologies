# ansible-automation-platform
This is a kustomize component that installs the ansible automation platform operator on the k8s cluster in preperation for installing additional Ansible SaaS components 

### How to use
```bash
git clone git@github.com:Ansible-SaaS/ansible-automation-platform.git
cd ansible-automation-platform/

kustomize build -o run-level-1.yaml run-level-1/
kubectl apply run-level-1.yaml

kustomize build -o run-level-2.yaml run-level-2/
kubectl apply run-level-2.yaml

kustomize build -o run-level-3.yaml run-level-3/
kubectl apply run-level-3.yaml

kustomize build -o run-level-4.yaml run-level-4/
kubectl apply run-level-4.yaml
```
