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
3. - Conditionals - Firewall - install yum tools and  setup credentials





4. Ansible Galaxy
Use Ansible Galaxy and Configure Varaible in vars - 
ansible-galaxy install linux-system-roles.cockpit
ansible-galaxy install linux-system-roles.firewall
- 
and configure podman - 

---
- name: install cockpit
  hosts: web
  become: true
  tasks:

  - name: Install RHEL/Fedora Web Console (Cockpit)
    include_role:
      name: linux-system-roles.cockpit
    vars:
      cockpit_packages: default
       - cockpit-storaged
       - cockpit-podman
    when:  ansible_memtotal_mb >= 8024
 
  - name: Configure Firewall for Web Console
    include_role:
      name: linux-system-roles.firewall
    vars:
      firewall:
        service: cockpit
        state: enabled
    when:  ansible_memtotal_mb >= 8024
    
    did it fail ? fix it ?

---
- name: isntall cockpit
  hosts: web
  become: true
  tasks:  
  - name: Install RHEL/Fedora Web Console (Cockpit)
    include_role:
      name: linux-system-roles.cockpit
    vars:
      cockpit_packages: 
       - cockpit-storaged
       - cockpit-networkmanager
       - cockpit-ws
       - cockpit-system
    when:  ansible_memtotal_mb >= 2024
 
  - name: Configure Firewall for Web Console
    include_role:
      name: linux-system-roles.firewall


  - name: install package
    package:
      name: policycoreutils-python
      state: present
  - name: enable seport
    seport:
      ports: 9090
      proto: tcp
      setype: websm_port_t
      state: present


5. Ansible Facts
Configuring and Setting up a LVM using LVM Facts. 


6. Ansible Secrets - Vault
How to configure Ansible Vault and throw into Current Docker Playbook - 

ansible-vault create secret.yml
