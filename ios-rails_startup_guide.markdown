# Guide to iOS 5 and Rails 3.1 Development
## Copyright (C) 2012 Garrison Prinslow

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

---

## 1. Introduction
This guide is intended to assist new developers to Rails who want to also build an iOS application that uses Rails services. It also provides some tips and introduces best practices that I used in my own project. Additional instructions and suggestions 

It provides step-by-step instructions for setting up an iOS + Rails development environment, with the main focus on Rails tools and setup.  

Next, the guide provides instructions on how to start an example iOS + Rails project, ending with instructions on how to integrate the iOS with Rails services.

### How current is this?

This guide has been edited and tested to work primarily with the following environment:

*	Mac OS X 10.7.3 (Lion)
* Xcode 4.2.1 & iOS 5.0.1
* Rails 3.1

The environment setup instructions maintain compatibility with the author's prior environment:

* Mac OS X 10.6.7 (Snow Leopard)
* Xcode 4.2 & iOS 5.0
* Rails 3.0

Any differences in instructions for alternate environments are noted in the guide.

### License & Sharing

This guide and associated examples are distributed under the [MIT License](http://www.opensource.org/licenses/MIT).

The guide is written entirely in .markdown to facilitate future updates.  I welcome forks and suggestions for improvement.

---

## 2. iOS Environment Setup

Developing with iOS requires installation of Apple's Xcode tool.

### 2.1. Xcode

1. Go to [http://developer.apple.com](http://developer.apple.com).

2. Click on the link for the [iOS Dev Center](http://developer.apple.com/devcenter/ios/).
	*	Bookmark this page, you will be going to it frequently.
		*	Note the documentation, downloads, and featured content (which include Apple produced guides).
		*	This page provides official answers for many topics, but may not be accessible or tailored to your situation.
	*	In order to download Xcode 4, you may need to [register](http://developer.apple.com/programs/register/) as an Apple Developer.  It is probably a good idea to do so anyway.
		*	You can register for free, but you will not be able use an actual device for app testing, nor can you submit apps to the App Store.
		*	You can pay to $99 per year for individual Developer access.  The key benefit here is that you can put your applications on an actual device for testing (as opposed to a simulated device), and submit apps to the App Store.
			*	Alternatively, if you are a student in an iOS development course, they may have an academic team set up for you.
			*	Other more expensive plan options are aimed at corporate developer teams.

3. [Download](https://developer.apple.com/devcenter/ios/index.action#downloads) the latest Xcode version in the Downloads section.  This will include the appropriate iOS SDK. 

	1.	OS X 10.7 (Lion): click on the [Xcode](https://developer.apple.com/xcode/index.php) link and then click on the button to View in Mac App Store.

	2.	OS X 10.6 (Snow Leopard): click on [View Downloads](https://developer.apple.com/downloads/) and locate Xcode for Snow Leopard.

4. Install the version of Xcode you downloaded.

	1.	Follow the prompts from the installer, default options are fine.
	2.	After installation Xcode will start and show the Welcome window.
	3.	Installation of Xcode is complete.  You may close it for now.

### 2.2. Workspace folder

1.	For experienced Rails/iOS developers, this next step may be too basic.  It is merely a personal recommendation in setting up a development workspace folder.
2.	Rails development involves significant use of the terminal (console).  My tips:
	*	Simpler paths tend to save time.
	*	Using all lower case names is easier for autocomplete in the terminal (tab button).
3.	With this in mind, here is an example setup:
	1.	Within your Home (`~`) folder, create a folder called `dev`.
	2.	Within `~/dev/` create two folders:
		1.	An iOS workspace: 	`~/dev/ios`
		2.	A Rails workspace: 	`~/dev/rails`
4.	The folder `ios` will similarly be the folder you go to when you create new iOS 5 projects. When you create a new project, Xcode automatically makes a top-level folder with that project name.
5.	The folder `rails` will be the folder you go to when you create new Rails projects. Similarly, when you create a new project, Rails automatically makes a top-level folder with that project name.

---

## 3. Rails Environment Setup

### 3.1. Rails Development Tools

1.	Some Rails developers prefer an IDE, and some options include: RadRails, RubyMine, and 3rd Rail.  
2.	However, many Rails developers simply use a capable Text Editor and the Terminal.
	*	If you like GUI editors, I recommend [TextMate](http://macromates.com/), because it has good syntax highlighting for Ruby/Rails, comprehensive options, is very stable, and has good organization of your project folder.  Rails projects have a lot of folders and subfolders so this is important.  I do not receive any benefit from this recommendation, so it is not biased by that motivation.
	*	Naturally, there are many other capable editors, both GUI and non-GUI.  Feel free to use whatever you like most, but I will be using TextMate in this guide.  You would probably want to look for Ruby/Rails syntax highlighting, however.
	*	The Terminal app provided with Mac OS X (especially Lion) is quite capable.  However, there are some additional features that make [iTerm 2](http://www.iterm2.com/) worth a look.
		*	For example, receiving Growl notifications from terminal tabs/windows is helpful for automated testing and the local rails server.
3.	Install and set up your selected combination of IDE, Text Editor, and Terminal to your personal preferences.

### 3.2. Browser

1.	Web browsers are a personal choice but it is advisable to install up-to-date versions of popular browsers to cross-check for compatibility and appearance testing.
2.	I personally use Chrome regularly and cross-check on FireFox and Safari. The Firebug add-on for FireFox can be very useful for inspecting problematic HTML/CSS.

### 3.3. Ruby Version Manager (RVM)

1.	The next step is to install Ruby Version Manager (RVM), which manages Ruby versions installed on the same machine.
2.	The [RVM installation instructions](https://rvm.beginrescueend.com/rvm/install/) for a single user environment are summarized here but may not be the latest version.
3.	Install the latest stable version of RVM:
		
		$ bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)

4.	Load RVM into your shell session by adding the RVM function to your `.bash_profile`:

		$ echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function' >> ~/.bash_profile

5.	Close your current shell/terminal and open a new one to reload.  Or, run:

		$ source ~/.bash_profile

6.	Test that RVM was installed and configured successfully with the following command, which should output `rvm is a function` as shown:
		
		$ type rvm | head -1
		rvm is a function

7.	Determine if there are any dependency requirements for your OS by running:
	
		$ rvm requirements
		
8.	You can try out RVM with the following command, which lists known rubies:

		$ rvm list known

9.	Installation of RVM is now complete.

### 3.4. Ruby

1.	Check what version of Ruby you currently have (if any):

		$ ruby -v

2.	Get the latest (head) version of RVM and reload:

		$ rvm get head && rvm reload

3.	Install Ruby 1.9.3 (for Rails 3.1):

	1.	For OS X 10.7 (Lion): [source](http://stackoverflow.com/questions/8032824/cant-install-ruby-under-lion-with-rvm-gcc-issues)
		
			$ rvm install 1.9.3 --with-gcc=clang
		
	2.	For OS X 10.6 (Snow Leopard):
	
			$ rvm install 1.9.3

4.	Create a gemset:

		$ rvm --create 1.9.3@rails3
		
5.	Use the gemset you created as default:

		$ rvm --default use 1.9.3@rails3

6.	Installation of Ruby is now complete.

### 3.5. RubyGems

1.	[RubyGems](http://rubygems.org/) manages the gems (packages) you use in your Ruby projects, and makes installing, using and uninstalling gems straightforward.
2.	RubyGems should have been included with the RVM installation.  To verify, run:
		
		$ which gem

3.	If RubyGems is installed, then just update the RubyGem system:

		$ gem update --system

4.	If RubyGems is *not* installed, then it must be installed manually:

	1.	Download the latest version [here](http://rubygems.org/pages/download)
	2.	Unpack the package into a directory
	3.	In that directory, install it by running the command:
	
			$ ruby setup.rb
			
5.	As a final (optional) step, you may suppress automatic documentation to speed up gem installation.

	1.	Create a text file called `.gemrc` in your home `~` directory.
	2.	Add the following line to this `.gemrc` file:
	
			gem: --no-ri --no-rdoc

6.	RubyGems is now installed.
 
### 3.6. Rails

1.	The first step to installing Rails is adding entries to your PATH:

		$ export PATH="/usr/local/bin:/usr/local/sbin:/usr/local/mysql/bin:$PATH"
		
2.	Next, run the installation command:

		$ gem install rails
		
		Note that you may also specify a specific version, e.g.
		
				$ gem install rails --version 3.1.0
				
3.	When the installation completes, check that Rails is installed by running:

		$ rails -v

4.	The version of Rails will display if installed correctly.  If it does not, ensure that you have added the entries to your PATH per Step 1.

5.	Rails is now installed.

### 3.7. Git

1.	This step is not technically required but if you want any version control on your Rails project, Git is one of the easiest to use.  Since its free, simple, and runs locally, there really is no reason *not* to use it.  Of course, you can use another type of source control, but I will cover Git in this guide.

2.	Git should have been installed by Xcode but it is advisable to install manually as well.  These instructions are adapted from the [Installing Git section of Pro Git](http://progit.org/book/ch1-4.html).

3.	On Mac OS X 10.6/10.7: The easiest way is to install Git using the [Git-OSX-Installer](http://code.google.com/p/git-osx-installer/downloads/list).  Select an appropriate package and install according to its instructions.  Note that Snow Leopard packages will work on Lion.

### 3.8. Github [optional]

### 3.9. Dropbox [optional]

---

## 4. Starting an iOS Project

* General note: Advise naming project as you would want it to appear in App Store.  Denote prototypes using Git branches.

---

## 5. Starting a Rails Project

---

## 6. Integrating iOS with Rails