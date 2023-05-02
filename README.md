# Creating a wireGuard VPN with Pihole & Unbound

Hi there! So for a little project I wanted to utilize a cloud compute server to make a VPN. 
Since this was just a small project and nothing for commerical use, I was looking for a free option and stumbled upon this repo:
https://github.com/ripienaar/free-for-dev#major-cloud-providers

In the repo, you can see all the major cloud providers and their respective free tier limits. I ended up going with Oracle Cloud but the results should be the same for any cloud provider.

Once you create an account, look into creating a cloud compute instance and installing Linux. I am unsure the different Linux flavors, but I chose Ubuntu since thats what Im most familar with. Dont forget to get the private key to connect to the server! It will take a bit for the server to setup, but once it is, ssh into it. For this you can use terminal (macOS or linux) or powershell (windows) or PuTTy (macOS or windows). I personally used PuTTy since that is what Im used to.

However, I was running into issues, turns out PuTTy uses its own type of keys so I had to use PuTTy Gen to convert the private key to a PuTTy usable format.
Then I was finally able to connect to the server.


#####The first thing 
Then update your Linux install using 
```
sudo apt update -y
```
followed by 
```
sudo apt upgrade -y
```

You might get a message to reboot since somethings may depend on old kernels, so run 
```
reboot
```

It should kick you out as you lose connection, and wait for your server to come back online then SSH back into it
#Optional Start
Now this next part is optional, but I like everything in one folder for neatness sake. In the case of wireGuard, I want everything wireGuard related in this folder, which is 
```
cd /etc/wireguard/
```

However, I could not get access to this folder, even with sudo, so I switched to root user using the following command
```
sudo su -
```
