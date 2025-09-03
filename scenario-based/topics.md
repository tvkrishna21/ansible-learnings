***
# Scenario based topics 
***
#### 1. You have an ansible playbook that configures a server with a specific package. You want to run the playbook on a group of servers, but you want to skip the task that installs the package on a specific server. How would you do this?

#### Ans: To skip a task for a specific host, you can use the --limit option and specify the hostname or pattern to match the host you want to skip.
```
ansible-playbook --limit '!server1' playbook.yml
```

#### 2. You have an ansible playbook that installs and configures a web server. You want to run the playbook, but you want to test the changes before making them. How would you do this?

#### Ans: To test the changes made by an ansible playbook without making any actual changes, you can use the --check flag. This will perform a "dry run" of the playbook and report any changes that would be made, but it will not actually make any changes on the target hosts.
```
ansible-playbook --check playbook.yml
```

#### 3. You have an ansible playbook that installs and configures a database server. You want to run the playbook, but you want to make it so that any failures will be ignored, and the playbook will continue to run. How would you do this?

#### Ans: To ignore failures and continue running the playbook, you can use the --force-handlers flag. This will cause ansible to continue running the playbook even if a task fails, and it will run any handlers (tasks that are triggered by other tasks) that were registered for the failed tasks.
```
>ansible-playbook --force-handlers playbook.yml
```

#### 4. You have an ansible playbook that installs and configures a load balancer. You want to run the playbook, but you want to make it so that any tasks that would change the target hosts will be skipped. How would you do this?

#### Ans: To skip tasks that would make changes to the target hosts, you can use the --diff flag. This will perform a "dry run" of the playbook and report any changes that would be made, but it will skip any tasks that would make actual changes on the target hosts.
```
ansible-playbook --diff playbook.yml
```

#### 5. You have an ansible playbook that installs and configures a monitoring system. You want to run the playbook, but you want to make it so that any tasks that are not idempotent (i.e., tasks that cannot be run multiple times without causing harm) will be skipped. How would you do this?

#### Ans: To skip non-idempotent tasks, you can use the --skip-tags flag and specify the tag or tags for the tasks you want to skip.
```
ansible-playbook --skip-tags non_idempotent playbook.yml
```

#### 6. You have an ansible playbook that installs and configures a file server. You want to run the playbook, but you want to make it so that any tasks that are not relevant to the target hosts will be skipped. How would you do this?

#### Ans: To skip tasks that are not relevant to the target hosts, you can use the --skip-tags flag and specify the tag or tags for the tasks you want to skip.
```
ansible-playbook --skip-tags '!file_server' playbook.yml
```
##### Example

```
ansible-playbook my_app_deploy.yml --skip-tags "restart_service"
```
```
- name: Restart web server
  ansible.builtin.service:
    name: apache2
    state: restarted
  tags: restart_service
```
##### This command would execute all tasks in my_app_deploy.yml except for the one tagged restart_service.

#### 7. You want to run a shell command on all of the servers in your inventory. How would you do this using ansible?

#### Ans: You can use the command module to run a shell command on all of the servers in your inventory.
```
 - name: Run shell command
   hosts: all
   tasks:
     - name: Run command
       command: echo hello
```

#### 8. You want to create a user on all of the servers in your inventory. How would you do this using ansible?

#### Ans: To create a user on a target host using Ansible, the ansible.builtin.user module is utilized within a playbook.
```
---
- name: Create a new user on target hosts
  hosts: your_target_hosts # Replace with your inventory group or hostname
  become: true # Use sudo/elevated privileges for user creation
  tasks:
    - name: Add a new user named 'newuser'
      ansible.builtin.user:
        name: newuser
        state: present
        shell: /bin/bash # Optional: Specify the user's login shell
        createhome: yes # Optional: Create the user's home directory
        groups: sudo,docker # Optional: Add the user to specified groups
        append: yes # Optional: Append to existing groups instead of replacing
        password: "{{ 'your_password_here' | password_hash('sha512') }}" # Hashed password
        # For security, consider using Ansible Vault for sensitive data like passwords.
```

#### 9. You want to run a playbook only on a specific host within your inventory group. What do you use?

#### Ans: Use the --limit option with the hostname.
```
ansible-playbook playbook.yml --limit host1
```

#### 10. You want to encrypt sensitive variables like passwords. How do you do it?

