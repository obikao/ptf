
The PenTesters Framework (PTF)

A TrustedSec Project - Copyright 2015

Written by: David Kennedy (@HackingDave)

https://www.trustedsec.com

Twitter: @TrustedSec, @HackingDave

The PenTesters Framework (PTF) is a Python script designed for Debian/Ubuntu based distributions to create a similar and familiar distribution for Penetration Testing. As pentesters, we've been accustom to the /pentest/ directories or our own toolsets that we want to keep up-to-date all of the time. We have those "go to" tools that we use on a regular basis, and using the latest and greatest is important.

PTF attempts to install all of your penetration testing tools (latest and greatest), compile them, build them, and make it so that you can install/update your distribution on any machine. Everything is organized in a fashion that is cohesive to the Penetration Testing Execution Standard (PTES) and eliminates a lot of things that are hardly used. PTF simplifies installation and packaging and creates an entire pentest framework for you. Since this is a framework, you can configure and add as you see fit. We commonly see internally developed repos that you can use as well as part of this framework. It's all up to you.

The ultimate goal is for community support on this project. We want new tools added to the github repository. Submit your modules. It's super simple to configure and add them and only takes a few minute.

#Instructions:

First check out the config/ptf.config file which contains the base location of where to install everything. By default this will install in the /pentest directory. Once you have that configured, move to running PTF by typing ./ptf (or python ptf).

This will put you in a Metasploitesque type shell which has a similar look and feel for consistency. Show modules, use <modules>, etc. are all accepted commands. First things first, always type help or ? to see a full list of commands.

###Update EVERYTHING!

If you want to install and/or update everything, simply do the following:

./ptf

use modules/install_update_all

run

This will install all of the tools inside of PTF. If they are already installed, this will iterate through and update everything for you automatically.

You can also show options to change information about the modules.

#Modules:

First, head over to the modules/ directory, inside of there are sub directories based on the Penetration Testing Execution Standard (PTES) phases. Go into those phases and look at the different modules. As soon as you add a new one, for example testing.py, it will automatically be imported next time you launch PTF. There are a few key components when looking at a module that must be completed.

Below is a sample module

AUTHOR="David Kennedy (ReL1K)"

DESCRIPTION="This module will install/update the Browser Exploitation Framework (BeEF)"

INSTALL_TYPE="GIT"

REPOSITORY_LOCATION="https://github.com/beefproject/beef"

INSTALL_LOCATION="beef"

DEBIAN="ruby1.9.3,sqlite3,ruby-sqlite3"

BYPASS_UPDATE="NO"

AFTER_COMMANDS="cd {INSTALL_LOCATION},ruby install-beef"

LAUNCHER="beef"

###Module Development:

All of the fields are pretty easy, on the repository locations, you can use GIT, SVN or FILE. Fill in the depends, and where you want the install location to be. PTF will take where the python file is located (for example exploitation) and move it to what you specify in the PTF config (located under config). By default it installs all your tools to /pentest/PTES_PHASE/TOOL_FOLDER

Note in modules, you can specify after commands {INSTALL_LOCATION}. This will append where you want the install location to go when using after commands.

###BYPASS UPDATES:

When using traditional git or svn as a main method, what will happen after a module is installed is it will just go and grab the latest version of the tool. With after commands, normally when installing, you may need to run the after commands after each time you update. If you specify bypass updates to YES (BYPASS_UPDATE="YES"), each time the tool is run, it will check out the latest version and still run after commands. If this is marked to no, it will only git pull the latest version of the system. For "FILE" options, it is recommended to always use BYPASS_UPDATE="YES" so that it will overwrite the files each time.

###After Commands:

After commands are commands that you can insert after an installation. This could be switching to a directory and kicking off additional commands to finish the installation. For example in the BEEF scenario, you need to run ruby install-beef afterwards.  Below is an example of after commands using the {INSTALL_LOCATION} flag.
 
AFTER_COMMANDS="cp config/dict/rockyou.txt {INSTALL_LOCATION}"

For AFTER_COMMANDS that do self install (don't need user interaction).

###Automatic Launchers

The flag LAUNCHER= in modules is optional. If you add LAUNCHER="setoolkit" for example, PTF will automatically create a launcher for the tool under /usr/local/bin/. In the setoolkit example, when run - PTF will automatically create a file under /usr/local/bin/setoolkit so you can launch SET from anywhere by simply typing setoolkit. All files will still be installed under the appropriate categories, for example /pentest/exploitation/setoolkit however an automatic launcher will be created.

You can have multiple launchers for an application - for example Metasploit you may want msfconsole, msfvenom, etc. etc. In order to add multiple ones, simply put a "," between them. For example LAUNCHER="msfconsole,msfvenom". This would create launchers for both.

#TODO:

* Support other operating systems aside from Kali, Ubuntu, Debian
