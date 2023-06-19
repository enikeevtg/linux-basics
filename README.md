# linux-basics
## UNIX/Linux operating systems (Basic).

Linux system installation and updates. Administration basics.

## Contents

1. [Installation of the OS](#part-1-installation-of-the-os)  
2. [Creating a user](#part-2-creating-a-user)  
3. [Setting up the OS network](#part-3-setting-up-the-os-network)   


## Part 1. Installation of the OS

* Ubuntu version checking

      cat /etc/issue

    ![p1_2_setup_17_result.png](screenshots/part_1/p1_2_setup_17_result.png)

## Part 2. Creating a user

**== Task ==**

##### Create a user other than the one created during installation. The user must be added to `adm` group.
- Add a screenshot of command call to create user.
- The new user must be in the output of the command: \
  `cat /etc/passwd`
- Add a screenshot of the command output.

**== Result ==**

* Command to create user

      $ sudo adduser <new_user_name>

    ![p2_1_adduser.png](screenshots/part_2/p2_1_adduser.png)

* Command to add new user to `adm` group

      $ sudo usermod -G adm <user_name>

    ![p2_2_usermod.png](screenshots/part_2/p2_2_usermod.png)

* New user creation checking 
      
      $ cat /etc/passwd | grep task2_user

    ![p2_3_passwd.png](screenshots/part_2/p2_3_passwd.png)

* Group checking

      $ cat /etc/group | grep adm
    
    ![p2_4_group_checking.png](screenshots/part_2/p2_4_group_checking.png)

    > sudo - Substitute User and do

## Part 3. Setting up the OS network

`-` In our world, you can't go far without the Internet. However, since we want to train you for the role of a system administrator, I'll show you a little more than just setting up a network.

`-` Before we begin, I suggest reading about network interfaces and DHCP.

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

**== Result ==**

* Setting the machine name as user-1

      $ hostnamectl set-hostname user-1

    ![p3_set_1_machine_name.png](screenshots/part_3/p3_1_set_machine_name.png)

    After machine reset

    ![p3_2_after_reset.png](screenshots/part_3/p3_2_after_reset.png)

* Setting the time zone corresponding to current location

      $ timedatectl
      $ timdatectl list-timezones | grep Moscow
      $ sudo timedatectl set-timezone Europe/Moscow
      $ timedatectl


    ![p3_3_timezone.png](screenshots/part_3/p3_3_timezone.png)

    Another way with tzselect utility:
      
      $ tzselect

    ![p3_4_tzselect.png](screenshots/part_3/p3_4_tzselect.png)

* The names of the network interfaces output using a console command

      $ ip -br link show

    ![p3_5_network_interfaces.png](screenshots/part_3/p3_5_network_interfaces.png)

    > [Virtual loopback interface](https://en.wikipedia.org/wiki/Loopback)
    > 
    > Implementations of the Internet protocol suite include a virtual network interface through which network applications can communicate when executing on the same machine. It is implemented entirely within the operating system's networking software and passes no packets to any network interface controller. Any traffic that a computer program sends to a loopback IP address is simply and immediately passed back up the network software stack as if it had been received from another device. Unix-like systems usually name this loopback interface **lo** or **lo0**.
    >
    > [lo (loopback device)](https://ru.hexlet.io/courses/linux-administration/lessons/interfaces/theory_unit) – виртуальный интерфейс, присутствующий по умолчанию в любом Linux. Он используется для отладки сетевых программ и запуска серверных приложений на локальной машине. С этим интерфейсом всегда связан адрес 127.0.0.1. У него есть dns-имя – *localhost*. Посмотреть привязку можно в файле `/etc/hosts`.

* Getting the ip address of the device from the DHCP server

    ![p3_6_ip_dhcp.png](screenshots/part_3/p3_6_ip_dhcp.png)

    > [Dynamic Host Configuration Protocol (DHCP)](https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/dhcp-top) is a client/server protocol that automatically provides an Internet Protocol (IP) host with its IP address and other related configuration information such as the subnet mask and default gateway.
    >
    > [DHCP](https://ru.wikipedia.org/wiki/DHCP) (англ. Dynamic Host Configuration Protocol — протокол динамической настройки узла) — сетевой протокол, позволяющий сетевым устройствам автоматически получать IP-адрес и другие параметры, необходимые для работы в сети TCP/IP. Данный протокол работает по модели «клиент-сервер».
