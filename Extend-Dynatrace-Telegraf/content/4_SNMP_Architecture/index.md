## SNMP Architecture

### SNMP Concepts:
SNMP stands for **S**imple **N**etwork **M**anagement **P**rotocol and has been in use since 1988. It is used to configure/manage/monitor network components remotely and the SNMP Architecture looks like the below:
![image](../assets/images/snmp-architecture.png)

Let us jump into the runtime components of SNMP:
1. **SNMP managed device and resources**:
These are the devices on which SNMP is running and that are managed using SNMP. Whilst SNMP was initially developed to be used for switches/routers that cannot run a standard Operating System, it has now been extended to several other components (quite recently **I**nternet **o**f **T**hings). For our use-case, we will simulate a SNMP device on the AWS instance which will be our managed device. To start the managed device, navigate to **/home/ubuntu/extendDynatrace/telegraf/examples/** and run the SNMP simulator as below:

```bash
$ cd /home/ubuntu/extendDynatrace/telegraf/examples/

$ sudo ./run_simple_agent_over_tcpsocket.sh
```

> Negative
: This will start the agent on port :5555 and can respond to any queries made by the SNMP manager. Open another window in our terminal to run further commands.

2. **SNMP Manager**:
The SNMP manager is a system that controls and monitors the activities of network hosts using SNMP. They can query different SNMP devices using SNMP messages to get details of the health of devices and have the ability to control these devices by sending configuration SNMP messages. In our use-case, our telegraf would be the SNMP manager that would manage the device by querying the agent. To verify if the SNMP manager is running on your instance, execute the below command:

```bash
$ service telegraf status
```
![image](../assets/images/running-telegraf.png)

3. **SNMP agent**:
SNMP agents are the agents that respond to SNMP manager by sending the values of the requested devices to the SNMP manager. In our case, snmpd will act as agent and respond to SNMP manager which is running on the same instance. To view our SNMP agent that would respond to the SNMP manager (telegraf), run the below command:

```bash
$ service snmpd status
```

![image](../assets/images/running-snmpd.png)

4. **OID (Object Identifiers)**:
OID is a bunch of numbers separated by dots that are assigned by SNMP in order to determine an object uniquely on a device (similar to say "IP address").
So, for example, power-on or power-off button on your TV or IoT would be assigned an OID (like 1.3.6.1.2.1.2.2.1.8) by SNMP, so whenever it refers 1.3.6.1.2.1.2.2.1.8, it is referring to the power button.

5. **MIB (Management Information Base)**:
MIBs is word-based translated OIDs to make it human readable. So, in the above example, a MIB for the same OID could be something like "SAMSUNG-SYSTEM-MIB::power.0". So, in essence a MIB and OID would have one-to-one mapping.

Both MIBs and OIDs are stored physically on the device that is being monitored and are defined by the manufacturer. Often the MIBs are available online and is easily accessible on manufacturer website. In our case, we have the MIBs of our simulator stored at /usr/share/snmp/mibs/ with name **SIMPLE-MIB.txt**.

6. **SNMP Message Types/Commands**:
SNMP tools perform many functions that can be a mix of push configuration or pull information from the underlying SNMP agents. The following two commands describe the commands that SNMP manager understands and which we will use during our session:

6.1: **Get Request (snmpwalk)**: A request to retrieve the value of a variable or list of variables. When SNMP manager initiates a snmpwalk, SNMP agent responds back with value of all the requested variable. SNMP Manager can either send request for a single variable or multiple variables in a single request.

Let us now use our pre-populated variable present in our MIBs file and retrieve its value by issuing a snmpwalk command. You can check the value by running 'snmpwalk' as below,

```bash
$ snmpwalk -v 2c -c simple -M+. localhost:5555 SIMPLE-MIB::simpleInteger.0
```

![image](../assets/images/snmpwalk_0.png)

6.2 **Set Request—Sent (snmpset)**: These are the commands sent by SNMP manager to the agent to set a particular value for a variable.
Now, let us set a new value for the variable we ran snmpwalk on so we can simulate a real scenario. To do so, please open a new shell and run below command to set a new value for SIMPLE-MIB::simpleInteger.0

```bash
$ snmpset -v 2c -c simple -M+. localhost:5555 SIMPLE-MIB::simpleInteger.0 i 20
```

![image](../assets/images/simulator-set-value.png)

If we can now run 'snmpwalk' to check SIMPLE-MIB::simpleInteger.0 value, we will see the new value reflected as below:
![image](../assets/images/snmpwalk_20.png)


