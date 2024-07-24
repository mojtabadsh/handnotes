# Handlers

> Sometimes you want a task to run only when a change is made on a machine. For example, you may want to restart a service if a task updates the configuration of that service, but not if the configuration is unchanged. Ansible uses handlers to address this use case. 

> Handlers are tasks that only run when notified.(Trigger a task If Needed)
>
>  Each handler should have a globally unique name.
>
> Trigger a task at the end of each block of tasks in a playbook, and will only be triggered Once even if notified by multiple different tasks.

vim roles/project/tasks/main.yml

```yaml
- name: config nginx
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify:
    - restart nginx 
```

vim roles/project/handlers/main.yml

```yaml
- name: restart nginx
  service:
    name: nginx
    state: restarted
```

vim roles/project/templates/nginx.conf.j2

```yaml
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;
...
```