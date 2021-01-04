# Hashtopolis, Hashcat v6.x.x, Vast.ai

This container is useful for quickly! deploying lots of agents from vast.ai to your hashtopolis server with single clicks for an on-demand hash cracking session.

You should be familiar with the prerequisites for hashcat and hashtopolis, this isn't a step-by-step guide as such, It's half the battle is if you already have access to a hashtopolis server.

### Prerequisites

In order to fully utilise this container you'll require the following...

* [hashtopolis](https://github.com/s3inlc/hashtopolis/)
server which is remotely accessible
* [vast.ai account](https://vast.ai/)

### Usage and Information

Note: That this is a CUDA Installation, NOT OpenCL

This is a one click set-up with vast.ai. Just click "Rent" button and your client will auto register with your hashtopolis server and will instantly start working on any available tasks you have waiting.

This container takes the pain out of manually checking boxes and adding in additional parameters for each agent if you want to register a lot of agents at one time.

### Hashtopolis Modifications

You'll notice that each time you register an agent to your HTP server that you need to manually update the `Agent Trust Status` and you'll also receive a hashcat error `clGetPlatformIDs(): CL_PLATFORM_NOT_FOUND_KHR`, but we can fix this easily in hashtopolis the `whitelist feature` for errors.

Normally hashcat itself will ignore this error and continue but in hashtopolis's case it will take the error as fatal and halt the agent as by default the agent is set to be deactivated on any errors returned by hashcat.

`whitelist` this error in hashtopolis server settings. Enter `clGetPlatformIDs(): CL_PLATFORM_NOT_FOUND_KHR` in Server Settings page `config.php` where it says `Ignore error messages from crackers which contain given strings (multiple values are separated by comma)`

### Workaround for Trust Agent

Add a cron script on your hashtopolis server to periodically update the your hashtopolis MySQL database every 1 minute or so to set new agents as trusted. 
modify this line to suit your own environment if need be.
```
cd ~/
echo mysql -D your_hashtopolis_db -e \"UPDATE Agent SET isTrusted = '1'\" > set_trust.sh && chmod +x set_trust.sh
```
type `crontab -e` and add...

`* * * * * /root/set_trust.sh >/dev/null 2>&1`

![Alt text](https://i.ibb.co/n7VSmP8/cron1.png)

#### config.json

You need to have reusable voucher codes checked in your hashtopolis install.
Go to `https://{your_domain}/config.php?view=5` and check box to allow vouchers to be use multiple times.

#### Vast.ai

Edit `Image & Config` and use `milz0/vast-hashcat-hashtopolis` as your custom image

Your onstart-script should be written out as so in vast.ai, not before replacing both values {server} and {voucher_id} with your own.
```
cd htpclient
python3 hashtopolis.zip --url {server} --voucher {voucher_id}
```

![Alt text](https://i.ibb.co/hYZ6Mqh/vast.png)

#### Miscellaneous

For additional parameters like -w4 (workload) and -O (optimized kernels), You can set these globally per task in the command box #HL#

#### Demo

[![Demo](https://img.youtube.com/vi/A1QrUVy7UZ0/0.jpg)](https://www.youtube.com/watch?v=A1QrUVy7UZ0 "Demo")
