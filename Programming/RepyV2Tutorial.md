# Repy V2 Tutorial

## Introduction
This guide provides an introduction to using the [Repy V2](RepyV2API.md) sandbox environment.  It describes what restrictions are placed upon the sandboxed code with examples. At the end of reading this document you should be able to write Repy programs, manage the restrictions on programs, and understand whether Repy is appropriate for a specific task or program.

It is assumed that you have a basic understanding of network programming such as socket, ports, IP addresses, and etc.  Also, a basic understanding of HTML is useful but not required.  Lastly, you need a basic understanding of the Python programming language.  If not, you might want to first read through the Python tutorial at http://www.python.org/doc/. You do not need to be a Python expert to use Repy, but as Repy is a subset of Python, being able to write a simple Python program is essential.

## Building Repy V2
You have to build Repy V2 before starting the tutorial. Make sure you have a Git client and Python installed on your machine (Mac and many Linux boxes already ship with that). For more details, please refer to the [Seattle Build Instructions](../Contributing/BuildInstructions.md).

1. First, clone the newest version of Repy V2 from GitHub: ```git clone https://github.com/SeattleTestbed/repy_v2```
2. Create a directory of your choosing, inside of which Repy V2 will live. Unix example: ```mkdir ~/testv2```
3. Set up Repy V2: 
 1. ```cd repy_v2/scripts; python initialize.py``` This downloads all of Repy V2's dependencies into a folder `repy_v2/DEPENDENCIES`.
 2. Still from the `scripts/` directory, run ```python build.py ~/testv2``` to copy over everything Repy V2 needs to run into the `testv2` directory you created previously.

<!---
4. To run any code in Repy V2 (e.g., example.r2py), launch Repy from its directory: ```cd ~/testv2; python repy.py restrictions.test example.r2py```


