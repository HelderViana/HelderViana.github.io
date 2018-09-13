---
layout: post
title: Setup my build server.
category: blog
tags: .net core .net setup build server agent pool build windows CI CD DevOps Azure VSTS Visual Studio Online Azure DevOps
comments: true
---


###Setup your Build Server on Windows
For this step by step article, I setup one daily Nightly Build task, for a .Net Core MVC Project that I'm working on (part time side project) in order to process a daily base build, from my master branch.
#So to setup a Build Server what technologies should I use?
I had at least 2 big options:
1 - Cruise Control Net (a.k.a. CCNet) that I already use on my daily work;
2 - Visual Studio Team Services (a.k.a. VSTS from now on), that I usualy use for my personal projects to manage, and to use for my version control (both TFS and git, most of the time git);

The answer was easy: let's go for the one that I know less, and lest's learn it!

##Requirements
Windows machine, to handle the build itself;
Visual Studio Team Services account.

![_config.yml]({{ site.baseurl }}/images/VSTS_view_01.png)

##Tech
For this Post I'll explain how I setup my own build server on Windows environment, for this, I used my VSTS / a.k.a Visual Studio Online / a.k.a. (recent) Azure DevOps account. For this porpose I also used one Windows 2016 Server, that is used for server web pages, using IIS. I use this machine to setup some DEV/Testing web hosting, most of the time just for fun.



##Steps
First I created a new project on my VSTS, then at root level setup the Agent Pool for the build process.

![_config.yml]({{ site.baseurl }}/images/VSTS_view_pipeline_wizard_01.png)

At the Project level, lets setup a new pipeline on the "Buid and release" menu. It is just like a wizard menu, select the most convinient option, related to the source of the project. A easy note, just to say that in here (VSTS), you can for example only set the pipeline of builds and you can still have your source control on premisses or other place that is not VSTS, like github, bitbucket or (the very familiar) SVN.
Select the template that feets your needs, I'll use ASP .NET Core (.NET Framework) for this.

![_config.yml]({{ site.baseurl }}/images/VSTS_view_pipeline_wizard_02.png)

Setup the pipeline build with the name and Agent Pool, and on the Triggers menu I just set the Scheduled, to run a build every day at 00:30 UTC time, only at the master branch.

![_config.yml]({{ site.baseurl }}/images/VSTS_view_pipeline_wizard_03.png)

Actually it was not so easy, first, I add a new demand for the agent, forcing the existence of <b>dotnet</b>, and well, it was a bad idea... :) then I had already a previous agent installed on that box, and again, it was a bad idea, so firt I need to update it, I learned the hard way, that we can update all user agents on the web inteface, forcing them to update to the latest version. It seems not fix the problem, the build was still crashing... so I had an old version of Visual Studio Comunity Edition 2017 installed, and one old one of 2017 Enterprise Previou, and as I was try to build one new .Net Core 2.1 web app, I remenber that I needed to update Visual Studio on my local machine when I instaled new .Net Core 2.1... so I tried to upgrade the msbuild tools, as the erros was related to the msbuild...

![_config.yml]({{ site.baseurl }}/images/VSTS_build_error_04.png)

It was not so fast as I expected... more than 2 hours later on the night I gave up for that day... so I let windows install some more stuff, before update msbuild tools and I went to bed...


(...)
On my Windows box, I setup a new local account (yes, I'm not running a domain) with no advanced privileges, access like a regular user on a Windows 2016 Server, with no RDP access. After the process of instal the build agent (that I downloaded from VSTS, I setup it like a service on Windows and I provided the new user account as the "Run As" user). For the security point of view, it is better to setup a service like this with a low access account, insted of LOCAL SERVICE or NETWORK SERVICE accounts.


Next you can update your site name, avatar and other options using the _config.yml file in the root of your repository (shown below).



The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.