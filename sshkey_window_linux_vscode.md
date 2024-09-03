Vscode:
Install package: 
Remote SSH
>Connect to Host: ssh podman_user#192.168.1.1
>Enter password:

C:\User\TAI_HUYNH>ssh-keygen

C:\User\TAI_HUYNH\.ssh\id_rsa : Prive file 3KB

C:\User\TAI_HUYNH\.ssh\id_rsa : Public file 1KB

### Add ssh key to github

ssh -T git@github.com # add ssh key to `C:\Users\<YourUsername>\.ssh\known_hosts`

### Add ssh key to VScode
![image](https://github.com/user-attachments/assets/d35afcfa-e056-45b9-93f4-3806056965ca)

copy Public key to /home/tai_huynh/.ssh/id_rsa and change the name to /authorized_keys

sudo vim /etc/ssh/ssh_config: Add PubkeyAuthenication Yes

Step 3: Restart service ssh
systemctl restart ssh
Step 4: ssh to the server to check
ssh root@192.168.1.x