You can download Repy [here](https://seattleclearinghouse.poly.edu/download/flibble/).   

If you want you can look at the installer options using the ```--usage``` option.   You can also simply install the software using the defaults.

There is the link to file used in each section next to the title and also at the bottom of this tutorial.
Here is [raw-attachment:examples.zip link] to the zip file containing all files used in this tutorial.
All attached files work on Windows systems.
-->


## Executing Repy programs

You can use Repy functions from an interactive Python interpreter. In your `~/testv2` directory open Python, and map the symbols from repyportability into your namespace. For example, to get a randomfloat through repy, do:

```
$ cd ~/testv2
$ python
Python 2.7.10 (default, Oct 23 2015, 18:05:06)
[GCC 4.2.1 Compatible Apple LLVM 7.0.0 (clang-700.0.59.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from repyportability import *
>>> getruntime()
4.664378881454468
```

Note that this will not provide the security and performance isolation from Repy V2. As a result, you can also do things that are not allowed in a Repy V2 sandbox.

To run a sandboxed repy program from the command line, run the following with the correct values substituted:

```
python <path to repy.py> <path to restrictions file> <path to source code>
```

### File execution (example 1.1)
----
[restrictions.test](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/restrictions.test), [example.1.1.r2py](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/example.1.1.r2py)

Let's start with an example `example.1.1.r2py`, which prints the string `Hello World`. The source code looks like this:

```python
log("Hello World\n")
```

Download the two files linked above to `~/testv2` and run the following command from within the directory:

```
python repy.py restrictions.test example.1.1.r2py
```

The output is the following:

```python
Hello World
Terminated
```

"Hello World" was printed, and the last line means that the program was terminated successfully. Not all users will see "Terminated". This only shows up on some operating systems. Other operating systems may have different messages.


<!--- 
  REPYV2 CAN CURRENTLY ONLY BE EXECUTED LOCALLY
#### Executing programs remotely in VMs
----
You can also access the resources donated to you and run the program example.1.1.r2py in the resources you can control. You need to follow the steps below in the shell in order to  run the program in resources.

**Step 1. Run programs in Seattle**

To run a program, you should use the experiment manager seash. Seash allows you to control the files running in sandboxes you control.

```
python seash.py
```

**Step 2. Log in with your key**

Before logging in, you have to load your public and private keys. You can get the keys at the  [SeattleClearinghouse](https://seattleclearinghouse.poly.edu/) website. You need to make sure that you have two files, your public and private key. The name of each file is your ID and the extension of files are publickey and privatekey.
For example, if your ID were john, the two files would be ```john.publickey``` and ```john.privatekey```.   These files must be placed in the directory where you run ```seash```.

If you successfully logged in, you could notice that the prompt starts with you ID.

```python
!> loadkeys john
!> as john
john@ !>
```

**Step 3. Locate the resources (laptops, smartphones, etc.) you can use**

This command will tell the shell to locate resources that have been assigned to your account through the SeattleClearinghouse website. In order to assign resources to your account, you must go to the SeattleClearinghouse website, log into your account and get more resources under the 'My VMs' tab.

```python
john@ !> browse

Added targets: %1(10.0.1.1:1224:v9), %2(10.0.1.2:2888:v9)

Added group 'browsegood' with 2 targets

The command browse produced the output that shows resources(targets) you can control. 
```

In the first target %1(10.0.1.1:1224:v9), %1 is the alias of the target, 10.0.1.1 is IP address, 1224 is port number, and v9 is VM name. The VM is the partial portion of whole donated resources. It means that the donated resources could be one VM or divided and assigned to more than one person. 

**Step 4. Access the resources(VMs)**

```python
john@ !> on browsegood
```

**Step 5. Run a program**

In this case, the program named example.1.1.r2py is stored in the current directory. Note that the results will not be printed.

```python
john@browsegood !> upload example.1.1.r2py
john@browsegood !> start example.1.1.r2py
```
**Step 6. Look up the result of the program executed**

In order to see the output, run 'show log', as is done below:

```python
john@browsegood !> show log

Log from '10.0.1.1:1224:v9':
Hello World

Log from '10.0.1.2:2888:v9':
Hello World
```

**Step 7. Look up the information about the VMs you control**

```python
john@browsegood !> list

ID Own       Name          Status          Owner Information
%1  *   10.0.1.1:1224:v9 Terminated                               
%2  *   10.0.1.2:2888:v9 Terminated        
```

**Step 8. Exit from Seash**

```python
john@browsegood !> exit
```


You can find more practical examples in the [EducationalAssignments/TakeHome Take home assignment] page.                
-->


### Another Hello World example (example 1.2)
----
[example.1.2.r2py](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/example.1.2.r2py)

In this example, we'll wait for the user to browse a port we listen on. When the user browses our page, we'll display a hello world webpage and then exit. 

```python
def hello(ip, port, sockobj):
  # Receive the browser's HTTP request header, but we'll ignore it
  httpheader = sockobj.recv(512)
  response_body = "<html><head><title>Hello World</title></head>\
                <body><h1> Hello World! </h1></body></html>"
  # Send a simple HTTP header before the actual HTML
  sockobj.send("HTTP/1.1 200 OK\r\nContent-Length: " +
    str(len(response_body)) + "\r\n\r\n" + response_body)
  # close my connection with this user
  sockobj.close()


if len(callargs) > 1:
  raise Exception("Too many call arguments")

# Running remotely: whenever this vessel gets a connection
# on its IPaddress:Clearinghouseport it'll call hello
elif len(callargs) == 1:
  port = int(callargs[0])
  ip = getmyip()

# Running locally: whenever we get a connection
# on 127.0.0.1:12345 we'll call hello
else:
  port = 12345
  ip = '127.0.0.1'

server_socket = listenforconnection(ip, port)

while True:
  try:
    ret_ip, ret_port, ret_socket = server_socket.getconnection()
    hello(ret_ip, ret_port, ret_socket)

  except SocketWouldBlockError:
    sleep(0.1)
```

The initialize portion has two options.  If you run it locally, it says, wait for a connection on 127.0.0.1 (the "local" IP of the computer), port 12345, and when you get, it calls the function hello().

```
127.0.0.1 is a special IP address. 
It is localhost and means this computer. It is used to access the computer currently used.
```

<!---
Otherwise, you're running it remotely and need to pass it your Clearinghouse port.  For convenience, Seattle Clearinghouse assigns the same UDP port to all of a user's VMs. To figure out which port you should use, log into the SeattleClearinghouse website and on the Profile page, look to see what port number it indicates. You'll see a page like below.


![Seattle Clearinghouse](https://raw.githubusercontent.com/SeattleTestbed/docs/master/ATTACHMENTS/RepyV2Tutorial/clearinghouseport.png)

You should write your Clearinghouseport down as you'll need this value repeatedly.
-->
The `listenforconnection()` function binds to an IP and port and waits for incoming TCP connections, and returns a `TCPServerSocket` object. 

```
TCP Server Socket = listenforconnection(IP address, Port number)

IP address: IP address to listen on 
Port number: The port to bind to
TCP Server Socket: A way to obtain incoming connections

```

To read more about `listenforconnection()`, visit the [API](https://github.com/SeattleTestbed/docs/blob/master/Programming/RepyV2API.md)

The `getconnection()` function receives a connection that was initiated to an IP and port, and returns a tuple containing: (remote ip, remote port, socket object). You can pass the returned socket object to other functions to do operations. For example, when you want to send some data to the remote host you can call `send()`, and you can close this connection by calling `close()`. 

```
Remote IP, Remote Port, Socket Object = server_socket.getconnection()

Socket Object: End-point of a bidirectional communication flow across the Internet.

```

To read more about `getconnection()`, also visit the [API](https://github.com/SeattleTestbed/docs/blob/master/Programming/RepyV2API.md)

The function `hello()` sends the webpage (the text in quotes) using the sockobj and then closes the current connection.

``` 
hello(IP address, Port number, Socket Object)

The IP address and port the users are connecting to.

```

**Running locally**

To run this program locally, you can type like below.

```
python repy.py restrictions.test example.1.2.r2py
```

Then open a web browser and in the address bar type: http://127.0.0.1:12345  (this says, navigate to the "local" IP of the computer at port 12345). You should see the hello world webpage.

To stop the program, press CTRL-C.

**Running remotely**

<!---
Open seash the same as in the previous example but choose a single VM.

```python
default@ !> on %1  
default@%1 !>
```

Then, run this program passing it your Clearinghouse port.

```python
default@%1 !> upload example.1.2.r2py
default@%1 !> start example.1.2.r2py Clearinghouseport
default@%1 !> list                            
  ID   Own                  Name                Status      Owner Information
  %1             IP address:port:VMname     Started     
```

Notice that the program is "Started" instead of "Terminated". It's still running! 

Now open a web browser and in the address bar type: IP_of_%1:Clearinghouseport.  For example, if 1.2.3.4 is the IP address of %1, and your port is 54321 then open your browser and in the address bar type "http://1.2.3.4:54321". If the node doesn't respond, try again.    You should see the hello world webpage.

To stop the program, in the shell type:

```python
default@%1 !> stop  # This will stop any programs that are still running...  
default@%1 !> list                          
  ID    Own                Name                Status          Owner Information
  %1             IP address:port:VMname    Stopped   
```

Notice that the program is now "Stopped".   It's not running

-->

### Counting the number of calls (example 1.3)
----
[example.1.3.r2py](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/example.1.3.r2py)


In this example, we'll add a counter to our helloworld program. First we'll try some perfectly valid python code that will **NOT WORK** in Repy.

```python
def hello(ip, port, sockobj):  
  global pagecount   # GLOBALS ARE NOT ALLOWED IN REPY
  pagecount = pagecount + 1
  httpheader = sockobj.recv(512) # Receive HTTP header, but we'll ignore it
  response = ("<html><head><title>Hello World</title></head>"
              "<body><h1> Hello World!</h1>"
              "<p>You are visitor "+str(pagecount)+"</body></html>")
  response = ('HTTP/1.1 200 OK\nContent-Type: text/html\nContent-Length: '+
              str(len(response))+'\nServer: Seattle Testbed\n\n'+response)
  sockobj.send(response)
  # close my connection with this user
  sockobj.close()

mycontext['pagecount'] = 0

if len(callargs) > 1:
  raise Exception("Too many call arguments")

# Running remotely: whenever this VM gets a connection 
# on its IPaddress:Clearinghouseport it'll call hello
elif len(callargs) == 1:
  port = int(callargs[0])
  ip = getmyip()

# Running locally: whenever we get a connection
# on 127.0.0.1:12345 we'll call hello
else:
  port = 12345
  ip = '127.0.0.1'

server_socket = listenforconnection(ip, port)

while True:
  try:
    ret_ip, ret_port, ret_socket = server_socket.getconnection()
    hello(ret_ip, ret_port, ret_socket)    
  except SocketWouldBlockError:
    sleep(0.1)
```

This code will not work because globals do not exist in Repy. However, there is a mycontext dictionary provided for this purpose. The code can be written as follows:

```python
def hello(ip, port, sockobj):
  httpheader = sockobj.recv(512) # Receive HTTP header, but we'll ignore it
  mycontext['pagecount'] = mycontext['pagecount'] + 1

  response = ("<html><head><title>Hello World</title></head>"
              "<body><h1> Hello World!</h1>"
              "<p>You are visitor "+str(mycontext['pagecount'])+"</body></html>")
  response = ('HTTP/1.1 200 OK\nContent-Type: text/html\nContent-Length: '+
              str(len(response))+'\nServer: Seattle Testbed\n\n'+response)
  sockobj.send(response)
  # close my connection with this user
  sockobj.close()

mycontext['pagecount'] = 0

if len(callargs) > 1:
  raise Exception("Too many call arguments")

# Running remotely: whenever this VM gets a connection
# on its IPaddress:Clearinghouseport it'll call hello
elif len(callargs) == 1:
  port = int(callargs[0])
  ip = getmyip()

# Running locally: whenever we get a connection
# on 127.0.0.1:12345 we'll call hello
else:
  port = 12345
  ip = '127.0.0.1'

server_socket = listenforconnection(ip, port)

while True:
  try:
    ret_ip, ret_port, ret_socket = server_socket.getconnection()
    hello(ret_ip, ret_port, ret_socket)
  except SocketWouldBlockError:
    sleep(0.1)
```

Start the program using the shell, then open a web browser and in the address bar type: http://127.0.0.1:12345/.  You should see the hello world webpage and a count of 1. You can refresh the page and the count should increment. Once again, use the shell to stop the program. *Note: Some web browsers might make two requests each time you reload, so don't worry if your counter increments faster than you would expect.*

### Adding a Timer and listing the elapsed time (example 1.4)
----
[example.1.4.r2py](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/example.1.4.r2py)

The previous program would run forever, waiting for a user to connect so it could display a webpage.   What if you wanted the program to stop running after 1 minute?   To do this, we'll register a timer that will close the connection.

```python
def hello(ip, port, sockobj):
  try:
    httpheader = sockobj.recv(512) # Receive HTTP header, but we'll ignore it
  except SocketWouldBlockError:
    sockobj.close()
    return
  mycontext['pagecount'] = mycontext['pagecount'] + 1
  response = ("<html><head><title>Hello World</title></head>"
              "<body><h1> Hello World!</h1>"
              "<p>You are visitor "+str(mycontext['pagecount'])+"</body></html>")

  response = ('HTTP/1.1 200 OK\nContent-Type: text/html\nContent-Length: '+
              str(len(response))+'\nServer: Seattle Testbed\n\n'+response)
  sockobj.send(response)
  # close my connection with this user
  sockobj.close()


def close_after(t):
  def sleep_for():
    # after sleeping t sec, close the server
    sleep(t)
    exitall()
  return sleep_for


mycontext['pagecount'] = 0

if len(callargs) > 1:
  raise Exception("Too many call arguments")

# Running remotely: whenever this vessel gets a connection
# on its IPaddress:Clearinghouseport it'll call hello
elif len(callargs) == 1:
  port = int(callargs[0])
  ip = getmyip()

# Running locally: whenever we get a connection
# on 127.0.0.1:12345 we'll call hello
else:
  port = 12345
  ip = '127.0.0.1'

server_socket = listenforconnection(ip, port)
close_server = close_after(60)
createthread(close_server)

while True:
  try:
    ret_ip, ret_port, ret_socket = server_socket.getconnection()
    hello(ret_ip, ret_port, ret_socket)
  except SocketWouldBlockError:
    sleep(0.1)
  except SocketClosedLocal:
    log("The server is going to close now!\n")
    break
```
 

The timer (when called) will stop the communication. `settimer` returns an `eventhandle`. An `eventhandle` is similar to a `commhandle` in that it can be used to interact with an event. `listencommhandle` is not the arguments of settimer but that of `stop_listening`. The comma next to `stop_listening` and parentheses surrounding `listencommhandle` indicate that.

`getruntime()` is used to provide the amount of time that has elapsed since the program was started.   Note that it may be possible for the elapsed time to be > 60 seconds in this program because there may be some lag between when the program started and when your code ran or a delay in running the `stop_listening` event.

Start the program and then open a web browser and in the address bar type: http://127.0.0.1:12345/.  You'll notice that the webpage displays and after 1 minute the program will stop listening and stop itself.   <!--You'll see that the program's status is "Terminated" because it caused itself to exit (as opposed to "Stopped" where you stopped it while it was running). -->


<!--
### Interacting with Timers (example 1.5)
[example.1.5.repy](https://github.com/SeattleTestbed/tutorial/blob/master/example/example.1.5.repy)
----

We know how to set a timer to close the connection in 60 seconds, but that doesn't help us to keep the connection open. To have the connection kept open while requests continue to happen, every time we get a request we'll cancel the existing timer and set a new timer for 10 seconds in the future that will close the connection. The timer(when called) will stop the communication. canceltimer stops a timer (if it hasn't already started).

[[Image(SetTimer.jpg)]]

```python
def hello(ip,port,sockobj, thiscommhandle,listencommhandle):
  httpheader = sockobj.recv(512) # Receive HTTP header, but we'll ignore it
  mycontext['pagecount'] = mycontext['pagecount'] + 1
  sockobj.send("<html><head><title>Hello World</title></head>

                <body><h1> Hello World!</h1>

                <p>You are visitor "+str(mycontext['pagecount'])+"</body></html>")
  stopcomm(thiscommhandle)   # close my connection with this user

  # stop the previous timer...
  canceltimer(mycontext['stopevent'])

  # start a new one for 10 seconds from now...
  eventhandle = settimer(10, stop_listening, (listencommhandle,))
  mycontext['stopevent'] = eventhandle
 
def stop_listening(commhandle):
  stopcomm(commhandle)   # this will deregister hello

if callfunc == 'initialize':
  mycontext['pagecount'] = 0

  if len(callargs) > 1:
    raise Exception("Too many call arguments")

  # Running remotely:
  # whenever this VM gets a connection on its IPaddress:Clearinghouseport it'll call hello
  elif len(callargs) == 1:
    port = int(callargs[0])
    ip = getmyip()

  # Running locally:
  # whenever we get a connection on 127.0.0.1:12345 we'll call hello
  else:
    port = 12345
    ip = '127.0.0.1'
  listencommhandle = waitforconn(ip,port,hello)

  eventhandle = settimer(10, stop_listening, (listencommhandle,))
  mycontext['stopevent'] = eventhandle


Try running the program and then try browsing the webpage a few times and then wait. You'll notice the connection closes 10 seconds after you stop browsing. 

### Races, Sleep, and Locks (example 1.6)
----
[example.1.6.repy](https://github.com/SeattleTestbed/tutorial/blob/master/example/example.1.6.repy)


The wily reader may have noticed that the previous program has a race condition. It is possible to have multiple browser windows browse 127.0.0.1 at the same time. It is possible for browser window A to cancel the timer, but before it adds the new eventhandle to mycontext!['stopevent'], window B could also try to cancel the timer and set up its own event. Now there are two events that will stop communications and only one of these will be listed in mycontext!['stopevent']. This means that regardless of how often a user views the webpages, the other event will fire because there is no reference to the eventhandle. The connection will be closed (not what we want!). It would be difficult to trigger this condition manually, but we'll use a command "sleep" to pause the current function for some amount of time to allow us to trigger the bug so that we can see it in practice.


```python
def hello(ip,port,sockobj, thiscommhandle,listencommhandle):
  httpheader = sockobj.recv(512) # Receive HTTP header, but we'll ignore it
  mycontext['pagecount'] = mycontext['pagecount'] + 1
  sockobj.send("<html><head><title>Hello World</title></head>

                <body><h1> Hello World!</h1>

                <p>You are visitor "+str(mycontext['pagecount'])+"</body></html>")
  stopcomm(thiscommhandle)   # close my connection with this user
  canceltimer(mycontext['stopevent'])
  sleep(5)   # give me 5 seconds to trigger the race
  
  # wait 10 seconds, then call stop_listening with listencommhandle
  eventhandle = settimer(10, stop_listening, (listencommhandle,)) 
  
  mycontext['stopevent'] = eventhandle
 
def stop_listening(commhandle):
  stopcomm(commhandle)   # this will deregister hello

if callfunc == 'initialize':
  mycontext['pagecount'] = 0

  if len(callargs) > 1:
    raise Exception("Too many call arguments")

  # Running remotely:
  # whenever this VM gets a connection on its IPaddress:Clearinghouseport it'll call hello
  elif len(callargs) == 1:
    port = int(callargs[0])
    ip = getmyip()

  # Running locally:
  # whenever we get a connection on 127.0.0.1:12345 we'll call hello
  else:
    port = 12345
    ip = '127.0.0.1'
  listencommhandle = waitforconn(ip,port,hello)

  # wait 10 seconds, then call stop_listening with listencommhandle
  eventhandle = settimer(10, stop_listening, (listencommhandle,)) 
  
  mycontext['stopevent'] = eventhandle
```
 
Try browsing the webpage a few times rapidly and then browse every 3-5 seconds.   You'll notice the program closes automatically even though it shouldn't (because you kept browsing).  

One way to fix a race condition is using a lock. You can get a lock by calling getlock(). A lock supports two operations: acquire and release and is similar to the Python threading Lock class.

We can avoid race conditions by the following changes:

```python
def hello(ip,port,sockobj, thiscommhandle,listencommhandle):
  httpheader = sockobj.recv(512) # Receive HTTP header, but we'll ignore it
  mycontext['pagecount'] = mycontext['pagecount'] + 1
  sockobj.send("<html><head><title>Hello World</title></head>

                <body><h1> Hello World!</h1>

                <p>You are visitor "+str(mycontext['pagecount'])+"</body></html>")
  stopcomm(thiscommhandle)   # close my connection with this user

  mycontext['stoplock'].acquire() # acquire the lock

  canceltimer(mycontext['stopevent'])
  
  # wait 10 seconds, then call stop_listening with listencommhandle
  eventhandle = settimer(10, stop_listening, (listencommhandle,)) 
  
  mycontext['stopevent'] = eventhandle

  mycontext['stoplock'].release() # release the lock
 
def stop_listening(commhandle):
  stopcomm(commhandle)   # this will deregister hello

if callfunc == 'initialize':
  mycontext['pagecount'] = 0
  mycontext['stoplock'] = getlock()
  
  if len(callargs) > 1:
    raise Exception("Too many call arguments")

  # Running remotely:
  # whenever this VM gets a connection on its IPaddress:Clearinghouseport it'll call hello
  elif len(callargs) == 1:
    port = int(callargs[0])
    ip = getmyip()

  # Running locally:
  # whenever we get a connection on 127.0.0.1:12345 we'll call hello
  else:
    port = 12345
    ip = '127.0.0.1'
  listencommhandle = waitforconn(ip,port,hello)
  
  # wait 60 seconds, then call stop_listening with listencommhandle
  eventhandle = settimer(10, stop_listening, (listencommhandle,)) 
  
  mycontext['stopevent'] = eventhandle
```

Now if you run the program repeatedly, it shouldn't be possible to trigger a race condition.   Note that since only one event can be in the section of the code protected with locks, if you put a sleep in there and rapidly browse, you may have your program slow down because there are not enough free threads / events.  

Race conditions can happen with threads created by several different functions including waitforconn, recvmess, and settimer.   Remember that any callbacks or timers may be executing at the same time as your other code.   If you need to prevent multiple threads from modifying a variable or executing a piece of code at the same time, use a lock!

-->

## File Operations

### Introduction to Files (example 2.1)
----
[example.2.1.r2py](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/example.2.1.r2py), [hello.file](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/hello.file)

We'll now switch gears from our hello world web server and focus instead on writing data to files.   The first program we'll write the string "hello world" to a file and then read it back out and print it.

```python
# In repyV2 we use openfile(filename, create)
# Boolean value decide whether creating file if such file doesn't exist
myfileobject = openfile("hello.file", True)

# Use fileobject.writeat(data, offset) to write data into file
myfileobject.writeat("hello world\n", 0)

myfileobject.close()

newfileobject = openfile("hello.file", False)

# Use fileobject.readat(sizelimit, offset) to read data from file
myfilecontent = newfileobject.readat(None, 0)

log(myfilecontent)
newfileobject.close()
```

This program should print hello world (and an extra newline from '\n'). 


### Listing and Removing Files (example 2.2)
----
[example.2.2.r2py](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/example.2.2.r2py)

In this example, we'll change our program to write information to a few different files and after we're finished, we'll remove the files.   Instead of coding in the program which files to write information to, we'll let our command line arguments specify which files to write.

```python
for filename in callargs:   # callargs has all of the command line arguments in it.

  myfileobject = openfile(filename, True)
  myfileobject.writeat("hello world\n", 0)
  myfileobject.close()

  # Check the list of file names for files in the vessel by listfiles()
  # This may include things other than our files.
  myfilelist = listfiles()
  log(myfilelist)

for filename in callargs:   # let's remove our files now...
  removefile(filename)

log("\nThe files: " + str(callargs) + " should now be missing from " + str(listfiles()))
```

You should see output that first lists the file names in the current directory (along with the files you chose to write) and then shows the listing missing the files.   Note that if you choose file names with characters like "/", "\", "@", or other non-alphanumeric characters you will get an error because Repy programs are not allowed to use certain characters in file names.



## Network Operations 1

### Interacting with other computers (example 3.1)
----
[example.3.1.r2py](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/example.3.1.r2py)


In this example, we'll retrieve a webpage and print it on the screen.   We're not going to translate all of the HTML or remove the HTTP headers, so it will be a bit messy.  

```python
# open a connection to the google web server
destip = gethostbyname("www.google.com")
socketobject = openconnection(destip, 80, getmyip(), 12345, 5)

# this is a HTTP request...
httprequest = "GET /index.html HTTP/1.1\r\nHost: www.google.com\r\n\r\n"
socketobject.send(httprequest)

while True:
  try:
    log(socketobject.recv(4096), "\n")
  except SocketWouldBlockError:
    sleep(0.1)
  except SocketClosedRemote:
    break
```
   
You should see google's webpage along with some numbers and other information (this is the HTTP protocol).   However, the program does not stop (at least until you press CTRL-C)!   This is because HTTP 1.1 allows us to issue multiple page requests on a single connection.  

### Causing a program to exit (example 3.2)
----
[example.3.2.r2py](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/example.3.2.r2py)

In this example, we'll change the previous program to exit after we receive the data from google.   To do this we'll use a function called exitall().   This function causes the program to abort and all threads to exit immediately.   We can add this to a thread that sleeps for t seconds and then terminates, to the previous example as follows:

```python
def close_after(t):
  def sleep_for():
    # after sleeping t sec, terminate the program
    sleep(t)
    exitall()
  return sleep_for

# open a connection to the google web server
destip = gethostbyname("www.google.com")
socketobject = openconnection(destip, 80, getmyip(), 12345, 5)

# this is a HTTP request...
httprequest = "GET /index.html HTTP/1.1\r\nHost: www.google.com\r\n\r\n"
socketobject.send(httprequest)

terminate_program = close_after(10)
createthread(terminate_program)

while True:
  try:
    log(socketobject.recv(4096), "\n")
  except SocketWouldBlockError:
    sleep(0.1)
  except SocketClosedRemote:
    break
```
 
Alternatively, we could instead close the socket and check this condition.   Example code that shows this is below

```python
def close_after(t):
  def sleep_for():
    # after sleeping t sec, terminate the program
    sleep(t)
    socketobject.close() 
  return sleep_for

# open a connection to the google web server
destip = gethostbyname("www.google.com")
socketobject = openconnection(destip, 80, getmyip(), 12345, 5)
  
# this is a HTTP request...
httprequest = "GET /index.html HTTP/1.1\r\nHost: www.google.com\r\n\r\n"
socketobject.send(httprequest)

terminate_program = close_after(10)
createthread(terminate_program)
  
while True:
  try:
    log(socketobject.recv(4096), "\n")
  except SocketWouldBlockError as e:
    continue
  except SocketClosedRemote as e:
    break
  except SocketClosedLocal as e:
    break
```

## Network Operations 2
### Looking up IP addresses (example 4.1)
----
[example.4.1.r2py](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/example.4.1.r2py)


In this example, we'll look up some IP addresses using gethostname_ex:

```python
log(gethostbyname("www.google.com"), "\n")
log(gethostbyname("www.wikipedia.com"), "\n")
log(getmyip(), "\n")
```

This will print IP address, hostname and other information for google and wikipedia and will also print the current computer's IP address.   For more information about the format of the data returned from gethostbyname see the [API](https://github.com/SeattleTestbed/docs/blob/master/Programming/RepyV2API.md).



### Sending and receiving messages (example 4.2)
----
[example.4.2.r2py](https://raw.githubusercontent.com/SeattleTestbed/tutorial/master/example/example.4.2.r2py)


In this example, we'll manually send a NTP (time) lookup message and print the raw data that is returned.   Rather than hard code a server into our program, we'll randomly choose a server from a list of publicly available time servers.   A lot of the complexity in this program comes with encoding and decoding NTP data.  


```python
dy_import_module_symbols("random.r2py")

# See RFC 2030 (http://www.ietf.org/rfc/rfc2030.txt) for details about NTP
# this unpacks the data from the packet and changes it to a float
def convert_timestamp_to_float(timestamp):
  integerpart = (ord(timestamp[0])<<24) + (ord(timestamp[1])<<16) + (ord(timestamp[2])<<8) + (ord(timestamp[3]))
  floatpart = (ord(timestamp[4])<<24) + (ord(timestamp[5])<<16) + (ord(timestamp[6])<<8) + (ord(timestamp[7]))
  return integerpart + floatpart / float(2**32)


def decode_NTP_packet(ip, port, mess, sockobj):
  log("From " + str(ip) + ":" + str(port) + ", I received NTP data.\n")
  log("NTP Reference Identifier:", mess[12:16], '\n')
  log("NTP Transmit Time (in seconds since Jan 1st, 1900):", convert_timestamp_to_float(mess[40:48]), '\n')
  sockobj.close()

timeservers = ["time-a.nist.gov", "time-b.nist.gov", "time-a.timefreq.bldrdoc.gov",
               "time-b.timefreq.bldrdoc.gov", "time-c.timefreq.bldrdoc.gov",
               "utcnist.colorado.edu", "time.nist.gov", "nist1.symmetricom.com",
               "nist.netservicesgroup.com"]

# choose a random time server from the list
servername = timeservers[int(randomfloat()*len(timeservers))]
log("Using: ", servername, '\n')
serverip = gethostbyname(servername)

# this sends a request, version 3 in "client mode"
ntp_request_string = chr(27)+chr(0)*47

ip = getmyip()
port = 34612

# binds to an IP and port and waits for incoming UDP messages
udpserversocket = listenformessage(ip, port)

sendmessage(serverip, 123, ntp_request_string, ip, port) # port 123 is used for NTP

while True:
  try:
    remoteip, remoteport, message = udpserversocket.getmessage()
    decode_NTP_packet(remoteip, remoteport, message, udpserversocket)
  except SocketWouldBlockError:
    sleep(0.1)
  except SocketClosedLocal:
    break
```

To run this program locally, you need to do 

```python repy.py restrictions.test dylink.r2py example.4.2.r2py``` 

on command line. The port allowed for NTP communication can be found in the restriction file where 

```resource messport xxxxx   # use for getting an NTP update```

This program will print the number of seconds since Jan 1st, 1900 from a time server. If you run the program multiple times, you'll see it chooses servers randomly from the timeservers list.  

Getting the time from a synchronized time source is a handy thing to do (so this isn't bad code to repurpose for your code).   You shouldn't ask for the time repeatedly from the time servers though (they consider requests more frequent than 4 seconds apart to be a DoS attack).   You can ask once for a global reference time and then use getruntime to measure the elapsed time.

## Resources

In the previous section, we looked at restrictions, a mechanism for allowing or denying an action. These aren't adequate for many cases. For example, you can say a program can or cannot write files, but you cannot say that a program can only write 1MB worth of files.   This is where resources come in.   They put limits on the number of resources a program can consume.

If you look at a restrictions / resources file, you'll see a bunch of lines like this:

```python
resource cpu .10
resource memory 10000000   # 10 Million bytes
resource diskused 10000000 # 10 MB
resource events 10
resource filewrite 10000
resource fileread 10000
resource filesopened 5
resource insockets 5
resource outsockets 5
resource netsend 10000
resource netrecv 10000
resource loopsend 1000000
resource looprecv 1000000
resource lograte 30000
resource random 100
resource messport 12345
resource messport 12346
resource connport 12345
```

These lines specify the type and quantity of resources the program can consume.   Some resources like CPU, the netsend / recv rate, random number generation rate, etc. are for renewable resources.   Renewable resources are resources that replenish themselves over time.   For example, if your program is using too much CPU, it will be paused temporarily to allow other programs to run.    Trying to use too much of a renewable resource will cause a call to run slowly.

Alternatively, other resources like memory, disk used, sockets, etc. are not renewable.   In other words, the resource has a hard limit and is does does not automatically replenish itself over time.   When you use too much of one of these resources, the call in your program which causes it to be over the limit may raise an exception or in some cases the program may be killed outright.  

The restrictions lines (with call information) is obsolete for Repy V2 and is ignored.   The RepyV2SecurityLayers functionality is a replacement that improves upon the previous technology.

## Conclusion

This concludes a brief tour of the Repy V2 functionality.   This guide has covered most of the API calls for Repy V2.   For more detailed information see the [Repy V2 API Reference](RepyV2API.md).   You should now be able to build your own Repy applications that can run on hundreds of computers.   Good luck!
