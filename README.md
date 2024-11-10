# QoS for Multimedia using SDN

This project implements a Quality of Service (QoS) solution for multimedia streaming applications using Software-Defined Networking (SDN). The goal is to enhance user experience by prioritizing multimedia traffic, managing network congestion, reducing latency. This approach ensures reliable, high-quality streaming for applications like video conferencing and streaming services, regardless of network conditions.


## Video Demo 

You can watch the demo at https://www.youtube.com/watch?v=-K9sdOPxQd0.

## Installation

1. **Install Python 3.8:**
   ```shell
   sudo apt-get update
   sudo apt-get install python3.8 python3.8-venv python3.8-dev
   ```
   Python 3.8 is required because the Ryu library is not compatible with newer Python versions.

2. **Install ryu:**
    ```shell
    pip3.8 install ryu
    ```

3. **Uninstall the existing eventlet version:**
    ```shell
    pip3.8 uninstall eventlet
    ```

4. **Install supported eventlet version:**
    ```shell
    pip3.8 install eventlet==0.30.2
    ```
    
    Eventlet must be uninstalled because Ryu is only compatible with version 0.30.2 and does not support the latest Eventlet version.

5. **Install mininet:**
    ```shell
    sudo apt-get install mininet
    ```

6. **Install Open vSwitch:**
    ```shell 
    sudo apt-get install openvswitch-switch
    ```

7. **Install Ostinato:**
    ```shell
    sudo apt-get install ostinato
    ```

8. **Install VLC:**
    ```shell
    sudo apt-get install vlc
    ```

9. **Install xterm:**
    ```shell
    sudo apt-get install xterm
    ```
    
## Run Locally

Clone the project

```shell
git clone https://github.com/abhijitch1/QoS-for-Multimedia-using-SDN.git
```
 **Create a Mininet Topology**

   Open a terminal and create a Mininet network with three switches in a linear topology using the following command:
