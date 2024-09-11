# Important Pointers

1. Control node is where ansible is installed. Managed nodes are the target nodes.
2. Ansible is agentless. You don't require to install any agent on the managed nodes.
3. Python should be present on on both control node & managed nodes.
4. Passwordless Authentication helps to skip the repeated authentication and allow ansible to deploy without hurdles.
5. All the managed nodes shhould be present in the **inventory.ini** file.
6. To run the ansible adhoc commands you have to provide the inventory file. \
Syntax: `ansible -i <inventory-file> -m <module> -a <arguement> <hosts>`
Example: `ansible -i inventory.ini -m ping all`
7. Setting hosts to **all** will run the ansible commands on all the hosts mentioned in the inventory file.
8. We can group the hosts in the inventory file.
Example: 

```ini
[ubuntuServers]
ubuntu@1.1.1.1
ubuntu@2.2.2.2

[azureServers]
azureuser@8.9.0.1
```

9. If you don't want to repeatedly add the inventory file in the commands keep the inventory file in _/etc/ansible/hosts_ path. If the path doesn't exist, you can create it.
10. Playbook consists plays. Plays consist of hosts, user & tasks (one or more). Tasks consist of list of modules. To run ansible playbooks, you can use: \
Syntax: `ansible-playbook -i <inventory> <ansible-playbook>` \
Example: `ansible-playbook -i inventory.ini apache-playbook.yaml`
11. The field `become: true` helps ansible playbook run with root user.
12. The field `state: present` means install. The field `state: absent` means uninstall.
13. The first task every ansible play will run is **gathering facts** where it would try to check if it can connect to the managed hosts or not.
14. Roles help in code readability & modularity. Each segment of a playbook can be broken down into parts & kept in separate folders for ease of reading, reviewing & sharing.
15. To create the folder structure & role, run `ansible-galaxy role init <role-name>` \
Example: `ansible-galaxy role init apache2`
16. Roles can be uploaded and downloaded from ansible galaxy. It is like a hub where multiple roles can be found and shared. Download roles could be found in _~/.ansible/roles_
17. The directory _files_ in ansible role can be used to store the files used during playbook runs, such as html pages as used in one of the tutorials. They can be referenced using _/files/<filename>_
18. The directory _templates_ is used to keep files whose content might be dynamic. Say, there is a file which needs the hostname. It can be passed like _{{ ansible.facts['hostname'] }}
19. The directory _defaults_ can be used to store default values of variables.
20. The directory _handlers_ to store tasks that can be called multiple times in the playbook. For example, if there is a need to restart the system after a particular task, a handler can be configured and the task can notify the handler to run the specific tasks.
21. Ansible is idempotent in nature. If the change is already there, it won't do it again.
22. Ansible playbook can have multiple roles.
23. Ansible Galaxy is like dockerhub.
24. Roles from ansible galaxy can be installed by `ansible-galaxy role install <role-name>` which can be found in ansible galaxy pages. You would need to login to download & install roles.
25. Installed roles will be present at _~/.ansible/roles/_ directory.
26. To publish roles in ansible galaxy you would need an API token which can be found in the Ansible Galaxy dashboard through **Collections** tab. Moreover, you would need to to keep the roles related folders & details in a separate GitHub repository.
27. To publish roles run `ansible-galaxy import <github-username> <github-repository-with-role> --token <API-token>`
28. Ansible uses Jinja2 templating.
29. Ansible Collections are a bundle of roles, modules, playbooks that can be used to interact with third parties like AWS, Azure, GCP etc.
30. Precedence order of variables - [Source](https://medium.com/trendfingers/variable-precedence-in-ansible-2a3dba7766ab):
  i. extra-vars from command-line (top)
  ii. block variables and task variables
  iii. role & include variables
  iv. play varables from vars & vars_files
  v. set facts
  vi. host facts & registered variables from task output
  vii. playbook group_vars & host_vars in /playbook
  viii. invetory group_vars & host_vars in /inventory
  ix. inventory variables in inventory.ini
  x. role defaults (bottom)
31. 