# Ansible_Workshop_201
Ansible_Workshop_201
A lot of growing requests on Ansible and a more Advanced Workshop Class - 
1. Ansible Config
2. SSH Key Privilages
How To setup an End nodes - Linux Based - SSH Key Copy - modify ViSudo on Endpoint - Add to Group

ssh-keygen -f ~/.ssh/test.rsa

---
 - name: setup users and stuff
   hosts: web
   become: true
   tasks:
     - name: users
       user:
          state: present
          name: user
          group: wheel
  
     - name: setup key file
       copy:
          dest: "/etc/sudoers.d/user"
          content: "user  ALL=(ALL)  NOPASSWD: ALL" 

     - name: Deploy SSH Key
       authorized_key:
          user: user
          key: "{{ lookup('file', '~/.ssh/test.pub') }}"
          state: present     

   handlers:
     - name: restart ssh
       service:
          name: sshd
    state: restarted
3. Users - Firewall
Create a new User - allow items in Firewall via Handlers - Setup Group - 
4. Ansible Galaxy
Use Ansible Galaxy and Configure Varaible in Docker - 
5. Ansible Facts
Configuring and Setting up a LVM using LVM Facts. 
6. Ansible Secrets - Vault
How to configure Ansible Vault and throw into Current Docker Playbook - 


