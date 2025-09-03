***
# Scenario based topics 
***
## Q. You have an ansible playbook that configures a server with a specific package. You want to run the playbook on a group of servers, but you want to skip the task that installs the package on a specific server. How would you do this?

##  To skip a task for a specific host, you can use the --limit option and specify the hostname or pattern to match the host you want to skip.
>ansible-playbook --limit '!server1' playbook.yml
