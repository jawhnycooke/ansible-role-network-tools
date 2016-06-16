# ansible-role-network-tools
Group of roles to make network changes
So far a I have the following roles:
- ping: it will connect to all the devices and it would try to ping all GW.
- qos: to implement QoS configuration with ACL, Class-map, Policy-Map:
  - acl: apply ACL using **ios_template** depending in the position of the network device ```device_position```.
  - class_map: apply Class-Map using **ios_template** depending in the position of the network device ```device_position```.
  - policy_map: apply Policy-MAP using **ios_template** depending in the position of the network device ```device_position```.

So far the device_position are:
 - wand_edge
 - office_core
 - dc_core

This position is declared with the var ```device_position``` in the inventory file.
#### IMPORTANT

- The username and password are defined as envar ```ANSIBLE_NET_USERNAME``` and ```ANSIBLE_NET_PASSWORD```.
- For QoS: The logic here is that depdending on the role, the main task is going to look for var ```device_position```.yml, is going to use include to excecute the task in that file and then this one is going to copy a correct template. 

####To-Do:

So far the implementation of **ios_template** does not cover replacing for exact match of the string for example:
If the device is configured like this:

``` 
ip access-list extended Test
 deny udp any any
 permit ip any any
```
 
And the ansible template is:
``` 
ip access-list extended Test
 permit ip any any
 ```
That is totaly fine for ansible and it will not remove the line **deny udp any any**, still don't have a good workaround for this other than
use **ios_config** to remove completely the ACL and then re-apply with this current logic.

