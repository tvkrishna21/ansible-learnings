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
ansible-playbook your_playbook.yml --skip-tags "tag1,tag2"
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
