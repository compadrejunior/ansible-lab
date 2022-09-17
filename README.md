# ansible-lab
Ansible commands, snippets and recipes for learning purpose

## Starting the Lab
  ```bash
  cd diveintoansible-lab-master
  docker compose up
  ```

## Using the Lab
1. Type localhost:1000 into your browser. You will see the lab home page.
  
2. Select Ubuntu-c
3. Login with user ansible and password password

## Copying the password to instances
1. Install sshpass
   
    ```bash
    sudo apt update
    sudo apt install sshpass
    ```
2. Create a password file with text password
    ```bash
    echo password > password.txt
    ```
3. Create a shell script with name copy-password.sh
    ```bash
    vi copy-password.sh
    ```
4. Paste the following content
    ```bash
    #!/bin/bash
    for user in ansible root
    do
      for os in ubuntu centos
      do
        for instance in 1 2 3
        do
          sshpass -f password.txt ssh-copy-id -o StrictHostKeyChecking=no ${user}@${os}${instance}
        done
      done
    done
    ```
5. Save the file and exit vi.
6. Execute the script
    ```bash
    ./copy-password.sh
    ```

## Test connectivity ty your instances
1. Type in the following command to create your inventory and send a ping command to all instances.
    ```bash
    ansible -i,ubuntu1,ubuntu2,ubuntu3,centos1,centos2,centos3 all -m ping
    ```
    
    > **Note**: the *ansible -i* command creates an inventory 
    > and expect you to inform a file but instead you can send 
    > the content of the file just by typing a comma right 
    > after the i. The *all* parameter specifies that we will  
    > use all instances. The *-m ping* specifies the module 
    > that we will use. 
2. The output should be similar to the example bellow:
    ```bash
    ubuntu3 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
    }
    ubuntu2 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
        },
        "changed": false,
        "ping": "pong"
    }
    ubuntu1 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
        },
        "changed": false,
        "ping": "pong"
    }
    centos1 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/libexec/platform-python"
        },
        "changed": false,
        "ping": "pong"
    }
    centos2 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/libexec/platform-python"
        },
        "changed": false,
        "ping": "pong"
    }
    centos3 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/libexec/platform-python"
        },
        "changed": false,
        "ping": "pong"
    }
    ```