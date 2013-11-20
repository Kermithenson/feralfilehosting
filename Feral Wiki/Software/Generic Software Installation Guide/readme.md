
Generic Software Guide Introduction:
---

With your Feral slot you do not have `root` access to the server. Your account runs as user account in a Debian linux environment. These are not dedicated servers which means you cannot use these commands on your slot:

~~~
apt-get
su
sudo
~~~

This does not mean you cannot install software on your slot. It does means that you will have to attempt to install it to your HOME directory or below manually. For this there is no official support from Feral staff. For this guide we will look at the methods available to you to install software on your slot.

The main methods are:

Pre-compiled binaries created by the developers or similar. Examples of this include:

- ffmpeg
- java

Source-code to compile the binary on your slot. Examples of this include:

- git
- znc

Some Debian packages via unpacking them to access the binary.

- Spideroak

It is important that you understand that no one of these methods is a guaranteed thing. There are many factors that will contribute to the success or failure of the attempt. These could be dependencies on other applications, binaries, libraries or additional requirements that you have no way to fulfil. This is just something you will have to accept before attempting to install any software on your slot. In this linked examples you will see various methods and solutions to such issues.

Installed Software locations:
---

Throughout the Feral FAQs you will see that all user installed programs are installed to a single location which is:

~~~
~/programs
~~~

And then this location is added to the `PATH`:

~~~
~/programs/bin
~~~

When we add a location to the `PATH` in Linux it basically tells the SSH terminal to include this location when it looks for the binaries to execute. If a location is not in the `PATH` you would be required to use a full path to that program in order to execute it in your terminal. This method is used for simplicity. All files are installed to the `~/programs` folder and generally maintain a standardised directory structure.

This FAQ will use the same directory structure for any examples used, though once you understand the process you can install application where ever you want. 

Alternative Software locations:
---

Say you wanted multiple versions of a program installed. If you installed them all to `~/programs` you would just overwrite the previous version as it installed. To avoid this you would use a different `--prefix` when configuring, for example:

~~~
--prefix=$HOME/python27
~~~

~~~
--prefix=$HOME/python30
~~~

Or if you wanted all custom software in a managed location:

~~~
--prefix=$HOME/applications/python/python27
~~~

~~~
--prefix=$HOME/applications/python/python30
~~~

You could then manage you installations and have more than one version of a program.

CMAKE
---

Some application do not use `configure` and instead require you use`cmake` to build them. You can use this guide to install and use `CMAKE`:

