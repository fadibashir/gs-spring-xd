What you'll build
-----------------

You'll set up [Spring XD (eXtreme Data)](https://github.com/spring-projects/spring-xd/wiki), create a stream to monitor a live twitter feed, and pipe it into a file.

What you'll need
----------------

 - About 15 minutes
 - JDK 1.7 or later	
 
How to complete this guide
--------------------------

If you are on a Mac you can get some simple instructions to [install Homebrew](#scratch).

If you don't have a Mac or you aren't interested in installing Homebrew, then you can skip that and jump right to [Installing Spring XD](#initial).

<a name="scratch"></a>
Installing Homebrew
-------------------
There are several package managers available for Mac OS X, but one of the most popular is Homebrew. And we have strong support for Homebrew! If you haven't already, use these steps to set it up.

    $ ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
    
This installation script will provide extra information.

Homebrew has a base set of "formulas" to install many tools. But it also comes with the ability to support 3rd party sets called "taps". Pivotal has a [tap](http://github.com/pivotal/homebrew-tap) that includes several tools including Spring XD, which you'll see in the next section.

<a name="initial"></a>
Installing Spring XD
--------------------

If you are using a Mac with Homebrew, the process is pretty simple.

    $ brew tap pivotal/tap && brew install springxd
    
That's it! It might take a little bit of time to install.

If you aren't using Homebrew or you're on a different platform:

1. Download [Spring XD 1.0.0.M4](http://repo.spring.io/simple/libs-milestone-local/org/springframework/xd/spring-xd/1.0.0.M4/spring-xd-1.0.0.M4-dist.zip).
2. Move it to your preferred folder and unzip it.
3. Configure XD_HOME to point at that folder.


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
1.0.0.M4                         eXtreme Data

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
1.0.0.M4 | Admin Server Target: http://localhost:8080
Welcome to the Spring XD shell. For assistance hit TAB or type "help".
xd:>
```

Inside Spring XD's shell create a twitter stream:

    xd:> stream create --name twittersearchjava --definition "twittersearch --outputType=application/json --fixedDelay=1000 --consumerKey=afes2uqo6JAuFljdJFhqA --consumerSecret=0top8crpmd1MXGEbbgzAwVJSAODMcbeAbhwHXLnsg --query='java' | file"

Here you are creating a **stream** which consists of a *source* and a *sink*.

- The source is named **twittersearchjava**.
- The source is **twittersearch**, output formatted as JSON, every 1000 milliseconds, querying on the token *java*.
- The results are piped into the **file** sink, which defaults to **/tmp/xd/output/[streamName].out**

> **Note:** This example shows the output being written as JSON. Spring XD supports many more formats for both inputs and outputs. Read more about this at https://github.com/spring-projects/spring-xd/wiki/Type-Conversion.

In another terminal:

    $ cd /tmp/xd/output
    $ tail -f twittersearchjava.out

Spring XD is capturing live data from twitter about *java* and writing it to `twittersearchjava.out`. You should see something like this:

```json
{
	"id":394549063997460480,
	"text":"Hello world, do u need an experienced #Java #Software #Developer? Contact me ASAP. I work as a freelancer and full time. BB pin: 23AD2DCE","createdAt":1382902801000,
	"fromUser":"thejdeveloper",
	"profileImageUrl":"http://pbs.twimg.com/profile_images/378800000503045039/10ba9d12cdc8b130884e782bb9c999f9_normal.jpeg",
	"toUserId":0,
	"inReplyToStatusId":null,
	"inReplyToUserId":null,
	"inReplyToScreenName":"null",
	"fromUserId":584768355,
	"languageCode":"en",
	"source":"<a href=\"http://blackberry.com/twitter\" rel=\"nofollow\">Twitter for BlackBerry®</a>",
	"retweetCount":0,"retweeted":false,
	"retweetedStatus":null,
	"favorited":false,
	"entities":{
		"urls":[],
		"mentions":[],
		"media":[],
		"tickerSymbols":[],
		"hashTags":[
			{"text":"Java","indices":[38,43]},
			{"text":"Software","indices":[44,53]},
			{"text":"Developer","indices":[54,64]}
		]
	},
	"user":{
		"id":584768355,
		"screenName":"thejdeveloper",
		"name":"Johnson Ayomide",
		"url":"http://t.co/fLa7IYnE9f",
		"profileImageUrl":"http://pbs.twimg.com/profile_images/378800000503045039/10ba9d12cdc8b130884e782bb9c999f9_normal.jpeg",
		"description":"",
		"location":"Nigeria",
		"createdDate":1337428353000,
		"language":"en",
		"statusesCount":1143,
		"friendsCount":76,
		"followersCount":90,
		"favoritesCount":0,
		"listedCount":1,
		"following":false,
		"followRequestSent":false,
		"notificationsEnabled":false,
		"verified":false,
		"geoEnabled":true,
		"contributorsEnabled":false,
		"translator":false,
		"timeZone":null,
		"utcOffset":0,
		"sidebarBorderColor":"C0DEED",
		"sidebarFillColor":"DDEEF6",
		"backgroundColor":"C0DEED",
		"backgroundImageUrl":"http://abs.twimg.com/images/themes/theme1/bg.png",
		"backgroundImageTiled":false,
		"textColor":"333333",
		"linkColor":"0084B4",
		"protected":false,
		"profileUrl":"http://twitter.com/thejdeveloper"
	},
	"retweet":false
}
```

This is a single tweet in [JSON](/understanding/JSON) format. The file actually contains many tweets, but that would fill up the guide. And while Spring XD runs, the file sink will continue to grow as it accumulates more data.
    
> **Note:** Actually, the JSON will be compacted and not displayed in a pretty format. This was altered for readability.

Summary
-------

Congratulations! You've just set up Spring XD and tapped a live twitter stream of data, piping it into a file.
