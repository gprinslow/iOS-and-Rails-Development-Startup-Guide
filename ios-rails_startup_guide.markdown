# iOS and Rails Development Startup Guide
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
		
	*	Note that you may also specify a specific version, e.g.
		
			$ gem install rails --version 3.1.0

3.	When the installation completes, check that Rails is installed by running:

		$ rails -v

4.	The version of Rails will display if installed correctly.  If it does not, ensure that you have added the entries to your PATH per Step 1.

5.	Rails is now installed.

### 3.7. Git

1.	This step is not technically required but if you want any version control on your Rails project, Git is one of the easiest to use.  Since its free, simple, and runs locally, there really is no reason *not* to use it.  Of course, you can use another type of source control, but I will cover Git in this guide.

2.	Git should have been installed by Xcode but it is advisable to install manually as well.  These instructions are adapted from the [Installing Git section of Pro Git](http://progit.org/book/ch1-4.html).

3.	On Mac OS X 10.6/10.7: The easiest way is to install Git using the [Git-OSX-Installer](http://code.google.com/p/git-osx-installer/downloads/list).  Select an appropriate package and install according to its instructions.  Note that Snow Leopard packages will work on Lion.

4.	Following installation of Git, the following setup commands are necessary:

	1.	Set up your user name and email address:
	
			$ git config --global user.name "Your Name"
			$ git config --global user.email youremail@example.com
			
	2. Identify your text editor, e.g. TextMate:
	
			$ git config --global core.editor "mate -w"
			
	3. To save time, make an alias of the checkout command:
	
			$ git config --global alias.co checkout

5.	Your installation of Git is complete for purposes of this guide.  Other options are well documented [here](http://progit.org). 

### 3.8. Github [optional]

1.	Installation of Github is optional because by default, Github shares all code publicly.

	*	Getting a private repo is not cheap.  So this is best used for projects that will not be commercialized, or you are willing to pay for the private repository.
	
	*	However, you may feel warm and fuzzy about sharing and helping out the open source community.  Its up to you.
	
2.	This guide will proceed assuming that Github is being used.  Any Github-specific steps may be skipped if you are not using it.

3.	Go to [Github](https://github.com/).

4.	Sign up for Github account [here](https://github.com/signup/free).

5.	Follow the instructions to create your keys and set up your account on Github.

	*	You may also refer to the steps [here](http://help.github.com/mac-set-up-git/), e.g. for re-installing on a new computer.
	
6.	Your installation of Github is now complete.  You may also download [Github for Mac](http://mac.github.com) as an alternative to the command line.

### 3.9. Dropbox [optional]

1.	Dropbox is a popular cloud file backup service that may be used in conjunction with Git.  This material is adapted from [here](http://www.andyhawthorne.net/2011/09/git-and-dropbox-together/).

	*	Dropbox versions files and syncs across devices.  It is free up to 2GB.  Unlike Github, it is private by default.
	*	Cloning repos through Dropbox makes it easier to work on the same project using multiple computers.

2.	[Sign up](https://www.dropbox.com/) for a Dropbox account.

3.	[Download] Dropbox and install it.

4.	Create an directory for your repository backups.

	* For example, `~/Dropbox/dev/repos`.
	* Note that any directory within `Dropbox/Public` is shared publicly by default.
	
5.	Installation of Dropbox is complete.  See the section below 'Starting a Rails Project' for further instructions.

---

## 4. Starting an iOS Project

1.	This step is better covered by other guides on iOS development.
	
	*	A good starting point is the [official starting guide from Apple](https://developer.apple.com/library/ios/#referencelibrary/GettingStarted/RoadMapiOS/Introduction/Introduction.html).
	* Of the books I have read, I like the [Big Nerd Ranch Guide to iOS Programming](http://www.bignerdranch.com/book/ios_programming_the_big_nerd_ranch_guide_nd_edition_) the most.
	
2.	You may keep your iOS project independent of the Rails project. This is probably the simplest approach. A few caveats, however:

	*	I advise naming your iOS project as you would want it to appear in the App Store.  Prototype/Version names are best tracked using Git branches.
	*	If you reach a significant milestone in the project (e.g. you successfully integrated a service between your iOS and Rails app) it is advisable to clearly mark this point in *both* git projects at that time. This makes it easier to retrace your work if necessary.

---

## 5. Starting a Rails Project

### 5.1 Git Repositories

#### 5.1.1 Git Local Repository

1.	To start a new Rails project called "example", go to your Rails workspace, then type:

		$ rails new example

2.	Wait for the command to finish generating the template project files/directories. 
3.	Navigate to your project directory: `cd example`
4.	Set up your Git repository:
		
		$ git init

5.	Create a `.gitignore` file to avoid committing unnecessary files.  For example:

		$ mate .gitignore
		
	Then add the following to the .gitignore file:
		
		.bundle
		db/*.sqlite3*
		log/*.log
		*.log
		/tmp/
		doc/
		*.swp
		*~
		.project
		.DS_Store

6.	Save the `.gitignore` file.
7.	(Optional) Create and save a README or README.markdown file.
8.	Then add all the files in your project to git (except ones that match a .gitignore rule):

		$ git add .
		
9.	Then commit to the git repository with a descriptive comment:

		$ git commit -m "initial commit"
		
10.	Your basic git project setup is complete.

#### 5.1.2 Dropbox Repository Clone (Optional)

The following steps describe creating a clone of your local Git repository that is saved to Dropbox. The advantage of this approach is that you can easily work on multiple computers using the same local Git repository.

1.	Open a Terminal window.
2.	Navigate to your project directory, e.g.:
	
		$	cd ~/workspace/rails/project_name
	
3.	Initialize a local Git repository (if you haven't already; see step 5.1.1).
4.	Next, clone the local repository to Dropbox using the --bare option:

		$ git clone --bare . ~/Dropbox/dev/repo_clones/project_name.git

	The Dropbox directory above is provided as an example.  Also, be sure to name your `project.git` file according to your actual project name.
	
5.	Create a remote alias in order to push to the cloned repo:

		$ git remote add project_name_alias ~/Dropbox/dev/repo_clones/project_name.git
	
	Note that you can create an alias for your project here, and indeed it may help to name it differently from other remote destinations; e.g., `project-clone`.
	
6.	Work on your project normally in your local workspace.
7.	When you are ready to push to the cloned repository, the command is:

		$ git push project_name_alias master
		
	Where `master` is the branch you are pushing.

8.	If you want to work on a different computer, install as described to this point. Then, to clone the project:

		$ git clone ~/Dropbox/dev/repo_clones/project_name.git
		
9.	Continue working as before and push back to the repo.

#### 5.1.3 Github (Optional)

Do the following steps to create any new project that you want backed up to Github:

1.	Go to https://github.com and log in with your account.
2.	Select “New Repository”.
3.	On the Create a New Repository dialog:

	*	Project Name: how you will refer to it (alias) locally and the project name visible to others
	*	Description: (Optional) description of your project
	*	Homepage URL: (Optional) URL for your project in general

4.	Once you have created the new repository it will give you instructions.
5.	In this case, because you have already created the repository, just add the remote alias:
		
		$	git remote add origin git@github.com:<username>/<github_project_name>.git

6.	Then push the code to Github:
		
		$	git push -u origin master
		
7.	Setup of Github is complete.

### 5.2 Deploying Your Rails Project

Deploying your Rails project allows you to test its functionality. During development, the localhost server is most useful, but other deployment options are available as well.

#### 5.2.1 Localhost

1.	After all the setup, its time to take a look at your new app, is it is provided by default.
2.	If you are using TextMate, create a New Project, then drag your project folder to the corresponding area in TextMate.  This will provide an easy way to navigate the many files and folders in a Rails project.
3.	Look for the “Gemfile” in your project’s root directory.  This contains a list of the gems that should be included in your project, with the option of specifying them by environment. You do not need to change anything right now, however.
4.	To use the Gemfile and install the appropriate gems, run this command:
		
		$ bundle install
5.	After the gem installation completes, you can run the following command to run a web server for your app as localhost:

		$ rails server

6.	The output on the console will tell you that the app is running on port 3000 at address 0.0.0.0.  You can then view the web app by going to:

		http://localhost:3000/ (which corresponds to 127.0.0.1)

7.	You will then see the default welcome screen provided by Rails -- You’re riding Ruby on Rails! 
8.	You might want to take a look at your application’s environment, although you need not change anything right now.

#### 5.2.2 Deploying to Heroku (Optional)

1.	While it is a bit early to be thinking about deployment, especially with an empty starter app, setting it up now will make it easier to deploy incrementally and test in a non-local environment.
2.	Unless you are a corporate developer (team) who can afford hosting with Engine Yard, the recommended approach is Heroku.  Heroku offers free plans to get started, and if you need more performance/storage/etc. you can upgrade later.  It is quite easy to deploy your app, so long as you are using Git.  This guide assumes you are using Git.
3.	First, you need to [sign up](https://api.heroku.com/signup) for a Heroku account.  After signing up, follow any provided instructions.
	
	*	Note that if you already have a Heroku account, but are re-installing, there is a [quickstart guide](http://devcenter.heroku.com/articles/quickstart) that describes these steps.
	
4.	Install the heroku gem:

	$ gem install heroku
	
5.	Add your credentials (public key) to heroku, which you should have created for Git.  Run the command below and then enter your heroku account credentials:

		$ heroku keys:add

6.	Run the following command to create a new application where your app will be hosted:

		$ heroku create

	*	Take note of the URL.  Your app will be accessible at this URL from any browser after you push to it.
7.	To deploy to heroku use Git to push the app to heroku:
	
		$ git push heroku master

8.	Finally, to open your app using your default web browser as hosted on heroku:
	
		$ heroku open

9.	If you are not satisfied with the assigned subdomain, you may use the following command to rename the subdomain:
	
		$ heroku rename <new name>

10.	At some point, you will need to bootstrap your database; you may do it now:
	
		$ heroku rake db:migrate

	If it complains about not having a “activerecord-postgresql-adapter” gem:
	
	1.	Create the db:
				
			$ bin/rake db:create
					
	2.	Modify your Gemfile by adding:
					
			group :production do
				# gems specifically for Heroku go here
				gem 'pg'
			end
			
	3.	Install your Gemfile excluding production:
			
			$ bundle install --without production
			
	4.	Next, add and commit any uncommitted files:

			$ git add .
			$ git commit -m “Modify for Heroku”
			
	5.	Push the changes to heroku:
		
			$ git push heroku

	6.	Now, finally, you can re-run: 
	
			$ heroku rake db:migrate 

11.	That’s it! Heroku makes deploying your app quite straightforward.
12.	Later on, you may want to read about other features of [Heroku](http://www.heroku.com/).

### 5.3 Rails Development Cycle & Tips




---

## 6. Integrating iOS with Rails

### Introduction

This section seems to be the territory that is not well covered elsewhere on the web.  While there are many iOS apps that can be entirely self-contained, there are also fair portion that rely on backend data hosted on a server.  Web services are often the most logical choice to fulfill that need.

Rails is a good choice for a modern framework that is both service-oriented and supports a rigid MVC standard for developing its own web apps.  Depending on the level of service needed for your iOS app, you might need to develop highly atomic services that are SOA-focused.  However, getting started with Rails is relatively easy, and will support both a website as well as provide web services that are usable on iOS.

### References

In my own search for the best way to connect Rails and iOS, I consulted several resources:

*	[https://github.com/akosma/iPhoneWebServicesClient](https://github.com/akosma/iPhoneWebServicesClient)
* [http://www.slideshare.net/metaskills/synchronizing-core-data-with-rails](http://www.slideshare.net/metaskills/synchronizing-core-data-with-rails)
*	[http://iphoneonrails.com/](http://iphoneonrails.com/) -- A now defunct iPhone project to integrate with Rails.  I could not get it to work, at least with Rails 3+ and iOS 4+.
*	[http://iphonerails.org/Site/Book.html](http://iphonerails.org/Site/Book.html) -- This tutorial by Sergio Fernandez was very useful and served as a starting point for my own development. It makes use of portions of Michael Hartl's excellent [Ruby on Rails Tutorial](http://ruby.railstutorial.org/).

### Framework

Because I did not find a framework or project that supported web service interaction between iOS 5 and Rails 3.1, I developed my own framework and used it to support an iOS app that will be released soon.  I named the framework iOS-Rails-Interface or "iRI" for short.

I have shared **[iRI on Github](https://github.com/gprinslow)**, and I hope it may prove useful to you. I know it is far from perfect so I welcome constructive criticism, suggestions, forks, etc.

Please see the [iRI Github page](https://github.com/gprinslow) for details and instructions.

___

## 7. Appendix

### Useful Commands & Tips

*	Bundler:
	*	Default:	`bundle exec [command]`
	*	Install:	`bundle install --binstubs`
	*	Then:			`bin/[command]`

*	Common Commands:

		bin/spork
		bin/rake db:migrate
		bin/rake db:test:prepare
		autotest
		rails s
		rails s thin
		bin/annotate --position before

*	Running locally as “Production”:

		bin/rake db:migrate RAILS_ENV=production
		rails server --environment production

*	Precompile assets for Heroku:

		heroku rake db:migrate
		bundle exec rake assets:precompile

*	Reset records in database:
	
		bin/rake db:reset
		
*	Remove a file from git repository without deleting local copy:
	
		git rm --cached {filename}
		
*	Populate DB with sample users using lib/tasks/x.rake
	
		bundle exec rake db:populate
		
*	Push commands being used:
	*	`git push [origin <branch_name>]`					=> Github
	*	`git push clone [origin <branch_name>]`		=> Dropbox
	*	`git push heroku [origin <branch_name>]`	=> Heroku

---