```shell
sudo mn --topo linear,3 --mac --switch ovsk --controller remote -x
```

 **Set Switch Protocols to OpenFlow 1.3**

   In the `xterm` windows that open, locate the terminals for    each switch (`s1`, `s2`, and `s3`). For each switch, enter the command to set it to use OpenFlow protocol version 1.3:

   - For switch `s1`
        ```shell
        ovs-vsctl set Bridge s1 protocols=OpenFlow13
        ```
   - For switch `s2`
        ```shell
        ovs-vsctl set Bridge s2 protocols=OpenFlow13
        ```
   - For switch `s3`
        ```shell
        ovs-vsctl set Bridge s3 protocols=OpenFlow13
        ```

   This configures all switches in the network to use OpenFlow version 1.3.

   **Set IP addresses for all of the hosts**

   In the `xterm` windows that open, locate the terminals for each host (`h1`,`h2`, and `h3`). For each host, enter the commands to set it to an IP address of our choice:

   - For host `h1`
        ```shell
        ip addr del 10.0.0.1/8 dev h1-eth0
        ip addr add 172.16.20.10/24 dev h1-eth0
        ```

   - For host `h2`
        ```shell
        ip addr del 10.0.0.2/8 dev h2-eth0
        ip addr add 172.16.10.10/24 dev h2-eth0
        ```
   - For host `h3`
        ```shell
        ip addr del 10.0.0.3/8 dev h3-eth0
        ip addr add 192.168.30.10/24 dev h3-eth0

This allows the hosts to have the specified IP addresses.

**Run the ryu rest router:**

In the `xterm` windows that open, locate the terminal for controller `c0` and enter the following command to use ryu rest router:
```shell
ryu-manager ryu.app.rest_router
```

**Run the bash script in the repository:**

The script.sh must be installed and the permissions for execution must be added to it with `chmod +x script.sh`. This script will contain addresses that are needed to be added in order for them to communicate between one another.

**Set default routes for all of the hosts:**

In the `xterm` windows of the hosts(`h1`,`h2` and `h3`), enter the following command for it to have the default switch to send data to:

   - For host `h1`
        ```shell
        ip route add default via 172.16.20.1
        ```

   - For host `h2`
        ```shell
        ip route add default via 172.16.10.1
        ```
   - For host `h3`
        ```shell
        ip route add default via 192.168.30.1
        ```


**Run the bash script1 from the repository:**

The script1.sh must be installed and the permissions for execution must be added to it with `chmod +x script1.sh`. This script will contain default and static routes for each of the router.

These commands till now will create a virtual network as depicted in the image below.

<img width="511" alt="network" src="https://github.com/user-attachments/assets/553060ee-c0e3-4d02-b608-ce0bdba3e156">



**Generating artificial traffic in the network:**

Open the `xterm` window of `h3`, and enter the following command:

```shell
ostinato &
```

This will open the window as follows:

<img width="1470" alt="ostinato 1" src="https://github.com/user-attachments/assets/c85fdda1-7a74-4d6f-8b62-4e39373f62c4">


Upon right-clicking in the streams area, you'll see an option to add a stream, which allows you to generate artificial traffic according to your requirements. A window will appear, as shown in the images below. Please select the options as indicated in the images.

<img width="1470" alt="ostinato 2" src="https://github.com/user-attachments/assets/6fd4068a-9355-4386-81c4-77d6fbbe2bf8">
<img width="1470" alt="ostinato 3" src="https://github.com/user-attachments/assets/31df60aa-a770-44f9-b81a-ed98680cd9cf">
<img width="1470" alt="ostinato 4" src="https://github.com/user-attachments/assets/34c38b4e-6a67-4022-86cb-fe6a24efcdf0">
<img width="1470" alt="ostinato 5" src="https://github.com/user-attachments/assets/c14742d2-bb96-4e1c-9906-99126c8d11af">

Now start the transmissions in ostinato.





**Stream video using vlc:**

Open the `xterm` window of `h1` and `h2`, and enter the following command to open vlc:

```shell
vlc &
```

This will open the windows for corresponding hosts as follows:

<img width="1470" alt="vlc 1" src="https://github.com/user-attachments/assets/c57ec3ad-4183-4e6e-96db-f90df8105a6c">


Configure the vlc window of host `h2` to receive the sample video from host `h1` as follows:

<img width="1470" alt="vlc 2" src="https://github.com/user-attachments/assets/9ece54c3-8ccf-409c-8839-9e47b5f7e18b">
<img width="1470" alt="vlc 3" src="https://github.com/user-attachments/assets/2287fd27-b999-40b4-bbe8-470f994aa28e">


Configure the vlc window of host `h1` to stream a sample video as follows:

<img width="1470" alt="vlc 4" src="https://github.com/user-attachments/assets/c50f79ee-a7de-4e70-b23d-1af1201610ff">
<img width="1470" alt="vlc 5" src="https://github.com/user-attachments/assets/1dc239ec-2f8b-4c75-b0d3-91552ca278b9">
<img width="1470" alt="vlc 6" src="https://github.com/user-attachments/assets/94ac1300-2fe6-4b62-a73e-8b8ac8de205b">
<img width="1470" alt="vlc 7" src="https://github.com/user-attachments/assets/10e28742-969b-4ad9-95de-85f6177e0665">
<img width="1470" alt="vlc 8" src="https://github.com/user-attachments/assets/aa561147-a641-4a2a-afbd-96d7cad2fca2">
<img width="1470" alt="vlc 9" src="https://github.com/user-attachments/assets/c2c12a54-e407-45f1-9915-0be061031ee4">


After configuring the vlc windows of both the hosts, we can see that video is being streamed by host `h1` which is being received by host`h2` as shown below:


<img width="1470" alt="vlc 10" src="https://github.com/user-attachments/assets/b9c2e0a6-9332-4393-a093-18c2f24b3632">




Repeat the previous steps, but this time update the rest_router.py file included with Ryu by replacing it with the version from the repository. This will allow you to understand the QoS provided by our application.

**Steps to change the rest_router.py from ryu:**

Use the `find` command of linux to find the `rest_router.py` like this:

```shell
find -name rest_router.py
```

Once you have found the `rest_router.py` file within the Ryu package, replace it with the version from your repository. You can use commands like `mv` to accomplish this.









## Authors

- [Chunduri Abhijit](https://www.github.com/abhijitch1)
- [Boda Yashwanth](https://www.github.com/yashwanthboda)
- [Raman Sharma](https://www.github.com/ramansharma829455)
- [Ambali Yasho Vardhan](https://www.github.com/yashovardhan2004)
- [Majji Harsha Vardhan](https://www.github.com/harsha-050)
- [Polasa Hariharan](https://www.github.com/notapro-I)