#### Ans: Use Ansible Vault to encrypt sensitive data.
```
ansible-vault encrypt secrets.yml
```

#### 11. You need to update only 2 servers at a time during a rolling deployment. What Ansible option helps?

#### Ans: Use the serial keyword in your playbook.yaml file
```
- hosts: webservers
  serial: 2
```

#### 12. You need to pass a variable during playbook execution. How?

#### Ans: Use the --extra-vars option.
```
ansible-playbook playbook.yml --extra-vars "env=prod"`
```

#### 13. You want to prevent a task from running when a file doesn’t exist. What do you use?

#### Ans: Use the when condition with stat module.
```
- name: Check file
  stat: path=/etc/myconfig.conf
  register: file_check

- name: Do something
  when: file_check.stat.exists
```

#### 14. You want to run a playbook step-by-step for debugging. What’s the best way?

#### Ans: Use --step to confirm each task before it runs.
```
ansible-playbook playbook.yml --step
```

#### 15. You need to stop execution if a task fails. How?

#### Ans: Tasks fail by default, but to ensure immediate failure, avoid ignore_errors.
```
- name: Critical task
  command: /bin/false
```

#### 16. You want to run only one specific task in a playbook. What do you use?

#### Ans: Tag the task and use --tags to run it.
```
- name: Install nginx
  apt: name=nginx state=present
  tags: nginx
```
```
ansible-playbook playbook.yml --tags nginx`
```

#### 17. How do you store host-specific variables?

#### Ans: Create a file in the host_vars directory with the hostname.
```
host_vars/
└── web01.yml
```

#### 18. You need to loop over a list of packages to install them. How?

#### Ans: Use the loop directive.
```
- name: Install packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - nginx
    - curl
    - git
```

#### 19. How do you disable fact gathering to speed up playbook runs?

#### Ans: Set gather_facts: false in the playbook
```
- hosts: all
  gather_facts: false
```

#### 20. You want to ensure a service is restarted only if a file was changed. How?

#### Ans: Use notify and handlers.
```
- name: Modify config
  template: src=conf.j2 dest=/etc/app.conf
  notify: Restart app

handlers:
  - name: Restart app
    service: name=myapp state=restarted
```

#### 21. You want to conditionally run a task based on a variable. How?

#### Ans: Use the when keyword.
```
- name: Install nginx on prod
  apt: name=nginx state=present
  when: env == "prod"
```

#### 22. You want to run a shell script on a host. What do you use?

#### Ans: Use the script module or command module or shell module
##### Shell module
```
- name: Run a shell script
  hosts: your_ansible_host_group
  tasks:
    - name: Execute my_script.sh
      shell: /path/to/my_script.sh arg1 arg2
```
##### Command module
```
- name: Run a command
  hosts: your_ansible_host_group
  tasks:
    - name: Execute a simple command
      command: /path/to/my_script.sh
```
##### Script module
```
- name: Run a local script on remote host
  hosts: your_ansible_host_group
  tasks:
    - name: Transfer and execute script
      script: /path/to/local/script.sh arg1 arg2
```

#### 23. You need to fetch a file from a remote server to localhost. How?

#### Ans: Use the fetch module or copy module.
```
- name: Copy logs to local
  fetch:
    src: /var/log/syslog
    dest: ./backup/
    flat: yes
```

#### 24. You want to include tasks from another YAML file. How?

#### Ans: Use the include_tasks module.
```
- include_tasks: security-hardening.yml
```

#### 25. You need to remove a package from all hosts. What’s the easiest way?

```
- name: Remove nginx
  apt:
    name: nginx
    state: absent
```

#### 26. How do you retry a failed task a few times before marking it failed?

#### Ans: Use retries with until.
```
- name: Wait for port
  wait_for: port=80
  retries: 5
  delay: 10
  until: result is succeeded
```

#### 27. What is [servers.vars] in ansible inventory

#### Ans: This construct allows you to define variables that are common to all members of a specific group, without having to define them individually for each host. This promotes reusability and simplifies inventory management, especially in larger environments.
```
[servers]
server1.example.com
server2.example.com

[servers:vars]
http_port=8080
database_name=production_db
```

##### In this example:
  * The servers group contains server1.example.com and server2.example.com.
  * The [servers:vars] section defines http_port as 8080 and database_name as production_db.
  * Both server1.example.com and server2.example.com will inherit these variables and can use them in Ansible playbooks.

