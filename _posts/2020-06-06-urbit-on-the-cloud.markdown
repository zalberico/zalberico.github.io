---                                                                                                                
layout: post
title: "Urbit In The Cloud"
date: 2020-06-06
categories: essay
---

### Sailing your Urbit ship on a Digital Ocean

#### What is this guide?
The goal of this guide is to have clear and easy to follow best practices for deploying an Urbit node to a server you control in the cloud. Deploying in the cloud allows you to access your Urbit from any device.

Most Urbit users start out running their ship locally on one machine in order to play with it, but this means when your machine is offline your Urbit node is offline too (and can't get updates). 
You can also only access your Urbit from that one machine.

This guide uses Digital Ocean as the cloud provider because DO focuses on this kind of use case (individual devs running something for themselves). 
This means prices are easy to understand and they provide good tools that are easy to use. In theory the same steps apply to other cloud providers like AWS, Google Cloud, and Azure, but they're enterprise focused - so their prices and tooling are more complicated (partly to obscure costs, but they also just do more than we need).

Tlon is working on automation to make deployment to Google Cloud easier in the future, but for now Digital Ocean is a good option, and what I personally use.

This guide assumes you're running macOS or linux on your local machine.

#### Create a Digital Ocean droplet
 - Create an account on [Digital Ocean][Digital Ocean].
 - Create a droplet with the following settings:
 - **Image**: Ubuntu 18.04 x64
 - **Plan**: Standard
 - **Size**: 
   - **Minimum**: $10 (2GB/1CPU) 
   - **Recommended**: $15 (3GB/1CPU) 
   - **Bonus**: $20 (4GB/2CPU)
 - **Add block storage**: Skip
 - **Datacenter Region**: Choose the region closest to you.
 - **VPC Network**: No VPC
 - **Additional Options**: None
 - **Authentication**: SSH keys, add a New SSH Key following the instructions DO gives you.
 - **How many Droplets**: 1
 - **Choose a hostname**: This will be the hostname of the box you ssh into (can be whatever you want, I used my Urbit planet name).
 - **Add tags**: None
 - **Project**: It'll select your default.
 - **Backups**: Optional (it costs a little extra, but I have it enabled for peace of mind).

#### Getting your own domain
Your own domain will make accessing your Urbit a lot easier (it'll also allow you to secure things with a Let's Encrypt cert). Domains are relatively inexpensive and since this guide is about best practices I'm making it a required step. 

There are a lot of domain name registrars you can use, this guide suggests [gandi.net][Gandi] because that's the one I use. From there you can search for and register a domain that you like.

#### Configuring your domain for your Digital Ocean droplet
Once you've registered your domain you'll need to configure it to use Digital Ocean for DNS. The following steps are done on the Gandi website.
 - Click Domain on the left panel
 - Click the domain you're going to use for Urbit
 - Click "Gandi's LiveDNS nameservers" under name servers on the overview page
 - Click Change
 - Remove Gandi servers and add the Digital Ocean ones instead: 
   - `ns1.digitalocean.com`
   - `ns2.digitalocean.com`
   - `ns3.digitalocean.com`
 - Save the change.
 - It can take some time for this change to propagate, but I found it to be pretty quick (a few minutes).
 - Now that you've updated the DNS records you can add the domain to your droplet.
 - Back on the DO site, click Networking from the left panel and then enter the domain you registered to have it set for your project.

#### Creating your non-root user
With our domain in place we're now ready to actually log into the box and start to configure the server itself.
 - Since we don't yet have a user we'll need to log in as root:
   ```
   $ ssh root@your_server_ip
   ```
 - If you set a passphrase on your ssh key, you'll be asked for it. If not, you should automatically be logged in.
 - Create a new user, in our example we'll use *sammy* (to match the DO docs), but you should use your own username: 
   ```
   # adduser sammy
   ```
 - Enter a strong password for your user. The questions `adduser` asks you don't matter, hit enter to skip them.
 - Give your new user sudo access: 
   ```
   # usermod -aG sudo sammy
   ```
 - Next we need to enable external access to our new user by moving the ssh key over from root (and setting proper permissions on it).
 - Be careful to note the **lack of trailing slash** in the command below after `/.ssh`:
   ```
   # rsync --archive --chown=sammy:sammy ~/.ssh /home/sammy
   ```
 - Test this connection with `ssh sammy@your_server_ip` from your local machine in a new terminal window.
 - To test that your domain is working try `ssh sammy@your_domain` from a new terminal on your local machine.
 - You should now be able to use this user going forward with `sudo` when necessary.

#### Setting up a basic firewall
Continuing to follow the DO docs we're going to configure the ufw firewall.
 - The below command shows us the applications available to be easily configured with firewall rules by ufw.
   ```
   $ sudo ufw app list
   ```
 - Next we'll configure ufw to allow connections via ssh when the firewall is enabled.
   ```
   $ sudo ufw allow OpenSSH
   ```
 - Next we'll turn on the firewall.
   ```
   $ sudo ufw enable
   ```
 - To see the current the current firewall status use the following.
   ```
   $ sudo ufw status
   ```

#### Installing Nginx
Nginx is a webserver we're going to use as a reverse proxy. All incoming traffic to our Digital Ocean droplet will pass through Nginx and from there to Urbit. This allows us to lock everything else down and secure just that entry point.
 - First we need to update available packages and install Nginx.
   ```
   $ sudo apt update
   $ sudo apt install nginx
   ```
 - Now that we've installed Nginx we have to update the firewall settings:
   ```
   $ sudo ufw allow 'Nginx Full'
   ```
 - You can verify this change with:
   ```
   $ sudo ufw status
   ```
 - Nginx should automatically be started after installation, you can confirm this with:
   ```
   $ systemctl status nginx
   ```
 - You can also confirm that it's up by going to your droplet's IP address, you should see a "Welcome to nginx!" message.
 - You can start/stop/restart Nginx with commands like the following:
   ```
   $ sudo systemctl restart nginx
   ```
 - You can also reload config without restarting with:
   ```
   $ sudo systemctl reload nginx
   ```

#### Configuring Nginx
Now we're going to configure Nginx so it serves up your Urbit traffic securely.
 - Navigate to the `sites-available` directory:
   ```
   $ cd /etc/nginx/sites-available
   ```
 - Create a new file for your domain:
   ```
   $ sudo vim your_domain
   ```
 - Add the following config (putting your domain in the **your_domain** location):
   ```
   server {
    server_name your_domain;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Forwarded for=$remote_addr;
        proxy_set_header Connection '';
        proxy_http_version 1.1;
        chunked_transfer_encoding off;
        proxy_buffering off;
        proxy_cache off;
    }
   }
   ```
   *Note*: **your_domain** should be of the form `example.com` without www or https.
 - Next we're going to create a symlink to enable this in `sites-enabled` for Nginx:
   ```
   $ sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/your_domain
   ```
 - Now reload the Nginx config:
   ```
   $ sudo nginx -s reload
   ```

#### Configuring Let's Encrypt secure certificate
Now that we've got the Nginx reverse proxy installed we're going to get a Let's Encrypt SSL cert for it and configure it to automatically renew.
 - We're going to do this with certbot, first we have to add the up to date repository:
 ```
 $ sudo add-apt-repository ppa:certbot/certbot
 ```
 - Next we're going to install the package:
 ```
 $ sudo apt install python-certbot-nginx
 ```
 - Next we're going to request a cert from certbot:
 ```
 $ sudo certbot --nginx -d your_domain
 ```
 - You'll have to agree to the TOS and then it'll run a test to verify that you control the domain you're requesting a cert for.
 - Certbot will then ask if you want to redirect all traffic to HTTPS. You should select yes for this (option 2).
 - Certbot will automatically update your Nginx config with the settings it needs. It'll also automatically renew before cert expiration.
 - You can now verify your site is working properly by going to `https://your_domain`, you should see a secure certificate.
 - To confirm that certbot will renew properly you can run the following command:
 ```
 $ sudo certbot renew --dry-run
 ```

#### Installing Urbit
Finally we're ready to install Urbit on your very own server. This part is actually pretty easy, if you haven't installed Urbit locally then the instructions are the exact same as the ones in the Urbit [install doc][Urbit Install]. If you have a local ship already, we're going to install Urbit on the server and then send your local ship up.
 - **WARN**: Since Urbit is p2p you don't want to ever run two copies of your ship simultaneously. This is because other nodes that interact with each of your copies will be confused by which one is the most up to date. If you end up accidentally doing this you'll have to do a 'personal breach' described in the Urbit docs to fix things.
 - The first thing you're going to want to do is shut down your local ship, either with control-d or `|exit` in dojo.
 - Next we're gong to install Urbit on the server:
   ```
   $ ssh your_user@your_domain
   $ curl -O https://bootstrap.urbit.org/urbit-v0.10.5-linux64.tgz
   $ tar xzf urbit-v0.10.5-linux64.tgz
   $ cd urbit-v0.10.5-linux64
   ```
 - Now we're going to tar up your local ship and send it to your server, from your local machine's urbit directory:
   ```
   $ tar -zcvf <ship_dir_name>.tar.gz <ship_dir_name>
   $ scp <ship_dir_name>.tar.gz  your_user@your_domain:/home/your_user/urbit-v0.10.5-linux64
   ```
 - Back on your server let's untar your ship and start it up:
   ```
   $ ssh your_user@your_domain
   $ cd urbit-v0.10.5-linux64
   $ tar -zxvf <ship_dir_name>.tar.gz
   $ ./urbit <ship_dir_name>
   ```
 - Your ship should now be sailing on the digital ocean. Check `https://your_domain`, if everything is working properly you should see a login page.
 - Log in with the code from `+code` in dojo like normal and you should see all of your applications.

#### Leaving your Urbit running in a Screen session
Finally, to leave your Urbit running after you disconnect we can leave it in a Screen session. This is just a way to leave applications running in the background and then reconnect to them later. Alternatively, the same can be done with tmux. 
 - First start with your ship stopped, then run the following:
   ```
   $ screen -S urbit
   ```
 - This will start a screen session, we can now start up the Urbit ship in this session:
   ```
   $ ./urbit <ship_dir_name>
   ```
 - Then we can disconnect from the screen session and leave the ship running with `control-a d`
 - To get back into the screen sesssion:
   ```
   $ screen -r
   ```
- There are more screen commands for interacting with sessions that are easy to find on the internet.

#### Links and Misc.
A lot of the above documentation comes from combining existing resources from Digital Ocean and Urbit into a single guide. The main piece here that I had to figure out myself was the specific Nginx config required to get Urbit to work properly. 

Nginx is also pretty powerful and configurable. You can do things like have your Urbit on an existing server under a subdomain. That kind of thing is left as an exercise for the reader.

On iOS you can save a website to your homescreen as an icon. If you do this for your Urbit domain it's a little like having it as an app.

 - For the docs that made up this guide see the following links.
   - [Digital Ocean Initial Setup][DO Initial Setup]
   - [Digital Ocean DNS][DO DNS]
   - [Digital Ocean Nginx Installation][DO Nginx Install]
   - [Digital Ocean Nginx Config][DO Nginx Config]
   - [Digital Ocean SSL Cert Setup][DO SSL Config]
   - [Urbit Install Docs][Urbit Install]
   - [Urbit Basic Cloud Install][Urbit Basic Cloud Install]

Please email me or submit PRs for breaks if you find them to my [blog][Blog Github].

 [Gandi]: https://www.gandi.net/
 [Digital Ocean]: https://www.digitalocean.com/
 [DO DNS]: https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars
 [DO Nginx Install]: https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04
 [DO Nginx Config]: https://www.digitalocean.com/community/tutorials/how-to-deploy-a-go-web-application-using-nginx-on-ubuntu-18-04
 [DO SSL Config]: https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04
 [Urbit Install]: https://urbit.org/using/install/
 [Urbit Basic Cloud Install]: https://medium.com/@urbitlive/hello-world-urbit-edition-install-boot-and-run-your-urbit-planet-on-a-10-cloud-server-b9579745b9a8
 [DO Initial Setup]: https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04
 [Blog Github]: https://github.com/zalberico/zalberico.github.io
