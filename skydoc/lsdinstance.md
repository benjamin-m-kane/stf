# Setting Up LSD Instances

1. Login to http://lsd.bskyb.com/horizon using your user sign on credentials:

<img src="https://github.com/sky-uk/stf/blob/master/res/sky/image07.png?raw=true" alt="LSD">

The following screen is displayed providing a summary of the server:
<img src="https://github.com/sky-uk/stf/blob/master/res/sky/image06.png" alt="LSD">
2. Select ‘Instances’ in the left hand menu and click ‘+ Launch Instance’:

<img src="https://github.com/sky-uk/stf/blob/master/res/sky/image11.png?raw=true" alt="LSD">

3. Set the availability zone to ‘AZIV’, provide a Name for the instance and flavor to ‘m1.smaller’. Set ‘Instance Boot Source’ to ‘Boot from image’ and in the drop down menu select the image ‘CoreOS-633.1.0 (400.9 MB).


4. Select the access and security tab and click ‘+’ to add a new key pair. 

5. Provide a key pair name. Open a terminal and enter the command `ssh-keygen -t rsa -f cloud.key`. Enter `cat cloud.key.pub` and copy the key text into the ‘Public Key’ box.

6. In the ‘Networking Tab’ drag and drop ‘STF-untrusted-3320’ into the ‘Selected Networks’ box and then launch the instance.

7. Assign floating IP by selecting ‘more’ > ‘assign floating IP’ for the newly added server in the table. Set the Pool to ‘vlan3320’ and then click ‘Allocate IP’.  Generate a new key and select ‘Associate Floating IP’.

8. Test the server by using SSH to access the machine:
SSH into machine: `ssh –i <key name> <servername>@<YOUR_FLOATING_IP>`
 e.g.  `ssh –i cloud.key core@10.92.71.242`

Alternatively ping the floating IP: `ping <YOUR_FLOATING_IP>`