[CMAKE - Basic Setup](https://www.feralhosting.com/faq/view?question=270)

--prefix=$HOME
---

It is very common for source code application to come with an executable file called `configure`. When using this program to configure our source code for installation we can often pass it some arguments that tell it to do certain things or behave in a certain way. The most important of theses is the `--prefix` argument.

If you do not specify a `prefix` what happens is that `configure` will assume you want to use the standard file structure such as `/etc` and `/lib` for which you do not have permission to use. So what we do is specify a `--prefix` that tell `configure` where to install the application and subsequently where to find the files it needs to run.

So when we pass this argument to `configure`

~~~
--prefix=$HOME/desired-location
~~~

We are telling it to install the application to `desired-location` in our `HOME` directory.

**Important note:** You can read the `configure` file with a text editor. In this file you will be able to find and see the available arguments you can use with this installation when you encounter some errors.

In some cases `configure` does not exist or is not included with the files and you must refer to the developers documentation to see how the files are required to be processed.

PATH explained:
---

The concept of the `PATH` for new users can be a difficult one to come to terms with so I will briefly explain it here.

Your terminal makes use of all kinds of variables when processing information. `PATH` is one of these variables. `PATH` is basically just a list of locations, in order of preference from left to right, that the shell will look at when searching for a binary to execute. So when I use certain command in the terminal like `cp` or `mv` or `rm` I don't have to use the full path to the binary each and every time. Being a variable is means we can modify those locations to suit our needs and this will in turn be reflected in the behaviour of our terminal. The command for doing this will be used and explained through the FAQ.

One thing to consider when adding locations to the `PATH` variable is the order in which you wish the locations to be checked

~~~
PATH=~/programs/bin:$PATH
~~~

This is the variable we are modifying:

~~~
PATH=
~~~

This is the location we are including:

~~~
~/programs/bin
~~~

This means that we want to come before, or on the left of the existing `PATH` locations

~~~
:$PATH
~~~

To have it come after you would do this:

~~~
PATH=$PATH:~/programs/bin
~~~

And example of this in use:

Let us say when have installed the statically pre-compiled `ffmpeg` binary to our slot. How do we use this over the existing `ffmpeg` already installed on the slot?

If we have installed it to `~/programs` then we would use this `PATH` in our `~/.bashrc`:

~~~
PATH=~/programs/bin:$PATH
~~~

If we instead did this, it would find the existing `ffmpeg` before find our own installation since it will look in the existing `PATH` locations first and it will find the default installation of `ffmpeg`:

~~~
PATH=$PATH:~/programs/bin
~~~

For more information see this page - [internal variables.](http://tldp.org/LDP/abs/html/internalvariables.html)

wget usage in this FAQ
---

In this FAQ and others you will see  this command format used. I will briefly explain what it is and why it is used.

~~~
wget -qO ~/node.js.tar.gz http://somesite/somefile.tar.gz
~~~

`wget` - is the command that we wish to use to download the file
`-qO ~/somefile.tar.gz` - are the argurments we pass to `wget` so it performs some specific behaviour.
`http://somesite/somefile.tar.gz` - is the example URL to the pre-compiled file we want.

`q` tells `wget` to be quiet about the download and not to show us in the terminal.

`O ~/somefile.tar.gz` tells `wget` to name this file `somefile.tar.gz` in the root of you slot. This is because I use `~/` prefixed to the name of the file I have specified. `~` known as a `tilde` is basically shorthand for the full path to our `HOME` directory when used in a `BASH` shell. This shell is the default shell you will be using with your Feral slot.

~~~
~
~~~

Is the same as the path to your HOME folder, which will look something like this:

~~~
/media/12345/username/home
~~~

So this means that:

~~~
~/somefile.tar.gz
~~~

Is the same as:

~~~
/media/12345/username/home/somefile.tar.gz
~~~

This method is used to ensure the files we want are placed in a specific location that we can use to continue the installation.

**Important notes:**

With some URLs you may need to quotes the URL when certain characters are used. For example, using single quotes:

~~~
'http://somesite/somefile.tar.gz'
~~~

Some URLs do not return the file we actually want and in this case you can try to use this argument with `wget`:

~~~
--content-disposition
~~~

1: Pre-compiled binaries
---

In this example we will look at two particular applications that both have existing FAQs here.

[node.js - How to install](https://www.feralhosting.com/faq/view?question=199)

~~~
wget -qO ~/node.js.tar.gz http://nodejs.org/dist/v0.10.22/node-v0.10.22-linux-x64.tar.gz
~~~

Now we have downloaded the archive to our `HOME` folder we can unpack it. In this case the archive is a tar archive to we will use `tar` to extract it.

~~~
tar xf ~/node.js.tar.gz
~~~

**Important note:** Effectively, once you have extracted the the contents of the archive, the program is ready to use by directly calling it via a full path in you terminal using `~/node-v0.10.22-linux-x64/bin/node` . For the purposes of this FAQ will continue to move the files to a desired location and add that location to our `PATH` for easy use and so you become more familiar with the overall process.

Now the contents of the archive has just been extracted to the `HOME` folder of our slot. It will generally be safe to assume the new folder name is the same as the archive's minus the extension, but to make sure, we can type this command in our shell:

~~~
ls
~~~

As we can see the folder name is `node-v0.10.22-linux-x64`

![](https://raw.github.com/feralhosting/feralfilehosting/master/Feral Wiki/Software/Generic Software Installation Guide/lsnode.png)

The directory structure of the pre compile programs will commonly resemble the standard Unix directory structure.

![](https://raw.github.com/feralhosting/feralfilehosting/master/Feral Wiki/Software/Generic Software Installation Guide/nodedirectory.png)

So what we need to do is copy the contents of the folder to our desired location:

~~~
cp -rf ~/node-v0.10.22-linux-x64/. ~/programs
~~~

Now we add this location to our `PATH` using the special command.

~~~
[[][/[][[ ! "$(grep '~/programs/bin' ~/.bashrc)" ]] && echo 'export PATH=~/programs/bin:$PATH' >> ~/.bashrc ; source ~/.bashrc
~~~

This command is meant to be reusable without duplicating the intended result. It will check to see if the location already exists before appending to the `~/bashrc` and reloading the file. You do not need to understand the entire command, just the intended function.

So you have now installed `node` and it is ready for use by simple called `node` in the terminal.

~~~
node -v
~~~

And that is how you can install and use software that has be pre compiled to run on the linux platform.

**Important note:** Not every pre-compiled binary will conform to this FAQ and might have a unique directory structure that doe not quite fit in with what was described in this section. See the `ffmpeg` example to see this.

2: Compiling from source
---

When compiling from source you will need to follow this outline, we will use `curl` as the installation example:

First, we download the software to a file named `curl.tar.gz` in our `HOME` directory using this command:

~~~
wget -qO ~/curl.tar.gz http://curl.haxx.se/download/curl-7.33.0.tar.gz
~~~

Now we need to extract the archive

~~~
tar xf ~/curl.tar.gz
~~~

Now the contents of the archive has just been extracted to the `HOME` folder of our slot. It will generally be safe to assume the new folder name is the same as the archive's minus the extension, but to make sure, we can type this command in our shell:

~~~
ls
~~~

![](https://raw.github.com/feralhosting/feralfilehosting/master/Feral Wiki/Software/Generic Software Installation Guide/lscurl.png)

As we can see the folder name is `curl-7.33.0`. So now we move into this folder using the `cd` command:

~~~
cd ~/curl-7.33.0
~~~

Now we will configure `curl` to use our desired installation location, which is `~/programs`:

~~~
./configure --prefix=$HOME/programs
~~~

Once the configuration is complete you can `make` the file:

**Important note:** This stage can take a very long time. The time taken varies from one application to the other.

~~~
make
~~~

If this step completed successfully you can now `make install` the program to finish the process.

~~~
make install 
~~~

Once this command has completed you can leave the folder using the `cd` command. Using just `cd` on its own will return you to the `HOME` directory.

~~~
cd
~~~

You can now remove the downloaded files. As you can see in this command there is one folder `~/curl-7.33.0` and one archive `~/curl.tar.gz` being removed. You can chain more than one file or folder to be removed this way.

~~~
rm -rf ~/curl-7.33.0 ~/curl.tar.gz
~~~

Now we add this location to our `PATH` using the special command.

~~~
[[][/[][[ ! "$(grep '~/programs/bin' ~/.bashrc)" ]] && echo 'export PATH=~/programs/bin:$PATH' >> ~/.bashrc ; source ~/.bashrc
~~~

This command is meant to be reusable without duplicating the intended result. It will check to see if the location already exists before appending to the `~/bashrc` and reloading the file. You do not need to understand the entire command, just the intended function.

Do this command to call `curl` and check it's version.

~~~
curl -V
~~~

**Important note:** Not every source-code package will conform to this FAQ and might have unique requirements or hard coded settings that cannot be tweaked to suit the Feral slot with the scope of this FAQ. See the `git` FAQ for an example of this.

3: Debian packages
---

To explain is simply, a `deb` is an archive that contains folders and files using a standard directory structure. We can extract the `deb` and attempt to use the files.

First download the `deb` that you want to extract:

~~~
wget -qO ~/some.deb 'https://somesite/some.deb'
~~~

Now we will extract the `deb` using the `dpkg-deb` command. We will tell it where we want the files to be extracted to. It will create this location if it does not exist. In our example this location is `desired-location`. So we are going to extract `some.deb` to `desired-location`.

~~~
dpkg-deb -x ~/some.deb ~/desired-location
~~~

Now you will find that the contents of the `desired-location` contain a standard directory structure. This is because file would generally be extracted to the core system folders and not a subdirectory when installed using a `deb`. 

![](https://raw.github.com/feralhosting/feralfilehosting/master/Feral Wiki/Software/Generic Software Installation Guide/debdirectory.png)

So to make it more user like we can do this:

~~~
cp -rf ~/desired-location/usr/bin/. ~/desired-location
~~~

~~~
rm -rf ~/desired-location/etc ~/desired-location/usr ~/some.deb.deb
~~~

Examples:
---

1: Pre-compiled examples
---

[node.js - How to install](https://www.feralhosting.com/faq/view?question=199)

[Java 1.7](https://www.feralhosting.com/faq/view?question=183)

[p7zip - basic installation](https://www.feralhosting.com/faq/view?question=245)

[Teamspeak 3 server](https://www.feralhosting.com/faq/view?question=175)

[BitTorrent Sync btsync - basic setup](https://www.feralhosting.com/faq/view?question=224)

[Mumble client and murmur server](https://www.feralhosting.com/faq/view?question=227)

[ffmpeg](https://www.feralhosting.com/faq/view?question=268)

2: Source-code examples
---

[git](https://www.feralhosting.com/faq/view?question=206)

[ruby](https://www.feralhosting.com/faq/view?question=265)

[python](https://www.feralhosting.com/faq/view?question=204)

[znc](https://www.feralhosting.com/faq/view?question=264)

[duplicity](https://www.feralhosting.com/faq/view?question=255)

[Curl](https://www.feralhosting.com/faq/view?question=267)

[mosh](https://www.feralhosting.com/faq/view?question=269)

3: Debian packages examples
---

[SpiderOak](https://www.feralhosting.com/faq/view?question=203)

[FileBot CLI - Basic Setup](https://www.feralhosting.com/faq/view?question=256)


