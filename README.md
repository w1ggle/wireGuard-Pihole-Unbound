# wireGuard-Pihole-Unbound

Hi! So for a little project I wanted to utilize a cloud compute server to make a VPN. 
I was looking for free tiers since this is just a small project for me so I was looking for something free and stumbled upon this repo:
https://github.com/ripienaar/free-for-dev#major-cloud-providers

Here you can see all the major cloud providers and their respective free tier limits. I ended up going with Oracle Cloud but the results should be the same for any cloud provider.

Get the cloud server setup and ssh into it. You will probably need the private key to connect so have that on handy. I used PuTTy and was running into issues, turns out PuTTy uses its own type of keys so I had to use PuTTy Gen to convert the private key to a PuTTy usable format.

Once that is done, switch to the root user using
```
sudo su -
```
I was having issues accessing files with my default user, even with sudo before commands so I just switched to root user to get around the issues

Then update your Linux install using 
```
apt update -y
```
followed by 
```
apt upgrade -y
```

You might get a message to reboot since somethings may depend on old kernels, so run 
```
reboot
```

It should kick you out as you lose connection, and wait for your server to come back online.
