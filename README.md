# private_cloud_openstack_RDO
How to build a private cloud using openstack
<img width=”800” alt=”image” src=https://user-images.githubusercontent.com/128939047/227747175-1309c1d6-89a7-43cf-9460-5ccefbcc81a4.jpeg>
## A Project on how to build a private cloud using Openstack RDO.
OpenStack is a free, open standard cloud computing platform. It is mostly deployed as infrastructure-as-a-service in both public and private clouds where virtual servers and other resources are made available to users and in this project we will build Our Platform which includes full network topology and instances.
You can know more about openstack through the following link – https://docs.openstack.org/yoga/
## 1. Set Up Your Environment
In this project we used CentOS Stream 8-streamn, a single VM of MINIMUM 12 GB RAM, 2 CPUs and 2 NICs
FQDN of machine openstack.lab.local, so we should edit /etc/hosts file.
## 2. Machine preparation :
- Enabling nested virtualization
<img width=”533” alt=”image” src=https://user-images.githubusercontent.com/76592289/227681753-7411aab1-e8da-46ab-a7e7-cbaba1d34aec.png>
## 3. Operating System Installation :
1. During the installation of the CentOS OS. Move to Install CentOS Stream 8-stream and click tab on your keyboard.
Thin Type a space to separate than the last kernel option, type net.ifnames=0 biosdevname=0
<img width=”800” alt=”image” src=https://user-images.githubusercontent.com/76592289/227681824-fa0327f3-9797-48c0-a0db-16c866cc175a.png>
This will enable old Linux NIC card naming (eth0, eth1, …etc).
2. Now you see the installation screen, Click on Network & Host.
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227682265-ecd6c7f2-dceb-472f-86d5-a97ab0df31a4.png>
2.1 First step we need to set the Host Name,
2.2
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227682323-4a5bf1cf-37ed-4b4d-bb94-a0da8cdf42bb.png>
2.3 Now, the network card got an IP from the DHCP. Configure the IP manually by clicking configure. Give the card the same fetched configuration from DHCP.
2.4 >> Disable IPV6 and configure IPV4
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227682445-502819db-cf30-4523-acd2-1677dc878f40.png>
2.3 Select the Installation Destination and choose your HDD, just click to Done button if u just have one HDD.
2.5 Click the Time and Date button and select Cairo as your local time zone, Make sure that NTP on the top right is enabled, click on the gear icon and wait for a moment to make sure it is syncing with centos public NTP servers.
2.6
2.5 For Software Selection, please, select Server with GUI
2.7 create root password,
2.8
2.7 create user and make him Admin,
3. Once all are set. Begin the OS installation.
4. When the installation is over Reboot and Accept the Licenses,
## 4. Prerequisites Deployment:
As a prerequisite, you have to install the needed repos and Adding to that, you must disable some services and enable the legacy network service.
The below section illustrates the actions we have to follow:
Firstly, we need to check the hostname, and check if we can reach the internet or not, and switch to be root.
1. Switch to root,
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227682865-001f0fa0-b3d2-47dd-8087-c3ef86d5ee12.png>
2. Add the hostname to /etc/hosts and make an alias for it,
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227683541-db6ba19a-08d2-479f-a336-9c74aac57a8a.png>
3. Disable and stop firewalld
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227684180-7832b68b-adbe-41d5-aacd-9047ce67835c.png>
4. Disable SeLinux
Sed -i ‘s/SELINUX=.*/SELINUX=disabled/’ /etc/selinux/config
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227685054-b0e9b7b0-d5fb-498f-abfa-ec8c652b009f.png>
5. Disable and stop Networkmanager
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227685745-8d31dd68-a85b-472d-b121-f260cd7d371b.png>
6. Download network-scripts
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227685998-32074c90-72b0-436e-9da2-58d009619acb.png>
7. Enable and Start network-scripts
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227688020-8630af38-0e9e-4ab5-a5d1-de3b00c1d4bc.png>
8. Enable power tools and install yoga,
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227688059-0b458126-e537-42c5-ad58-6ad078467088.png>
9. Update the system,
```
Dnf -y update
```
10. Install Packstack
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227688823-3f62669d-f64b-42fc-943b-ccd2b3dfcf95.png>
11. Generate answer file,
```
Packstack –gen-answer-file=/root/answers.txt –os-neutron-l2-agent=openvswitch
--os-neutron-ml2-mechanism-drivers=openvswitch –os-neutron-ml2-tenant-network-types=vxlan –os-neutron-ml2-type-drivers=vxlan,flat
--provision-demo=n –os-neutron-ovs-bridge-mappings=extnet:br-ex –os-neutron-ovs-bridge-interfaces=br-ex:eth0
--keystone-admin-passwd=redhat –os-heat-install=n
```
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227688975-6e84bfe8-ec82-44e7-8bbd-8c2ca8b380c0.png>
12. Start the installation,
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227689149-34e37516-f03c-433c-8130-6dfff788afc2.png>
Wait for the installation to finish, you can access the openstack dashboard browse to 192.168.140.131
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227689545-0e1d9fe6-d7ef-4aa2-b88f-ea303e79f718.jpg>
## 5. Validation:
Open the dashboard in your browser, and enter the User Name and the password to Sign In.
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227689810-d28657c5-bf6b-4605-812e-34bd5735394d.PNG>
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227689951-9dead25a-d272-4d8b-92f7-50bd73e70d1d.PNG>
1.1 Create a Private Network and then Create a Private Subnet:
1.2
```
1- Neutron net-create Internal
2- Openstack subnet create –network Internal –subnet-range 192.168.10.0/24 –dhcp Internal_subnet
```
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227736798-7cde2801-82d3-4416-9c44-d7b3dcb41912.png>
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227736748-d94a175b-9e5f-45be-b3c6-a6c44fef1e93.png>
1.3 Create an External Network and then Create an External Subnet
1.4
```
1- Openstack network create External –provider-network-type flat –provider-physical-network extnet –external
2- Openstack subnet create External_subnet –no-dhcp –allocation-pool start=192.168.140.141,end=192.168.140.151 –gateway 192.168.140.2 –network External –subnet-range 192.168.140.0/24
```
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227736984-bd2386d2-ae3c-49d3-ade9-40ce7201d34f.png>
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227737103-5e44516e-503d-4666-9242-dcb11a0530ef.png>
1.3 Create a Router: To connect the external network and the internal network to each other.
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227737224-ac1a263b-1dd5-43b8-a71e-4a9d8cb9eb76.png>
Set Router Gateway, Using External Network:
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227737307-e05f8f20-d882-4005-8ad6-46ea3964f923.png>
Set Router Gateway, Using Internal Network
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227737624-f43ad665-6f3a-4efb-9e45-3a41d9e8b313.png>
1.5 validate network topology from the GUI
1.6
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227737447-2f7a165e-f8a0-4a9c-8932-956ec6f51b49.PNG>
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227738399-3f575f38-3ca5-4cc9-a068-826856bdef1d.png>
2. Install image file and Create image using glance component:
```
Curl -L http://download.cirros-cloud.net/0.3.4/cirros-0.3.4-x86_64-disk.img | glance image-create --name=’cirros image’ –visibility=public –container-format=bare –disk-format=qcow2
```
>> CirrOS is a minimal Linux distribution that was designed for use as a test image on clouds such as OpenStack Compute.
<img width=”420” alt=”image” src=https://user-images.githubusercontent.com/76592289/227719344-2b403d30-5e61-4ccd-af05-96ddc88cb12f.png>
2.1 validate the image from the GUI
>> project->compute->images
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227719563-b3518cc1-c485-4c91-a4ff-4a99cc2d7f61.png>
3. create the Instance
```
Openstack server create –image ‘cirros image’ –flavor m1.tiny –network Internal server2
```
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227735989-d6d9445f-5ea6-4790-8fae-dd217c112517.png>
2.1 Create FloatingIP, to reach our instance from the Internet
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227736259-b4d94778-a2df-43d5-8ba9-7926b67acdb3.png>
2.2 Assign the FloatingIP to the Instance_Port
>> We know the IP for this Instance from GUI
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227738009-c2b51c48-3fd7-4bbf-9b7e-adadd56a57d5.png>
Validate the Instance from the GUI:
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227736139-b3f6a1c1-d370-4e19-9adb-9ac9bbe97a75.png>
3. Add two rules in the default security group to enable ssh port and ICMP
>> Project→ Network→ Security Groups → Manage Rule
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227723242-88d9da4e-198c-41d5-9817-77fb16284463.png>
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227723332-143096ba-60cc-424c-bb7a-34a0e4d67945.png>
Validate two rules added
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227723506-781e77e7-4917-430a-83cf-1eda801c648d.png>
## 6. Connect to your instance by SSH,
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227738683-a9d5d2bb-e0fe-43ab-a9fb-90bca5e179c2.png>
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227738842-a679c862-6bc7-4ab6-9108-da693eb2dc77.png>
6.1 Ping from the instance to the host,
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227738890-672444be-e195-4953-a93e-290954e4a982.png>
6.2 exit from ssh connection and then ping from host to instance
<img width=”520” alt=”image” src=https://user-images.githubusercontent.com/76592289/227739472-ec44dc8e-594e-4b67-8de6-9e04ab90f851.png>

