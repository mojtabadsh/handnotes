# Import Playbook & Tasks 

#### import playbook inside a master playbook

```yaml
vim project1.yml
---
- hosts: hostname
  roles:
     - project1
- import_playbook: project2.yml
```

#### import task file inside a main.yml in tasks Dir

```yaml
vim roles/tasks/main.yml
---
- name: Import Tasks
  file:
    path: /home/path
    state: touch
- import_tasks: task.yml
```

