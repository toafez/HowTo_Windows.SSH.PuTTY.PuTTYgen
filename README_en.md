English | [Deutsch](README.md)

# HowTo: SSH key with PuTTY and PuTTYgen
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Ftoafez%2FHowTo_Windows.SSH.PuTTY.PuTTYgen%2Fedit%2Fmain%2FREADME_en.md&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

## What is this about?
The following instructions describe **how to set up SSH public key authentication** from a local Linux system to a remote Linux server **using the PuTTY and PuTTYgen programs.

## Introduction
Secure Shell, or SSH for short, is a network protocol for establishing encrypted connections between devices on the local network or over the Internet. SSH public key authentication uses a private and public key pair to establish a password-free login to a remote server, which can be further secured by entering an additional passphrase if required. The use of such a key pair is therefore much more difficult to compromise than entering a password.

## Installing PuTTY and PuTTYgen
To generate an RSA key on Windows and then connect to a remote server via SSH, you will need the PuTTY client and PuTTYgen. Both can be downloaded as Windows installers from the [PuTTY download page](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

## Generate a public and private key using PuTTYgen
1. Run **PuTTYgen** from the Windows programs list.
2. In the **Parameters / Type of key to generated** section, select **RSA**.
3. In the **Parameters / Number of bits in a generated key** section, set the number of bits to at least **2048**, preferably **4096**.
4. Click on the **Generate** button in the **Actions / Generate a public/private key pair** section.

    ![01_PuTTY_Key_Generator](/images/01_PuTTY_Key_Generator.png)

5. You will now be asked to randomly move the mouse over the PuTTY Key Generator window to generate the public and private keys.

    ![02_PuTTY_Key_Generator](/images/02_PuTTY_Key_Generator.png)

6. Once the process is complete and the key information is displayed, a unique name for the key you have just created can be specified in the **Key / Key comment:** section. In this example we will name the key **puttygen-rsa-key**.
7. The use of a passphrase in the **Key / Key passphrase:** section is highly recommended, as it prevents the private key from being easily copied and used, even if your client operating system has been compromised. The disadvantage of a passphrase is that you have to enter it every time you want to connect via SSH. This can be counterproductive if you later want to establish unattended and thus automated SSH connections, for example to perform automated backups. In this case, we strongly advise against entering a passphrase. It is up to you whether you want to use a passphrase or not. In this example, no passphrase is used, but the following two prompts are simply skipped by pressing Enter.
8. Then highlight the public key in the **Public key for pasting into OpenSSH authorized_keys file** section. Right-click on the highlighted text, copy the public key to the clipboard, open a text editor of your choice, paste the public key into the file, and save the text file under a name of your choice in a secure location. 9.
9. Save the private key to the same secure location by clicking the **Save private key** button in the **Actions / Save the generated key** section. The file name is also freely selectable, but has the extension **.ppk**. In this example the file name is puttygen-rsa-key.ppk.

    ![03_PuTTY_Key_Generator](/images/03_PuTTY_Key_Generator.png)

## To connect to the remote server using PuTTY
1. Run **PuTTY** from the Windows programs list.
2. In the **Specify the destination you want to connect to / Host Name (or IP address)** and **Port** sections, enter the IP address of the remote server you want to connect to and adjust the port to use if necessary. This example uses IP address 172.16.1.12 and port 22.
3. In the **Specify the destination you want to connect to / Connection type** section, select the **SSH** option.
4. Click the **Open** button to connect to your remote server.

    ![10_PuTTY_Configuration](/images/10_PuTTY_Configuration.png)

5. When the connection is first established, you will receive a **"Security Alert"** and will be asked to accept the connection in order to perform an initial handshake, during which a so-called fingerprint is stored for future synchronisation. To do this, click on the **Accept** button.

    ![11_PuTTY_Configuration](/images/11_PuTTY_Configuration.png)

6. A terminal window should now open where you need to enter your username and password to log in to your remote server's console. In this example, the username **tommes** is used to log in to a fictitious **Ubuntu server**.

    ```
    login as: tommes
    tommes@172.16.1.12's password:
    ```

7. On successful login, the prompt should appear after a short greeting and, if necessary, further information.

   ```
   tommes@Ubuntu-Server:~$
   ```

8. You should now be in your own home directory. You can check this by typing **pwd** (stands for 'print working directory') and pressing Enter.

    ```
    tommes@Ubuntu-Server:~$ pwd
    /home/tommes
    ```

   In this example, we are in the home directory of the user tommes (correctly, of course, your username should be here), and that is exactly where we want to be. Depending on the remote server system you are connecting to, a different path may be displayed here. For example, if you are using a Synology NAS, the path to the user's home directory would be /var/services/homes/tommes.

    **Note:** If the system you are using allows direct authentication with a key to the superuser **root**, you can switch to the root account at this point to copy the public key. This is done by typing `su -`, `sudo -s` or `sudo -i` and entering a password.


## Copy the public key to the remote server
For better readability, all the following entries are shown without using the `tommes@Ubuntu-Server:~$` command.

1. First, a new hidden directory called .ssh is created in the home directory of the currently logged in user.

    `mkdir .ssh`

2. The directory just created is then given the appropriate rights.

    `chmod 700 .ssh`

3. Now change to this directory

    `cd .ssh`

4. At this point, the prompt should confirm the directory, which should look something like this

    `tommes@Ubuntu-Server:~/ssh$`

5. Continue with the vi editor, which should create or edit the authorized_keys file when called.

    `vi authorized_keys`

    **Note:** This is where it gets a bit tricky, because the vi editor is one of the most popular text editors under Linux, but it takes a lot of getting used to. The following instructions should be followed exactly. If you have typed something wrong, the best thing to do is to return to command mode by pressing **Escape** or **Esc** and close the document without saving. This is done by typing `:q!` and then just start again from the beginning.

 6. first, you will need the public key that you copied from PuTTYgen at the beginning of this guide and saved in a text file. Open the file, select the public key, right click on the selected text and copy the public key back to the clipboard. Then switch back to the terminal window to switch from command mode to input mode in the vi editor. To do this, simply type...

    `i`

7. ...move the mouse pointer into the terminal window and press the right mouse button. This will paste the public key from the clipboard into the open authorized_keys file in the vi editor. It should look something like this...

    ![20_PuTTY_Terminal](/images/20_PuTTY_Terminal.png)

8. If everything looks good, press the **Escape** or **Esc** key to switch back to command mode, and exit the vi editor by typing...

    `:qw!`

9. You should be back at the console. End the terminal session with

    exit

  ... .... The Terminal window should close automatically.

## Setting up PuTTY for key authentication
1. restart **PuTTY** from the Windows programs list.
2. in the **Specify the destination you want to connect to / Host name (or IP address)** and **Port** sections, enter the IP address of the remote server you want to connect to and adjust the port to use if necessary. This example uses IP address 172.16.1.12 and port 22.
3. in the **Specify the destination you want to connect to / Connection type** section, select the **SSH** option.

    ![12_PuTTY_Configuration](/images/12_PuTTY_Configuration.png)

4. in the left section **Category:** go to **Connection / Data** and in the right section **Login details / auto-login username** enter the username you want to use to connect to your remote server. If your system allows direct authentication with a key to the superuser **root**, you can also enter **root** here.
5. in the same section, under **When username is not specified**, select the **Prompt** option.

    ![13_PuTTY_Configuration](/images/13_PuTTY_Configuration.png)

6. in the left section **Category:** go to **Connection / SSH / Auth / Credentials:** and in the right section **Public-key authentication / Private key file for authentication:** enter the path to your **private** key that you created and saved with PuTTYgen at the beginning. It is best to use the **Browse** button to select the appropriate file with a .ppk extension. In this example the file name is puttygen-rsa-key.ppk.

    ![14_PuTTY_Configuration](/images/14_PuTTY_Configuration.png)

7. in the left section **Category:** go back to **Session:** and enter a name for the connection in the right section under **Load, save or delete a stored session / Saved Sessions:** and click **Save:**.

    ![15_PuTTY_Configuration](/images/15_PuTTY_Configuration.png)

8 In the future, you will be able to select the configuration you have just created within PuTTY to authenticate yourself to your remote server via key exchange.

    ![16_PuTTY_Configuration](/images/16_PuTTY_Configuration.png)

