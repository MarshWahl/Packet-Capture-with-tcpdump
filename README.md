# Packet-Capture-with-tcpdump

Using tcpdump to capture and analyze live network traffic from a Linux virtual machine.

## Identify Network Interfaces
1: Identify the network interfaces that can be used to capture network packet data.
Use the command "sudo ifconfig" to identify the interfaces that are available. This will return output such as the following:
![image](https://github.com/user-attachments/assets/89f632a5-c340-4e5e-b4d8-98c62c473514)

Here, **eth0** represents the ethernet network interface while **lo** represents the loopback network interface.

2: Use tcpdump to identify the interface options available for packet capture.
Use the command "sudo tcpdump -D". This command will also allow you to identify which network interfaces are available. This may be useful on systems that do not include the ifconfig command and will return output such as the following:
![image](https://github.com/user-attachments/assets/4013f077-f167-432a-9133-5609fc68d476)

## Inspect the network traffic of a network interface with tcpdump
Using tcpdump to filter live network packet traffic from the eth0 interface. Enter the command "sudo tcpdump -i eth0 -v -c5"
This command will run tcpdump with the following options:
**-i eth0**: Capture data specifically from the eth0 interface.
**-v**: Display detailed packet data.
**-c5**: Capture 5 packets of data.

This will return output such as the following:
![image](https://github.com/user-attachments/assets/29e48cfc-ba60-44f5-81d3-76962e1abf57)

This output shows on the first line: tcpdump reported that it was listening on the eth0 interface, and it provided information on the link type and the capture size in 262144 bytes.
On the next line, the first field is the packet's timestamp, followed by the protocol type (IP), and the verbose option, **-v**, has provided more details about the IP packet fields, such as TOS, TTL, offset, flags, internal protocol type (in this case, TCP (6)), and the length of the outer IP packet in bytes.
In the next section, the data shows the systems that are communicating with each other: (dd0a2a0992e9 > ...). The remaining data filters the header data for the inner TCP packet, the flags field identifies TCP flags. In this case, the **P** represents the push flag and the **period** indicates it's an ACK flag. This means the packet is pushing out data.
The next field is the TCP checksum value, which is used for detecting errors in the data. This section also includes the sequence and acknowledgment numbers, the window size, and the length of the inner TCP packet in bytes.

## Capture network traffic with tcpdump
Using tcpdump to save the captured network data to a packet capture file. Enter the command "sudo tcpdump -i eth0 -nn -c9 port 80 -w capture.pcap &"
This command will run tcpdump in the background with the following options:
**-i eth0**: Capture data from the eth0 interface.
**-nn**: Do not attempt to resolve IP addresses or ports to names.This is best practice from a security perspective, as the lookup data may not be valid. It also prevents malicious actors from being alerted to an investigation.
**-c9**: Capture 9 packets of data and then exit.
**port 80**: Filter only port 80 traffic. This is the default HTTP port.
**-w capture.pcap**: Save the captured data to the named file.
**&**: This is an instruction to the Bash shell to run the command in the background.

This will return output such as the following:
![image](https://github.com/user-attachments/assets/a5ec181b-4b88-449d-9832-6b0c9818c87a)

In this scenario, I will generate some HTTP traffic for it to capture using the command "curl opensource.google.com"

This will output such as the following:
![image](https://github.com/user-attachments/assets/e57f7bd9-6df4-4f80-8191-580fce6a0113)

And using the command "ls -l capture.pcap" to verify that the packet has been captured.

This will output such as the following:
![image](https://github.com/user-attachments/assets/4921f9e9-28a4-44ca-8a7b-137c1f050882)

## Filter the captured packet data
Using tcpdump to filter the packet header data from the capture.pcap capture file. Enter the command "sudo tcpdump -nn -r capture.pcap -v"
This command will run tcpdump with the following options:
**-nn**: Disable port and protocol name lookup.
**-r**: Read capture data from the named file.
**-v**: Display detailed packet data.

This will output such as the following:
![image](https://github.com/user-attachments/assets/e2afcc7c-9509-4299-87d9-55f1c6d0a246)

As with the **Inspect the network traffic of a network interface with tcpdump** secion, you can see the IP packet information along with information about the data that the packet contains.

Using tcpdump to filter the extended packet data from the capture.pcap capture file. Enter the command "sudo tcpdump -nn -r capture.pcap -X"
This command will run tcpdump with the following options:
**-nn**: Disable port and protocol name lookup.
**-r**: Read capture data from the named file.
**-X**: Display the hexadecimal and ASCII output format packet data. Security analysts can analyze hexadecimal and ASCII output to detect patterns or anomalies during malware analysis or forensic analysis.

This will output such as the following:
![image](https://github.com/user-attachments/assets/56249d8c-160f-4873-bf3a-2bd796c444f2)
