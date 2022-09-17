# ansible-lab
Ansible commands, snippets and recipes for learning purpose

## Starting the Lab
  ```bash
  cd diveintoansible-lab-master
  docker compose up
  ```

## Using the Lab
1. Type localhost:1000 into your browser
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
7. 