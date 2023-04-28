## Run just Nginx tag section 
```
ansible-playbook myplaybook.yml --tags nginx
```

## Skip particular tags

```
ansible-playbook myplaybook.yml --skip-tags zabbix
```






## PLAYBOOK :


```

---
- name: Install Nginx and Zabbix agent
  hosts: all
  become: true
  
  tasks:
    - name: Install Nginx
      yum:
        name: nginx
        state: present
      tags:
        - nginx
      notify:
        - start nginx

    - name: Configure Nginx
      template:
        src: templates/nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      tags:
        - nginx
      notify:
        - reload nginx

    - name: Install Zabbix agent
      yum:
        name: zabbix-agent
        state: present
      tags:
        - zabbix

    - name: Configure Zabbix agent
      template:
        src: templates/zabbix_agentd.conf.j2
        dest: /etc/zabbix/zabbix_agentd.conf
      tags:
        - zabbix
      notify:
        - restart zabbix-agent

  handlers:
    - name: start nginx
      service:
        name: nginx
        state: started

    - name: reload nginx
      service:
        name: nginx
        state: reloaded

    - name: restart zabbix-agent
      service:
        name: zabbix-agent
        state: restarted


```
