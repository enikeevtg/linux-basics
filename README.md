# UNIX/Linux operating systems (Basic).

Linux system installation and updates. Administration basics.

## Contents

1. [Installation of the OS](#part-1-installation-of-the-os)  
2. [Creating a user](#part-2-creating-a-user)  
3. [Setting up the OS network](#part-3-setting-up-the-os-network)   
4. [OS Update](#part-4-os-update)  
5. [Using the sudo command](#part-5-using-the-sudo-command)  
6. [Installing and configuring the time service](#part-6-installing-and-configuring-the-time-service)  
7. [Installing and using text editors](#part-7-installing-and-using-text-editors)  
8. [Installing and basic setup of SSHD service](#part-8-installing-and-basic-setup-of-the-sshd-service)  
9. [Installing and using the top, htop utilities](#part-9-installing-and-using-the-top-htop-utilities)   
10. [Using the fdisk utility](#part-10-using-the-fdisk-utility)   
11. [Using the df utility](#part-11-using-the-df-utility)    
12. [Using the du utility](#part-12-using-the-du-utility)    
13. [Installing and using the ncdu utility](#part-13-installing-and-using-the-ncdu-utility)    
14. [Working with system logs](#part-14-working-with-system-logs)     
15. [Using the CRON job scheduler](#part-15-using-the-cron-job-scheduler)


## Part 1. Installation of the OS

1.1. Ubuntu version checking

    cat /etc/issue

  ![p1_2_setup_17_result.png](screenshots/part_1/p1_2_setup_17_result.png)

## Part 2. Creating a user

**== Task ==**

##### Create a user other than the one created during installation. The user must be added to `adm` group.
- Add a screenshot of command call to create user.
- The new user must be in the output of the command: \
  `cat /etc/passwd`
- Add a screenshot of the command output.

**== Solution ==**

2.1. Command to create user

    $ sudo adduser <new_user_name>

  ![p2_1_adduser.png](screenshots/part_2/p2_1_adduser.png)

2.2. Command to add new user to `adm` group

    $ sudo usermod -G adm <user_name>

  ![p2_2_usermod.png](screenshots/part_2/p2_2_usermod.png)

2.3. New user creation checking 
      
    $ cat /etc/passwd | grep task2_user

  ![p2_3_passwd.png](screenshots/part_2/p2_3_passwd.png)

2.4. Group checking

    $ cat /etc/group | grep adm

  ![p2_4_group_checking.png](screenshots/part_2/p2_4_group_checking.png)

## Part 3. Setting up the OS network

**== Task ==**

##### Set the machine name as user-1
##### Set the time zone corresponding to your current location.

##### Output the names of the network interfaces using a console command.
- In the report give an explanation for the presence of the lo interface.
##### Use the console command to get the ip address of the device you are working on from the DHCP server.
- Decode DHCP in the report.
##### Define and display the external ip address of the gateway (ip) and the internal IP address of the gateway, aka default ip address (gw).
##### Set static (manually set, not received from DHCP server) ip, gw, dns settings (use public DNS servers, e.g. 1.1.1.1 or 8.8.8.8).

##### Reboot the virtual machine. Make sure that the static network settings (ip, gw, dns) correspond to those set in the previous point.
- Describe in the report what you have done to complete all seven points (you can do it in text or with screenshots).
- Successfully ping 1.1.1.1 and ya.ru remote hosts and add a screenshot of the output command to the report. There should be "0% packet loss" phrase in command output.

**== Solution ==**

3.1. Setting the machine name as user-1

    $ hostnamectl set-hostname user-1

  ![p3_set_1_machine_name.png](screenshots/part_3/p3_1_set_machine_name.png)

  After machine reboot
  
    $ sudo reboot

  ![p3_2_after_reset.png](screenshots/part_3/p3_2_after_reset.png)

3.2. Setting the time zone corresponding to current location

    $ timedatectl
    $ timdatectl list-timezones | grep Moscow
    $ sudo timedatectl set-timezone Europe/Moscow
    $ timedatectl


  ![p3_3_timezone.png](screenshots/part_3/p3_3_timezone.png)

  Another way with tzselect utility:
      
    $ tzselect

  ![p3_4_tzselect.png](screenshots/part_3/p3_4_tzselect.png)

3.3. The names of the network interfaces output using a console command

    $ ip -br address

  ![p3_5_network_interfaces.png](screenshots/part_3/p3_5_network_interfaces.png)

  > [Virtual loopback interface](https://en.wikipedia.org/wiki/Loopback)
  > 
  > Implementations of the Internet protocol suite include a virtual network interface through which network applications can communicate when executing on the same machine. It is implemented entirely within the operating system's networking software and passes no packets to any network interface controller. Any traffic that a computer program sends to a loopback IP address is simply and immediately passed back up the network software stack as if it had been received from another device. Unix-like systems usually name this loopback interface **lo** or **lo0**.
  >
  > [lo (loopback device)](https://ru.hexlet.io/courses/linux-administration/lessons/interfaces/theory_unit) – виртуальный интерфейс, присутствующий по умолчанию в любом Linux. Он используется для отладки сетевых программ и запуска серверных приложений на локальной машине. С этим интерфейсом всегда связан адрес 127.0.0.1. У него есть dns-имя – *localhost*. Посмотреть привязку можно в файле `/etc/hosts`.

3.4. Getting the ip address of the device from the DHCP server

    $ hostname -I

  ![p3_6_ip_dhcp.png](screenshots/part_3/p3_6_ip_dhcp.png)

  > [Dynamic Host Configuration Protocol (DHCP)](https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/dhcp-top) is a client/server protocol that automatically provides an Internet Protocol (IP) host with its IP address and other related configuration information such as the subnet mask and default gateway.
  >
  > [DHCP](https://ru.wikipedia.org/wiki/DHCP) (англ. Dynamic Host Configuration Protocol — протокол динамической настройки узла) — сетевой протокол, позволяющий сетевым устройствам автоматически получать IP-адрес и другие параметры, необходимые для работы в сети TCP/IP. Данный протокол работает по модели «клиент-сервер».

3.5. Defining and displaying the external ip address of the gateway (ip) and the internal IP address of the gateway, aka default ip address (gw)

* Some ways to find out external ip address of the gatewey

      $ wget -qO - icanhazip.com
      $ curl icanhazip.com
      $ curl ifconfig.me

  ![p3_7_external_ip.png](screenshots/part_3/p3_7_external_ip.png)

  > icanhazip.com - the site that returns an ip as a string.

* Defining internal ip address of the gatewey

      $ ip route | grep default
    
  ![p3_8_internal_ip.png](screenshots/part_3/p3_8_internal_ip.png)

3.6. Setting static (manually set, not received from DHCP server) ip, gw, dns settings (use public DNS servers, e.g. 1.1.1.1 or 8.8.8.8).

* Changing ip, gw, dns settings in `/etc/netplan/00-installer-config.yaml`

      $ sudo vim /etc/netplan/00-installer-config.yaml

  ![p3_9](screenshots/part_3/p3_9.png)

  before:

  ![p3_10_vim_before.png](screenshots/part_3/p3_10_vim_before.png)

  after:

  ![p3_11_vim_after.png](screenshots/part_3/p3_11_vim_after.png)

* Changes checking and applying

      $ sudo netplan apply

  ![p3_12_changes_applying.png](screenshots/part_3/p3_12_changes_applying.png)

3.7. Rebooting the virtual machine.
<!-- Making sure that the static network settings (ip, gw, dns) correspond to those set in the previous point. -->
<!-- - Describe in the report what you have done to complete all seven points (you can do it in text or with screenshots).
- Successfully ping 1.1.1.1 and ya.ru remote hosts and add a screenshot of the output command to the report. There should be "0% packet loss" phrase in command output. -->

* Rebooting

      $ reboot

* Ping checking

      $ ping 1.1.1.1

    ![p3_13_ping_1-1-1-1.png](screenshots/part_3/p3_13_ping_1-1-1-1.png)

      $ ping ya.ru

    ![p3_14_ping_ya-ru.png](screenshots/part_3/p3_14_ping_ya-ru.png)


## Part 4. OS Update

**== Task ==**

##### Update the system packages to the latest version
- After updating the system packages, if you enter the update command again, a message should appear saying there are no updates.
- Add a screenshot of this message to the report.

**== Solution ==**

* Sync the package index files from their sources via Internet

      $ sudo apt update
  
  ![p4_1_update.png](screenshots/part_4/p4_1_update.png)

* Installing the newest versions of all installed packages

      $ sudo apt upgrade

  ![p4_2_upgrade_process.png](screenshots/part_4/p4_2_upgrade_process.png)

* Updates checking

      $ sudo apt update

  ![p4_3_all_packeges_are_up_to_date.png](screenshots/part_4/p4_3_all_packeges_are_up_to_date.png)


## Part 5. Using the **sudo** command

**== Task ==**

##### Allow user created in [Part 2](#part-2-creating-a-user) to execute sudo command.
- In the report explain the *true* purpose of sudo command (don’t write about the fact that this word is "magic" one).
- Change the OS hostname via the user created in [Part 2](#part-2-creating-a-user) (using sudo).
- Add screenshot with changed hostname to the report.

**== Solution ==**

5.1. What is `sudo`? Sudoku maybe?

  > `sudo` - Substitute User and do - is a command-line utility for Unix and Unix-based operating systems such as Linux and macOS. The utility provides an efficient way to temporarily grant users or user groups privileged access to system resources so that they can run commands that they cannot run under their regular accounts. Users can even be granted permissions to run commands under the root account -- the most powerful account on Unix-like systems. `Sudo` also logs all commands and arguments so that administrators can track the behavior of `sudo` users.
  >
  > Назначение утилиты `sudo` — выполнение команды от имени другого пользователя, обычно от root. Смысл выполнения команды от root в том, что у него повышенные права доступа и, применяя `sudo`, обычный пользователь может выполнить те действия, на которые у него недостаточно прав.
  >
  > Она позволяет легко контролировать доступ к важным приложениям в системе. По умолчанию, при установке Ubuntu первому пользователю (тому, который создаётся во время установки) предоставляются полные права на использование sudo. Т.е. фактически первый пользователь обладает той же свободой действий, что и root.

5.2. Adding task2_user to sudo group

* Adding task2_user to sudo group

      $ sudo ussermod -aG sudo task2_user

  ![p5_1_usermod_sudo.png](screenshots/part_5/p5_1_usermod_sudo.png)

* Checking task2_user is in sudo group

      $ cat /etc/group | grep sudo
  
  ![p5_2_sudo_group.png](screenshots/part_5/p5_2_sudo_group.png)

5.3. Change the OS hostname via the task2_user

* Switching to task2_user

      $ su - task2_user

  ![p5_3_switch_to_task2_user.png](screenshots/part_5/p5_3_switch_to_task2_user.png)

* Changing OS hostname by task2_user

      $ sudo hostname task5_host
      $ hostname
  
  ![p5_4_1_new_hostname.png](screenshots/part_5/p5_4_1_new_hostname.png)

## Part 6. Installing and configuring the time service

**== Task ==**

##### Set up the automatic time synchronisation service.
- Output the time of the time zone in which you are currently located.
- The output of the following command must contain `NTPSynchronized=yes`: \
  `timedatectl show`
- Add screenshots of the correct time and command output to the report.

**== Solution ==**

6.1. Current time and timezone output

    $ timedatectl

  ![p6_1_current_timezone.png](screenshots/part_6/p6_1_current_timezone.png)

6.2. 

    $ timedatectl show | grep NTPSynchronized

  ![p6_2_ntpsync.png](screenshots/part_6/p6_2_ntpsync.png)

## Part 7. Installing and using text editors

**== Task ==**

##### Install **VIM** text editor (+ any two others if you like **NANO**, **MCEDIT**, **JOE** etc.)

##### Using each of the three selected editors, create a *test_X.txt* file, where X is the name of the editor in which the file is created. Write your nickname in it, close the file and save the changes.
- Add screenshots to the report:
    - Of each editor with the contents of the file before closing.
- Write down in the report what you have done to exit with the changes saved.

##### Using each of the three selected editors, open the file for editing, edit the file by replacing the nickname with the "21 School 21" string, close the file without saving the changes.
- Add screenshots to the report:
    - Of each editor with the contents of the file after editing.
- Write down in the report what you have done to exit without saving the changes.
##### Using each of the three selected editors, edit the file again (similar to the previous point) and then master the functions of searching through the contents of a file (a word) and replacing a word with any other one.
- Add screenshots to the report:
    - Of each editor with word search results.
    - Of each editor with commands entered to replace a word with another.

**== Solution ==**

7.1. Text editors installing

    $ sudo apt install vim
    $ sudo apt install nano
    $ sudo apt install mcedit

7.2. Text files creating and filling with some text

<details><summary>VIM</summary>

      $ vim test_vim.txt

  > press `i` to switch insert mode on

  ![p7_4_test_vim.png](screenshots/part_7/p7_4_test_vim.png)

  > press `Esc` to switch insert mode off
  >
  > input `:wq` to close the file and save the changes

</details>

<details><summary>NANO</summary>

      $ nano test_nano.txt
    
  ![p7_5_test_nano.png](screenshots/part_7/p7_5_test_nano.png)

  > press in sequence: `control+X` -> `y` -> `Enter` to close the file and save the changes

</details>

<details><summary>MCEDIT</summary>

      $ mcedit test_mcedit.txt
    
  ![p7_6_test_mcedit.png](screenshots/part_7/p7_6_test_mcedit.png)

  > press `F2` -> `Enter` to save the changes
  >
  > press `F10` to close the file
  >
  > OR press `Esc` -> `y` to close the file and save the changes

</details>

7.3. Text files editing and closing without saving the cheanges

<details><summary>VIM</summary>

      $ vim test_vim.txt

  > press `i` to switch insert mode on

  ![p7_7_vim_no_save.png](screenshots/part_7/p7_7_vim_no_save.png)

  > press `Esc` to switch insert mode off
  >
  > input `:q!` to close the file without saving the changes

</details>

<details><summary>NANO</summary>

      $ nano test_nano.txt
    
  ![p7_8_nano_no_save.png](screenshots/part_7/p7_8_nano_no_save.png)

  > press in sequence: `control+X` -> `n` to close the file without saving the changes

</details>

<details><summary>MCEDIT</summary>

      $ mcedit test_mcedit.txt
    
  ![p7_9_mcedit_no_save.png](screenshots/part_7/p7_9_mcedit_no_save.png)

  > press `F10` -> `n` to close the file without saving the changes
  >
  > OR press `Esc` -> `n` to close the file without saving the changes

</details>

7.4. Text searching and text replacing in the files

<details><summary>VIM</summary>

  ![p7_10_vim_search.png](screenshots/part_7/p7_10_vim_search.png)

    :s/21 /21_

  ![p7_11_vim_replace.png](screenshots/part_7/p7_11_vim_replace.png)

</details>

<details><summary>NANO</summary>

  > press `control+W` and input pattern to search

  ![p7_12_nano_search.png](screenshots/part_7/p7_12_nano_search.png)

  > press `control+\` -> input pattern -> `Enter` -> input new text -> `Enter`
  >
  > press:
  >> `y` to replace current instance
  >>
  >> `n` to miss current instance
  >>
  >> `a` to replace all matches
  >>
  >> `control+C` to cancel the replace mode

  ![p7_13_nano_replace.png](screenshots/part_7/p7_13_nano_replace.png)

</details>

<details><summary>MCEDIT</summary>

  > press `F7` to switch search mode on
  >
  > input pattern
  >
  > set searching mode
  >
  > press:
  >> `o` to find first match
  >>
  >> `f` to find all matches
  >>
  >> `c` or `Esc` to cancel the search mode

  ![p7_14_mcedit_search.png](screenshots/part_7/p7_14_mcedit_search.png)

  > press `F4` to switch replace mode on
  >
  > input search string
  >
  > input replacement string
  >
  > select replacement settings
  >
  > press:
  >> `o` to replace first match
  >>
  >> `c` or `Esc` to cancel the replace mode

  ![p7_15_mcedit_replace.png](screenshots/part_7/p7_15_mcedit_replace.png)

</details>
