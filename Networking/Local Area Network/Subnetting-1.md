# Subnetting

Networks can be found in all shapes and sizes, ranging from small to large. Subnetting is the term given to the act of splitting up a network into smaller networks within itself. There's only a certain amount of the network available, subnetting is the decision of which devices will be designated to each part, and reserving that range of the network for use by each part.

Take a business, for example; You will have different departments such as:

- Accounting
- Finance
- Human Resources

<img src="../../_resources/dfdda87fb215cd723555bde32345e05e-1.png" alt="dfdda87fb215cd723555bde32345e05e.png" width="465" height="410" class="jop-noMdConv" style="display: block; margin: 0 auto;">

Whilst you know which department to send the information in real life, networks need to know aswell. Network administrators use subnetting to categorize and assign specific parts of a network to reflect this.

Subnetting is achieved by splitting up the number of hosts that can fit within the network, represented by a number called a subnet mask.

| octet #1 | octet #2 | octet #3 | octet #4 |
| --- | --- | --- | --- |
| 192. | 168. | 1.  | 1   |
| 0 - 255 | 0 - 255 | 0 - 255 | 0 - 255 |


An IP address is made up of four sections called octets. The same goes for a subnet mask which is also represented as a number of four bytes (32 bits) ranging from 0 to 255 (0-255).

Subnets use IP addresses in three different ways:

- Identify the network address
- Identify the host address
- Identify the default gateway

Let's split these three up to understand their purposes into the table below:

| **Type** | **Purpose** | **Explanation** | **Example** |
| --- | --- | --- | --- |
| Network Address | This address identifies the start of the actual network and is used to identify a network's existence. | For example, a device with the IP address of 192.168.1.100 will be on the network identified by 192.168.1.0 | 192.168.1.0 |
| Host Address | An IP address here is used to identify a device on the subnet | For example, a device will have the network address of 192.168.1.1 | 192.168.1.100 |
| Default Gateway | The default gateway address is a special address assigned to a device on the network that is capable of sending information to another network | Any data that needs to go to a device that isn't on the same network (i.e. isn't on 192.168.1.0) will be sent to this device. These devices can use any host address but usually use either the first or last host address in a network (.1 or .254) | 192.168.1.254 |

&nbsp;

In small networks such as at home, you will be on one subnet as there is an unlikely chance that you need more than 254 devices connected at one time. However, places such as businesses and offices will have much more of these devices (PCs, printers, cameras and sensors), where subnetting takes place.

Subnetting provides a range of benefits, including:

- Efficiency
- Security
- Full control

We'll come on to explore exactly how subnetting provides these benefits at a later date; however, for now, all we need to understand is the security element to it. Let's take the typical café on the street. This cafe will have two networks:

1.  One for employees, cash registers, and other devices for the facility
2.  One for the general public to use as a hotspot

Subnetting allows you to separate these two use cases from each other whilst having the benefits of a connection to larger networks such as the Internet.