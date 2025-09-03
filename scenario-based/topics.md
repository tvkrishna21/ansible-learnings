***
# Scenario based topics 
***
### Ques. You have an ansible playbook that configures a server with a specific package. You want to run the playbook on a group of servers, but you want to skip the task that installs the package on a specific server. How would you do this?

### Ans: To skip a task for a specific host, you can use the --limit option and specify the hostname or pattern to match the host you want to skip.
>ansible-playbook --limit '!server1' playbook.yml

### 2. You have an ansible playbook that installs and configures a web server. You want to run the playbook, but you want to test the changes before making them. How would you do this?

#### Ans: To test the changes made by an ansible playbook without making any actual changes, you can use the --check flag. This will perform a "dry run" of the playbook and report any changes that would be made, but it will not actually make any changes on the target hosts.
>ansible-playbook --check playbook.yml
