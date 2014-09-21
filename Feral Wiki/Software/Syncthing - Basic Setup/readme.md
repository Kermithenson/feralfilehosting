
[Syncthing - Secure & Private](http://syncthing.net/)
---

![](1.png)

Install syncthing:
---

~~~
mkdir -p ~/bin && source ~/.{profile,bashrc}
wget -qO ~/syncthing.tar.gz https://github.com/syncthing/syncthing/releases/download/v0.9.17/syncthing-linux-amd64-v0.9.17.tar.gz
tar xf ~/syncthing.tar.gz
mv ~/syncthing-linux-amd64-v*/syncthing ~/bin/
cd && rm -rf syncthing{-linux-amd64-v*,.tar.gz}
~~~

Configure syncthing:
---

Now it is ready to run so that we can create the required configuration files.

Do this command in SSH to run it:

~~~
syncthing
~~~

Wait until it has fully loaded and created the required files then exit the process pressing  and holding `CTRL` then pressing `c`. It will look like this:

![](2.png)

1: We run the binary
2: We wait until the process has fully loaded and configured itself.
3: we exit the process using `CTRL` + `c`

Editing the` config.xml`
---

Do this command in SSH:

~~~
nano ~/.config/syncthing/config.xml
~~~

Your  generated configuration file will load and look like this. We need to make a few changes to suit our needs.

~~~
<configuration version="4">
    <repository id="default" directory="/media/DiskID/home/username/Sync" ro="false" rescanIntervalS="60" ignorePerms="false">
        <node id="N67TTCK-CW4EXN4-B3HD6TW-OYMXTQG-OWIKUI5-YJZDH2Q-7WUT6RL-F54DLAU"></node>
        <versioning></versioning>
    </repository>
    <node id="N67TTCK-CW4EXN4-B3HD6TW-OYMXTQG-OWIKUI5-YJZDH2Q-7WUT6RL-F54DLAU" name="server" compression="false">
        <address>dynamic</address>
    </node>
    <gui enabled="true" tls="false">
        <address>127.0.0.1:8080</address>
    </gui>
    <options>
        <listenAddress>0.0.0.0:22000</listenAddress>
        <globalAnnounceServer>announce.syncthing.net:22026</globalAnnounceServer>
        <globalAnnounceEnabled>true</globalAnnounceEnabled>
        <localAnnounceEnabled>true</localAnnounceEnabled>
        <localAnnouncePort>21025</localAnnouncePort>
        <localAnnounceMCAddr>[ff32::5222]:21026</localAnnounceMCAddr>
        <parallelRequests>16</parallelRequests>
        <maxSendKbps>0</maxSendKbps>
        <maxRecvKbps>0</maxRecvKbps>
        <reconnectionIntervalS>60</reconnectionIntervalS>
        <startBrowser>true</startBrowser>
        <upnpEnabled>true</upnpEnabled>
        <upnpLeaseMinutes>0</upnpLeaseMinutes>
        <upnpRenewalMinutes>30</upnpRenewalMinutes>
        <urAccepted>0</urAccepted>
        <restartOnWakeup>true</restartOnWakeup>
    </options>
</configuration>
~~~

**Change 1:** The WebUi address and port to port between the range of `6000` to `50000`:

~~~
<address>127.0.0.1:8080</address>
~~~

Change the hostname to `0.0.0.0` and port 

~~~
<address>0.0.0.0:29384</address>
~~~

**Change 2:** The programs listening port to port between the range of `6000` to `50000` that is NOT the default `22000`:

~~~
<listenAddress>0.0.0.0:22000</listenAddress>
~~~

Change it to something else:

~~~
<listenAddress>0.0.0.0:12532</listenAddress>
~~~

**Change 3:**

Find this setting:

~~~
<upnpEnabled>true</upnpEnabled>
~~~

And set it to false:

~~~
<upnpEnabled>false</upnpEnabled>
~~~

Then press and hold `CTRL` and then press `x` to save. Press `y` to confirm.

Accessing the Webui:
---

Now we are ready to relaunch the properly configured syncthing so that we can access and use the WebUi.

in SSH do run this command:

~~~
syncthing
~~~

Now the WebUi should load on the port we configured earlier.

This is what you will see when the syncthing WebUi loads, the first thin we need to do it click on the Options icon in the top right:

![](3.png)

Now click on `Settings`:

![](4.png)

Inside the settings you can set a usertname and password for the WebUi. Do this now:

![](5.png)

You will need to restart the syncthing server for this to take effect:

![](6.png)

Repository settings:
---

Each repository can be configured via the `Edit` button:

![](7.png)

This is the options window for a repository:

![](8.png)