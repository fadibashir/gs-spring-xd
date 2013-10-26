What you'll build
-----------------

You'll set up [Spring XD (eXtreme Data)](https://github.com/spring-projects/spring-xd/wiki), create a tap to monitor a live twitter feed, and pipe it into a file.

What you'll need
----------------

 - About 15 minutes
 
How to complete this guide
--------------------------

Like all Spring's [Getting Started guides](/guides/gs), you can start from scratch if you don't have Homebrew installed, or you can bypass that and jump right into setting up Spring XD.

To **start from scratch**, move on to [Installing Mac OS X Homebrew](#scratch).

To **skip the basics**, jump ahead to [Installing Spring XD](#initial).

<a name="scratch"></a>
Installing Mac OS X Homebrew
----------------------------
There are several package managers available for Mac OS X, but one of the most popular is Homebrew. And we have strong support for Homebrew! If you haven't already, use these steps to set it up.

    $ ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
    
This installation script will provide extra information.

> **Note:** Do you have OS X Mavericks (10.9)? There is current work underway to make sure Homebrew supports this version of OS X. See https://github.com/mxcl/homebrew/wiki for more community support.

<a name="initial"></a>
Installing Spring XD
--------------------
Homebrew has a base set of "formulas" to install many tools. But it also comes with the ability to support 3rd party sets called "taps". Pivotal has a tap.

    $ brew tap pivotal/tap && brew install springxd
    
It might take a little bit of time to install.

Running Spring XD
-----------------
To start Spring XD:

    $ xd-singlenode
    
> **Note:** *singlenode* is Spring XD's simplest mode and more suited for demonstration purposes. See https://github.com/spring-projects/spring-xd/wiki for more options and details.
    
You should see something like this:

```
22:55:18,330  INFO main server.AdminServer:205 - started embedded tomcat adapter
22:55:18,801  INFO main container.XDContainer:143 - started container: 0
22:55:18,808  INFO main launcher.LocalContainerLauncher:109 - 
 _____                           __   _______
/  ___|          (-)             \ \ / /  _  \
\ `--. _ __  _ __ _ _ __   __ _   \ V /| | | |
 `--. \ '_ \| '__| | '_ \ / _` |  / ^ \| | | |
/\__/ / |_) | |  | | | | | (_| | / / \ \ |/ /
\____/| .__/|_|  |_|_| |_|\__, | \/   \/___/
      | |                  __/ |
      |_|                 |___/
1.0.0.M3                         eXtreme Data

Using local mode JMX is disabled for XD components
```

In another terminal, start Spring XD's management  shell:

    $ xd-shell
    
This will produce a prompt:

```
 _____                           __   _______
/  ___|          (-)             \ \ / /  _  \
\ `--. _ __  _ __ _ _ __   __ _   \ V /| | | |
 `--. \ '_ \| '__| | '_ \ / _` |  / ^ \| | | |
/\__/ / |_) | |  | | | | | (_| | / / \ \ |/ /
\____/| .__/|_|  |_|_| |_|\__, | \/   \/___/
      | |                  __/ |
      |_|                 |___/
eXtreme Data
1.0.0.M3 | Admin Server Target: http://localhost:8080
Welcome to the Spring XD shell. For assistance hit TAB or type "help".
xd:>
```

Inside Spring XD's shell create a twitter stream:

    xd:> stream create --name twittersearchjava --definition "twittersearch --type=json --fixedDelay=1000 --consumerKey=afes2uqo6JAuFljdJFhqA --consumerSecret=0top8crpmd1MXGEbbgzAwVJSAODMcbeAbhwHXLnsg --query='java' | file"
    
> **Note:** In the next release after Spring XD M3, you won't have to write `--json=true` but instead `outputType=application/json`.

In another terminal:

    $ cd /tmp/xd/output
    $ tail -f twittersearchjava.out

Spring XD is capturing live data from twitter about *java* and writing it to `twittersearchjava.out`. You should see something like this:

```
org.springframework.integration.x.twitter.XDTweet@5aa2b17f
org.springframework.integration.x.twitter.XDTweet@42f2e263
org.springframework.integration.x.twitter.XDTweet@2e78b34b
org.springframework.integration.x.twitter.XDTweet@ef543046
...
```

    
Summary
-------

Congratulations! You've just set up Spring XD and tapped a live twitter stream of data, piping it into a file.
