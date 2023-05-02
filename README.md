# Creating a wireGuard VPN with Pihole & Unbound
# THIS IS A WORK IN PROGRESS
Hi there! So for a little project I wanted to utilize a cloud compute server to make a VPN. 
Since this was just a small project and nothing for commerical use, I was looking for a free option and stumbled upon this repo:
https://github.com/ripienaar/free-for-dev#major-cloud-providers

In the repo, you can see all the major cloud providers and their respective free tier limits. I ended up going with Oracle Cloud but the results should be the same for any cloud provider.

Once you create an account, look into creating a cloud compute instance and installing Linux. I am unsure the different Linux flavors, but I chose Ubuntu since thats what Im most familar with. Dont forget to get the private key to connect to the server! It will take a bit for the server to setup, but once it is, ssh into it. For this you can use terminal (macOS or linux) or powershell (windows) or PuTTy (macOS or windows). I personally used PuTTy since that is what Im used to.

However, I was running into issues, turns out PuTTy uses its own type of keys so I had to use PuTTy Gen to convert the private key to a PuTTy usable format.
Then I was finally able to connect to the server.


## 1. Updating Linux and Figuring Out Where to Store Everything  
Let's update your Linux install using: 
```
sudo apt update -y
```
followed by: 
```
sudo apt upgrade -y
```

You might get a message to reboot since somethings may depend on old kernels, so run: 
```
reboot
```

It should kick you out as you lose connection. We just need to wait for your server to come back online then we can SSH back into it.
### Optional Start
Now this next part is optional, but I like everything in one folder for neatness sake. In the case of wireGuard, I want everything wireGuard related in the wireGuard folder, so I switched to that folder using: 
```
cd /etc/wireguard/
```
NOTE: I could not get access to the wireGuard folder, even with sudo, so I switched to root user using the following command
```
sudo su -
```
Then switched to the wireGuard folder using the command listed before.

However, you can do all this in the starting page, so you dont have to do anything. However, if you do need to get back to the landing page, you use:
```
cd /home/<userName>/
```

### Optional End

Once you figure out where you want everything. We can start installing Wireguard.

## 2. Installing wireGuard
We can install wireguard to Ubuntu using 
```
sudo apt install wireguard
```

### 2.1 Setting up the wireGuard Server
Now we need to create keys that will be used for securely connecting to our wireGuard. Run the following to create them, chosing a name for the files:
```
wg genkey | tee <nameOfServerPrivateKey> | wg pubkey > <nameOfServerPublicKey>
```

now lets see the private key using
```
cat <nameOfServerPrivateKey>
```
and the public key
```
cat <nameOfServerPublicKey>
```

Store these 2 keys somewhere, we will need them later. I used notepad.
Now lets create the server config using
```
sudo nano /etc/wireguard/wg0.conf
```

In this file put the following contents:
```
[Interface]
Address = <Private IPv4 from your cloud provider>/32
ListenPort = 51820
PrivateKey = <ServerPrivateKey>
```
Ill be honest, according to my testing, it doesnt seem to matter what IP address you put in the Address= field, but I put the one assigned from the cloud provider to ensure compatibility. 

Now we'll set wireguard as a service to start up on reboot automatically
```
systemctl enable wg-quick@wg0
```
then we start wireguard
```
systemctl start wg-quick@wg0
```
and check if we did everything right
```
systemctl status wg-quick@wg0
```

## 2.2 Setting up the Client(s)
We need to create keys for each client (or peer, the proper terminology)
```
wg genkey | tee <nameOfPeerPrivateKey> | wg pubkey > <nameOfPeerPublicKey>
```

now lets see the private key using
```
cat <nameOfPeerPrivateKey>
```
and the public key
```
cat <nameOfPeerPublicKey>
```
Again, store these as we will need them later. 

Now we have everything needed to create a config file for the client using
```
sudo nano <nameOfClient>.conf
```
and put in the following data
```
[Interface]
PrivateKey = <nameOfPeerPrivateKey>                                          
Address = <Pick a Private IPv4 that matches the scheme of the server's Private IPv4, so a server w/ 10.0.0.whateverNumber will have 10.0.0.whateverNumberlong as it is different>/32
DNS = 1.1.1.1, 8.8.8.8

[Peer]
PublicKey = <nameOfServerPublicKey> 
AllowedIPs = 0.0.0.0/0
Endpoint = <Private IPv4 from your cloud provider>:51820
```
Keep note of the Address you put under [Interface], we're not done with it yet







# Verify
Check the following links:
https://dnsleaktest.com/
https://ipleak.net/

