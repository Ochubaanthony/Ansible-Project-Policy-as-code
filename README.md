open a vscode
create a folder "playbook"
on your terminal type " aws configure  " asking for Access and Secret key" you 
step to create the both key > right top conner > security crediential > under access key "click on Access key", then copy both keys and enter on terminal
enter Default region name: eu-central-1, output: json
need to install pip3 boto3, "brew install pthyon" , "pips --version", pip3 install boto3
install anasible aws collection
ansible-galaxy collection install amazon.aws
create a file call it  " s3_versioning.yaml"
copy the provider yaml file and paste on the s3-versioning.yaml
things to acheived 1. list all the s3 bucket array
---
- name: Enforce s3 bucket versioning on AWS account
  hosts: localhost
  gather_facts: false

  tasks:
    - name: List S3 buckets in AWS account
      amazon.aws.s3_bucket_info:
      register: result
    
    - debug:
        var: result
run
      ansible-playbook s3-versioning.yaml
and loop-array>item>versionning
---
- name: Enforce s3 bucket versioning on AWS account
  hosts: localhost
  gather_facts: false

  tasks:
    - name: List S3 buckets in AWS account
      amazon.aws.s3_bucket_info:
      register: result
    
    - debug:
        var: result
    
    - name: Enable versioning on S3 bucket
      amazon.aws.s3_bucket:
        name: "{{ item.name }}"
        versioning: yes
      loop: "{{ result.buckets }}" 
run
ansible-playbook s3-versioning.yaml
this list all the s3 bucket you create on the s3 console
