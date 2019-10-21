# openfaas-ansible-playbook
Ansible playbook as a function with OpenFaaS 

## How it works:
Ansible template **clones the playbooks and inventories from a GIT URL** on the **build time**.   
One need to specify the clone URL by editing the definition file
```
playbook_url="<playbook's git url>"
playbook="<playbook file name>"
inventory_url="<inventory's git url>"
inventory="<inventory file name>"
additional_packages="<any additional packages (apk) that you want to add>"
```

## Getting Started

Pull the `ansible` template 
```
faas template pull https://github.com/s8sg/openfaas-ansible-playbook.git
```
  
Create a new FaaS function
```
faas new --lang ansible ansible-test
```
   
Edit the definition file and set the desired value
```
vim ansible-test/definition
```
   
Build and Deploy the function
```
faas build --yaml ansible-test.yml
faas deploy --yaml ansible-test.yml
```
   
Execute the function
```
curl "http://127.0.0.1:8080/function/ansible-test"
```
