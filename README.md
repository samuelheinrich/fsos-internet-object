# fsos-internet-object
CLI Script to create an "Internet Object"

![image](https://user-images.githubusercontent.com/16579232/221982347-637ff6f6-c217-469a-864e-957b59d0ed8c.png)



What is it about?
Fortigate offers a variety of ways to route traffic towards the internet. Besides static routes, dynamic objects are also supported, such as FQDN objects or Internet Services.

![image](https://user-images.githubusercontent.com/16579232/221981630-e8187ade-ea17-431f-913e-54be9d868917.png)

If one wants to route "everything" towards the "Internet," typically a default route 0.0.0.0/0 or "ANY -> Zone WAN" is used.

Problem?
Usually, this does not cause any problem because, in static and policy-based routing, the most specific route is always applied.

For example:

10.0.0.0/8 -> 1.1.1.1
10.0.0.0/24 -> 1.1.1.2 <- Wins against 10.0.0.0/8

Unfortunately, this has a significant drawback in conjunction with SD-WAN rules, as it does not allow specifying zones. Therefore, "ANY" also includes private subnets that are supposed to be routed internally. Moreover, the concept of "more specific" does not exist here.

![image](https://user-images.githubusercontent.com/16579232/221981909-cfa9b359-d0e0-4f04-8bc4-cd020450ce1a.png)

In this case, it would be better to have a pure "Internet Object" that does not include the local subnets.

![image](https://user-images.githubusercontent.com/16579232/221983808-0fd41e1a-e531-4894-aac2-148a966e2430.png)


Such an object would need to include all IP ranges from 0.0.0.0 to 255.255.255.255, but exclude all addresses that are not relevant for the internet.

Here are the ranges we're talking about:
Besides the classic private address spaces:

Class A: 10.0.0.0 to 10.255.255.255
Class B: 172.16.0.0 to 172.31.255.255
Class C: 192.168.0.0 to 192.168.255.255
There are also other special addresses that are not routable:

192.0.2.0/24 Assigned as TEST-NET-1, for documentation and examples
198.51.100.0/24 Assigned as TEST-NET-2, for documentation and examples
203.0.113.0/24 Assigned as TEST-NET-3, for documentation and examples
224.0.0.0/4 Designated for private multicast (formerly Class D network.)
Technically, 127.0.0.0/8 would also be part of this list, but since these addresses do not communicate outside of a device by nature, they can be disregarded.

The challenge is to define the subnets below and above these ranges in such a way that they can be technically mapped for routing.

This repository provides the code to import the object on the Fortigate.
