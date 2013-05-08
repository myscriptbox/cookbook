# Install the myscriptbox.org programs

The `myscriptbox.org` programs work with `debian` systems or systems derived from debian, such as `ubuntu`, `mint`. Follow the link to find an (incomplete) list of [debian-derived distros](http://www.debian.org/misc/children-distros).

You can find their source code here: [github.com/myscriptbox-org](https://github.com/myscriptbox-org)

## Complete list of myscriptbox.org programs

1. `deb-client`: install a scriptbox repository on your own machine
2. `msb-simul-server`: create a local simulation repository server
3. `msb-make`: create your own scriptbox program
4. `ssh-man`: manage your ssh keys (to execute instructions on remote publication servers)
5. `gpg-man`: manage your gpg keys (to sign your scriptbox program packages)
6. `deb-publish-root`: set up your own real (or simulated) publication repository
7. `deb-publish`: publish to a publication repository (yours or someone else's)

## Install deb-client

Add the myscriptbox.org key to your repository keys:

        $ wget -q -O - http://packages.myscriptbox.org/gpg.key |  sudo apt-key add -

Next, add the myscriptbox.org repository to a file in /etc/apt/sources.list.d:

        $ echo "deb http://packages.myscriptbox.org/apt/ubuntu precise main" | \
                sudo tee -a /etc/apt/sources.list.d/myscriptbox.list
        $ echo "deb-src http://packages.myscriptbox.org/apt/ubuntu precise main" | \
                sudo tee -a /etc/apt/sources.list.d/myscriptbox.list

next, refresh your apt package cache:

        $ sudo apt-get update

Now you can install `deb-client`:

        $ sudo apt-get install deb-client

And access its man page:

        $ man deb-client

You can see immediately what the problem is with these instructions. How do you know everything went ok? What do you do when there are error messages telling you that something went wrong? 

Trying to make it easier to troubleshoot failed repo installations, is exactly what the little `deb-client` program is about. Unfortunately, you first need to install `deb-client`, and therefore add a repo key and a repository, and therefore exactly do with the more difficult steps above. These steps is what `deb-client` is trying to streamline ...

## Working with deb-client

`deb-client` revolves around `keys` and `repos`. A key is represented by an email address (or a keyID). A `repo` (or repository) is a remote server from which you want to install packages. `deb-client` only manages additional repos, not the standard ones from where you update the distribution:

To view a list of accepted keys:

        $ deb-client keys -show

        ftpmaster@ubuntu.com                     437D05B5   Ubuntu Archive Automatic Signing Key
        cdimage@ubuntu.com                       FBB75451   Ubuntu CD Image Automatic Signing Key
        ftpmaster@ubuntu.com                     3E5C1192   Ubuntu Extras Archive Automatic Signing Key
        linux-packages-keymaster@google.com      7FAC5991   Google, Inc. Linux Package Signing Key
        signatory@packages.myscriptbox.org       D10FDEE7   myscriptbox.org packaging service

To view a list of additional repos:

        $ deb-client repos -show

        file                                     url                                                     release        
        google-chrome.list                       http://dl.google.com/linux/chrome/deb/                  stable         
        myscriptbox.list                         http://packages.myscriptbox.org/apt/ubuntu              precise        
        precise-partner.list                     http://archive.canonical.com/ubuntu                     precise        

In order to install additional `keys` and `repos` you can use additional `deb-client` commands. You will find the complete list of commands with:
        
        $ man deb-client

If `deb-client` had been installed already (this is indeed a chicken and egg problem), you could have installed the key and remote repository for `deb-client` as following:

        $ sudo deb-client key http://packages.myscriptbox.org/gpg.key -add
        $ sudo deb-client repo myscriptbox http://packages.myscriptbox.org/apt/ubuntu precise -add

You can remove it with:

        $ sudo deb-client key signatory@packages.myscriptbox.org -remove
        $ sudo deb-client repo myscriptbox.list -remove

From there, you can use the `deb-client keys -show` and `deb-client repos -show` commands to verify that everything went ok:

        $ deb-client keys -show

        ftpmaster@ubuntu.com                     437D05B5   Ubuntu Archive Automatic Signing Key
        cdimage@ubuntu.com                       FBB75451   Ubuntu CD Image Automatic Signing Key
        ftpmaster@ubuntu.com                     3E5C1192   Ubuntu Extras Archive Automatic Signing Key
        linux-packages-keymaster@google.com      7FAC5991   Google, Inc. Linux Package Signing Key

        $ deb-client repos -show

        file                                     url                                                     release        
        google-chrome.list                       http://dl.google.com/linux/chrome/deb/                  stable         
        precise-partner.list                     http://archive.canonical.com/ubuntu                     precise        

Now, you can add them again, and verify that everything went ok.

## What is myscriptbox.org?

`myscriptbox.org` is a method to facilate building programs that work a bit like `deb-client` and to facilitate publishing them through remote debian repositories. The main idea is to build a commandline program first and test it from the commandline. From there, it becomes possible to call this commandline programs in other commandline programs, or alternatively, produce a graphical user interface and supply the combination to end users, who will be able to install and use it.

The idea is to build these graphical user interfaces in javascript with JQueryMobile/PhoneGap for deployment to phones and JQuery/Bootstrap to deploy them to web, desktop, or tablet. Time permitting, I will add to the `myscriptbox.org` a few tools that will facilitate building graphical user interfaces calling into your backend commandline scriptbox programs over ajax. But first, you need to build the backend programs that will respond to your graphical interfaces.

As you have noticed, you were able to download the `deb-client` program from a remote debian repository. How to set that up? How to manage it? The next steps in this cookbook will consist in:

1. Create a simple commandline script
2. Set up a simulation repository server on your own laptop
3. Publish your commandline script to your simulation repository server
4. Install the scriptbox program on your own machine from the simulation repository server 

If you setup a real remote repository, you can ask your friends to install your scriptbox program too. Since it is not possible for the publication scripts to see the difference between a real remote repository and a simulation repository (one on your own laptop), you will already know how to do that.

For commercial programs, you can also set up a remote repository on a commercial basis and allow access to paying customers only. The `myscriptbox.org` publication process allows for this too by implementing an optional system of registered users.

There is no particular preference for any scripting language for backend programs in `myscriptbox.org`. The default router is a shell script, but the subcommands can be written in any language. You also do not need to use the default bash script subcommand router. You can use your own router, written in any scripting language as you prefer. 

There are languages that may not work particularly well, directly in a scriptbox. Languages that require a compilation and/or linking step are not directly supported. We miss C here. Time permitting, I will try to find a way to easily deploy C programs and libraries too, in conjunction with a scriptbox. It will probably be a separate project, because C has substantially more extensive compilation and linking requirements than a scripting language.

Languages that start their primary process slowly, such as Java, may also not be a good choice for a scriptbox implementation. Other than that, you can use any one of the typical languages in a scripting environment: Bash (or other shells), Javascript, PHP, Perl, Lua, Tcl, Python, name it. It will probably work.

As you will see later on, the idea in a scriptbox program will be to reuse existing commandline programs and existing logic that already exist somewhere in the `scripting balkans`. The idea is not to write an extensive amount of code, but to make lots of existing code easier to use and to test. Typically, you can find lots of library code already available in:

* Javascript ([npmjs.org](http://npmjs.org))
* PHP        ([packagist.org](http://packagist.org))
* Perl       ([www.cpan.org](http://www.cpan.org))
* Lua        ([luaforge.net](http://luaforge.net))
* Python     ([pypi.python.org](http://pypi.python.org))
* Tcl        ([wiki.tcl.tk](http://wiki.tcl.tk/16925))

`myscriptbox.org` programs tend to chain executables and not function APIs. Therefore, it is perfectly possible to mix and match scripting languages. You do not have to worry about inter-language compatibility. This also means that `myscriptbox.org` is not a suitable method to publish library function APIs. It is only suitable to publish program-based APIs. Since the vast majority of network APIs (such as ajax) are mandatorily program-based -and not function-based APIs, you can see that program-based APIs have their place too.

To facilitate communication between two programs, it is also required to pay close attention to data serialization and deserialization. Time permitting, I will also write a few remarks about this issue. 

#Next steps
Now that you have successfully installed access from your own machine to the remote scriptbox repository, you can already install/uninstall the other programs too, even before I finish this cookbook. For example:

        $ apt-get install msb-simul-server

To uninstall:

        $ apt-get remove msb-simul-server

You can do this with all scriptbox programs currently available from the remote scriptbox repository: `deb-client`, `msb-simul-server`, `msb-make`, `ssh-man`, `gpg-man`, `deb-publish-root`, `deb-publish`.

Alternatively, you can download the source code for a scriptbox program and manually put its source code on your system PATH. For example, to run `ssh-man` from a source code folder:

        $ cd /home/<yourself>
        $ git clone git@github.com:myscriptbox-org/ssh-man.git
        $ msb-make bashrc /home/<yourself>/ssh-man -add-path  # this adds ssh-man on your PATH
        $ source ~/.bashrc 

This kind of installation straight from github works too. However, installed and distributed like that, other developers will not be able to reliably use your scriptbox program, in order to call it from their own programs. If you do not provide a real installable package, that installs your scriptbox program in a standard location, re-using your program may become a problem. The download-from-github procedure is therefore not a viable package distribution method. It is better to really build a package.

#Remainder of the Cookbook
Coming soon ...

