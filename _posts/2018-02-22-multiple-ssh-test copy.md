---
layout: post
title: Testing ssh connectivity to cluster
redirect_from: "/2013/10/31/sshconnectivity/"
permalink: multiple-ssh-test
---

We often encounter infrastructure issues when we start our processing. I had faced this issue often enough that made me do something about it. I thought of testing the connectivity through python since most of our code was on python and it was easier to integrate into the stack.

I researched about many testing libraries and pytest stood out. Paramiko is another library to test ssh connectivity. Combining both of these to analyze whether our infrastructure is ready or not.

Use case :  check ssh connectivity to multiple clusters in one test

~~~python
#!/usr/bin/python
import paramiko
import pytest

#define server ips to which connectivity is to be tested
#this method assumes that the username and the key file are same 
#across all the servers
@pytest.fixture(params=["ip1.ip1.ip1.ip1", "ip2.ip2.ip2.ip2"])
def ssh(request):
    ssh = paramiko.SSHClient()
    ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    ssh.connect(request.param,username="username",key_filename="connect.pem")
    yield ssh
    ssh.close()

# test connectivity by echoing hello and if the program is able to read
# the output from the stdout
def test_hello(ssh):
    stdin, stdout, stderr = ssh.exec_command("echo hello")
    stdin.close()
    stderr.read() == b""
    assert stdout.read() == b"hello\n"
~~~

Run `pytest` in the terminal to test this.

To check plan of pytest run :
~~~sh
pytest --collect-only
~~~

Fixture does the setup of environment before we execute our test cases , in this case since there are multiple parameters , you can find more than one call for that method in the test plan.

To use a fixture in our method, just pass the method name in the function arguments
~~~sh
$ pytest --collect-only
collected 2 items 
<Module Utility_test.py'>
  <Function 'test_hello[10.30.107.85]'>
  <Function 'test_hello[10.20.91.148]'>
~~~

