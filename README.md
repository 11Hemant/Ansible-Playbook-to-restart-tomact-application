# Ansible-Playbook-to-restart-tomact-application

## Step 1:Create a new file called restart_tomcat.yml and add the following code:
---
- name: Restart Tomcat if new war file
  
  hosts: our_tomcat_hosts
  
  become: true

  tasks:
    - name: Check if new war file exists
      stat:
        path: /path/to/new/war/file.war
      register: war_file

    - name: Copy new war file
      copy:
        src: /path/to/new/war/file.war
        dest: /path/to/tomcat/webapps/app.war
      when: war_file.stat.exists

    - name: Restart Tomcat
      service:
        name: tomcat
        state: restarted

- name: Check if Tomcat process is up and running
  hosts: our_tomcat_hosts
  become: true

  tasks:
    - name: Check if Tomcat process is running
      command: ps aux | grep tomcat | grep -v grep
      register: tomcat_process
      failed_when: tomcat_process.rc != 0

    - name: Print top 10 running processes
      command: ps aux --sort=-%cpu | head -n 11
      register: top_processes

    - name: Display top 10 running processes
      debug:
        msg: "{{ top_processes.stdout }}"

## Step 2:Replace the following placeholders in the playbook:

Replace our_tomcat_hosts with the target hosts where our Tomcat application is running.

Replace /path/to/new/war/file.war with the path to the new war file that we want to deploy.

Replace /path/to/tomcat/webapps/app.war with the path where our Tomcat application is deployed.

If the Tomcat service has a different name, modify tomcat accordingly.

## Step 2:Save the restart_tomcat.yml file.

## Step 3:Ensure that we have Ansible installed on your local machine.

## Step 4:Open a terminal or command prompt, navigate to the directory containing the restart_tomcat.yml file, and run the following command:


ansible-playbook restart_tomcat.yml

## Step 5:Ansible will execute the playbook and perform the following actions:

Check if a new war file exists and copy it to the Tomcat deployment directory if it does.

Restart the Tomcat service.

Check if the Tomcat process is up and running.

Print the top 10 running processes.

Ensure that we have SSH access to the target hosts configured in your Ansible inventory file. 

also set up and check passwordless SSH authentication between the Ansible control node and the target hosts.

Ensure that we have SSH access to the target hosts configured in your Ansible inventory file. 

also set up and check passwordless SSH authentication between the Ansible control node and the target hosts.
