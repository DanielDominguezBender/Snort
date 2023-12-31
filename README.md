# Snort

![snort1.jpg](imgs/snort1.jpg)

## What is SNORT?  

In simple words, SNORT is like a digital security guard for computer networks. Imagine it as a superhero that watches over your network, looking for bad guys trying to break in or cause trouble.

## What is it used for?

Some things that <b>SNORT</b> does:

Detecting Intruders: Snort's main job is to spot suspicious activities happening in your network. It pays attention to the data passing through your network, just like a guard keeps an eye on people entering a building.

Rules and Patterns: Snort uses a set of rules, like a secret codebook. These rules describe the behavior of known digital bad guys. When Snort sees something matching these rules, it raises a red flag.

Alerts: When Snort finds something fishy, it doesn't just keep it to itself. It sends out an alert, like sounding an alarm. This alert tells your network administrators that something might be wrong.

Protection: Snort can also step in and block suspicious traffic, just like a bouncer at a club who refuses entry to troublemakers. This is called Intrusion Prevention.

Updates: Snort isn't static. It's regularly updated with new rules to recognize the latest threats. It's like giving your security guard new information about the bad guys in town.

Use Cases: Snort is used in all sorts of places, from big companies to small homes. It helps protect networks from cyberattacks, like viruses and hackers, and ensures that digital information stays safe.

## How does SNORT work?  

Snort works by analyzing network traffic in real-time to detect suspicious or malicious activity. Here's a simplified explanation of how it works:

- Packet Capture: Snort starts by capturing network packets. Think of these packets as small pieces of data that travel over the internet, like letters in the mail.

- Preprocessing: Before analyzing packets, Snort might preprocess them. This means cleaning them up and preparing them for inspection.

- Rule Matching: Snort uses a set of rules, also known as signatures, to identify known patterns of malicious activity. These rules describe what specific threats look like. For example, a rule might say, "If a packet contains the word 'virus,' consider it suspicious."

- Packet Analysis: Snort examines each packet and compares its contents to the rules. If it finds a match, it triggers an alert. It's like Snort's way of saying, "Hey, I found something strange here!"

- Logging and Alerting: When Snort detects a suspicious packet, it can log information about it and generate an alert. This alert can be sent to a security administrator or a monitoring system.

- Decision and Action (optional): Depending on how Snort is configured, it can take actions beyond just alerting. For example, it can block the suspicious traffic to prevent it from causing harm. This is known as Intrusion Prevention.

- Continual Monitoring: Snort keeps monitoring network traffic continuously, so it's always on the lookout for potential threats.

A simple analogy on how SNORT works would be: imagine Snort as a security guard at a gate (your network). The guard has a list of known troublemakers (the rules), and they check everyone's belongings (network packets) as they enter. If they find anything suspicious (a match with the rules), they sound an alarm (generate an alert) or even block the troublemaker from entering (Intrusion Prevention).

Snort's strength lies in its ability to quickly and efficiently analyze a massive amount of network data, helping organizations identify and respond to potential security threats in real-time.

## Let's see an example

I installed SNORT on a Virtual Machine using a Ubuntu Distribution.

1) Necessary libraries:

![snort2.png](imgs/snort2.png)

```
sudo apt install -y gcc libpcre3-dev zlib1g-dev libluajit-5.1-dev libpcap-dev openssl libssl-dev libnghttp2-dev libdumbnet-dev
bison flex libdnet autoconf libtool
```
Once done this, I proceed to install <b>SNORT</b> in a temporary directory under the HOME folder.

![snort3.png](imgs/snort3.png)

2) Installing DAQ:
   
Snort uses something called DAQ to make abstract calls to packet capture libraries. To install DAQ I followed the steps explained on [UPCLOUD](https://upcloud.com/resources/tutorials/installing-snort-on-debian).  

![snort4.png](imgs/snort4.png)

- Unzip and enter the downloaded folder:
```
tar -xvzf daq-2.0.7.tar.gz
cd daq-2.0.7
```
- Run the autoreconf command (it gave me an error as I had it not installed). Installed it with:
```
sudo apt-get install autoconf
```
- Configure script and compile the program with: 
```
$ ./configure && make && sudo
make install
```
3) Installing snort:

Installing snort from source:
```
$ wget https://www.snort.org/downloads/snort/snort-2.9.20.tar.gz
```
Extract and enter the new directory:

```
tar -xvzf snort-2.9.16.tar.gz
cd snort-2.9.16
```

Set up the instalation:

```
$ ./configure --enable-sourcefire && make && sudo make install
```
![snort5.png](imgs/snort5.png)

Let's check if all was installed correctly:

```
snort --version
```

![snort6.png](imgs/snort6.png)

It is !! :sunglasses:

Now let's config some rules!! 😄

In this case a simple one which will detect a PING from the host to the virtual machine I have as a lab environment.
These are the steps I followed:

1) Update libraries
2) Create a symbolic link
3) Create a group and user for snort
4) Create a folder structure under which I saved the snort rules

![snort7.png](imgs/snort7.png)

Provide the correct permissions to the folder structure:

![snort8.png](imgs/snort8.png)

Change owner:

![snort9.png](imgs/snort9.png)

For future rules I've created 3 separate rule files.

Whitelisting:<br>
![snort10.png](imgs/snort10.png)

Blacklisting:<br>
![snort11.png](imgs/snort11.png)

Local:<br>
![snort12.png](imgs/snort12.png)

Copy the configuration to the folders previously downloaded:
![snort13.png](imgs/snort13.png)

Now let's download the detection rules. They can be <i>community rules</i> or <i>registered user rules</i>. I chosed the <i>community rules</i>.
![snort14.png](imgs/snort14.png)

Extract the rules:
![snort15.png](imgs/snort15.png)

Once all this is done, now I can proceed to create the rule for the PING detection.

In the config folder I have to protect the network address from the Linux machine.
![snort16.png](imgs/snort16.png)

In here I changed the variable <b>any</b> to the IP of the machine: 172.18.72.15.
In this case I allowed the subnet /16 as well.
![snort17.png](imgs/snort17.png)

Allowing all external traffic:
![snort18.png](imgs/snort18.png)

Update the path to the rules:
![snort19.png](imgs/snort19.png)

Update the paths:
![snort20.png](imgs/snort20.png)

Config the LOGS for Snort (with a limit of 256):
![snort21.png](imgs/snort21.png)

In order to apply the rules, I have to decomment the line:
```
# site specific rules
```
![snort22.png](imgs/snort22.png)

And as last step validate the changes done so far:
```
sudo snort -T -c /etc/snort/snort.conf
```
![snort23.png](imgs/snort23.png)

Now I apply the snort rule for the PING command under the LOCAL RULE file:
![snort24.png](imgs/snort24.png)

The message that should be visible when the snort rule detects a ping is "uoc-ping".

And activate the port listening with the following command:
```
$ sudo snort -A console -q -c /etc/snort/snort.conf -i enp0s3
```
Now let's see if all this work was for nothing or not ... :cold_sweat:
I make a ping attempt form the Windows machine to the Linux (under which I have installed Snort):
![snort24.png](imgs/snort24.png)

It's clearly visible that the snort rule I've implementes is working.


