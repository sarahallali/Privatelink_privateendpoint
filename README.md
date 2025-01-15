In this demo you will learn and practice service endpoint, private link service and private endpoint.

In this repository you will find two architecture a starting architecture that you practice on top of it the demo steps that you could read below. The second architecture named final architecture is a reference architecture that you could use to check if you implemented correctly all the demo steps. 

In this demo steps you will practice to : 

1. Implement a service endpoint on a storage account. 
2. Implement a private endpoint to a storage account.  
3. Implement a private link service and a private endpoint using a load balancer and VMs. 

Course catalogue : AZ-700, AZ-500, AZ-305
Starting architecture: 

![Image](https://github.com/user-attachments/assets/a8fe2636-33ff-4c0c-8050-6cfb6d14291b)

The architecture has 
1.	A first VM “VMConsumer” that we will configure to access using private link to a storage account and uses another private link to access to a web app VM through a Standard load balancer.  This VM has public IP address connected to it. The VM has a network security group “VMconsumer-nsg” attached to the subnet.
2.	A second virtual machine is “Webappproducerapp” where we installed a basic web app to be accessed in the demo later on. The VM has a network security group “webappproducerapp-nsg” attached to the subnet.
3.	A storage account “storageaccountdemoplink” that has inside it a blob with a picture that we will access in different way through the network. 
4.	A load balancer “LB_private_link” that load balance the traffic to the “Webappproducerapp” VM . We need this load balancer to access the web app in the VM via a private link.

Pre demo setup: 

1.	You need to configure defender for cloud to access the “VMConsumer” with a just in time policy. Or you can add a Bastion or an RDP network rule for your machine ip adress. 
2.	In the sitting of the storage account activate anonymous access and access key to do the demo.
3.	Deploy the "Designer.jpeg" picture in a blob container in the storage account and set the access of the container to Blob (anonymous read access for blobs only).
   
Demo steps : 

Part I: Service Endpoint and Private endpoint with Storage account:

1.	Go to the Networking section in the storage account. In the “firewall and virtual networks” menu show that it is enabled from all networks. 
2.	Access the blob in the storage via internet publicly. For this go to the data blob container and copy the link for the “Designer.png” picture and if needed generate a URL with a SAS signature.
3.	Go again to the Networking section in the storage account and switch now to “Enabled from selected virtual networks and IP addresses”. Create a service endpoint to the VM_ConsumerVNET myPESubnet subnet. 
4.	Show that you can no more access the Blob from the internet (your computer). 
5.	Connect from the VM Consumer to the storage via the browser with the same copied link.  Show that you can now access the blob. 
6.	Go again to the Networking section and enable access for your local machine by adding the ip address of the machine.  Show that by adding this specific address you can now access the blob via the Firewall.
7.	Now Disable the public access in the Firewall section and go to the private endpoint connections section to create a private endpoint. Add the blob as Tagged sub-resource and VMconsumer-vnet default backendSubnet. Create the private DNS as you need it for the demo. 
8.	Add the virtual network VM_ConsumerVNET to the private DNS Zone to access the private domain name.  Take this opportunity to explain why you need to add it.
9.	Connect now to the storage account from the VMConsumer. Show that now you need a different URL and now you need a private DNS. Take this as an occasion to explain the raison why and how the routing is happening.
10.	Optional step: you could add peering and access from a VM in the peered Vnet.

    
Part II: Private link service with LB
1.	Access the private link center to create a private link service. In the outbound setting refer LB_private_link as load balancer. Use LB_private_link-frontendconfig01 as frontend IP address. Use webappproducerapp-vnet/default as subnet. 
2.	Now create a private endpoint to access the private link from the consumer network. In the resource type put Microsoft.Network/privateLinkservices and choose the resource that you have just created. In the VNET choose the VMconsumer-vnet default subnet. Take note of the private ip address used. 
3.	From the VM consumer access from the browser to the web app. For this put the ip address that you noted previously. Take the opportunity to explain that now the LB is represented through a new ip address. 
Final architecture:
   
![Image](https://github.com/user-attachments/assets/fb691a32-37d0-4144-aa47-86bbd1ff1bb0)
