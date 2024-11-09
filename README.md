Here’s a `README.md` file that explains the Ansible playbook template you've shared and provides deployment instructions.

---

# Ansible Playbook for Apache and Git Installation on Ubuntu Clients

This Ansible playbook installs and configures Apache and Git on a group of Ubuntu clients. It includes tasks to update all packages, install Apache, configure and start its service, and copy an `index.html` file to the web server's root directory. Additionally, it provides an example of using the debug module to display a custom message across all hosts.

## Requirements

- Ansible installed on the control node.
- An inventory file defining a host group named `ubuntu-clients` with the IP addresses or hostnames of the target Ubuntu clients.
- `index.html` file on the control node to be copied to `/var/www/html` on the clients.

## Playbook Structure

The playbook contains three main sections:

1. **Apache Installation on Ubuntu Clients**
   - Updates all packages on the server.
   - Installs Apache (`apache2` package).
   - Starts and enables the Apache service.
   - Copies a custom `index.html` file from the control node to the client servers.

2. **Git Installation on Ubuntu Clients**
   - Installs Git on the Ubuntu clients.
   - Ensures the package cache is updated before installation.

3. **Debug Task**
   - Displays a custom debug message ("Andre DIBI") on all hosts.

### Playbook Tasks Explanation

```yaml
- name: Install apache on ubuntu clients
  hosts: ubuntu-clients
  become: yes

  tasks:
    - name: Upgrade all packages on the server
      ansible.builtin.apt:
        name: '*'
        state: latest     

    - name: Install Apache
      ansible.builtin.apt:
        name: apache2
        state: present

    - name: Start Apache service if not already started
      ansible.builtin.service:
        name: apache2
        state: started 

    - name: Enable Apache2 service
      ansible.builtin.service:
        name: apache2
        enabled: yes 

    - name: Copy index.html file to clients
      ansible.builtin.copy:
        src: index.html
        dest: /var/www/html
```

## Deployment Instructions

1. **Prepare the Ansible Inventory File**
   - Create an `inventory` file listing the Ubuntu clients under the group `[ubuntu-clients]`:

     ```ini
     [ubuntu-clients]
     client1 ansible_host=192.168.1.10
     client2 ansible_host=192.168.1.11
     ```

2. **Run the Playbook**

   Execute the playbook with the following command:

   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

3. **Verify Apache Installation**
   - After the playbook completes, you can verify Apache is running by navigating to the IP address of one of the Ubuntu clients in a web browser.

