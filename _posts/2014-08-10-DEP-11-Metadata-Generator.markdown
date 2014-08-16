---
layout: post
title:  "DEP-11/AppStream for Debian Archive"
date:   2014-08-15 14:34:10
author: Abhishek Bhattacharjee
categories: jekyll update
---
To begin with, here's something Google Summer of Code. The above project is a part of Google Summer of Code '14
which is a well known program for promoting free and open source softwares among students. The project is being
supervised by Matthias Klumpp who is a Debian Developer([DD](https://wiki.debian.org/DebianDeveloper))

I'll try to cover everything under a single post about AppStream and what this project aims to achieve in layman's terms.

###AppStream###
AppStream is a cross-distro specification for software metadata, as well as libraries reading the data.
The metadata of an archive is kept in the ftp servers, where the packages reside too.
And these metadata is kept in a format that is readable by AppStream.
AppStream allows **Software Center**-like applications like Ubuntu Software Center or Apper to talk to these servers
and provide informations to the users.
AppStream makes cross-distribution possible as it specifies a standard for the metadata and also provide libraries which
can be used to by any Software center clients. More about AppStream can be found [here](http://www.freedesktop.org/software/appstream/docs/)

###Debian Enhancement Proposal(DEP) 11 ###
DEP-11 is a draft made to support AppStream for Debian. Debian currently doesn't support AppStream.
DEP-11 proposes the metadata to be kept in [YAML](http://en.wikipedia.org/wiki/YAML) format and not in xml which
is the format specified by AppStream. 
So to realize this, we needed to write a generator that generates data in accordance with DEP-11 draft.
This was the main aim of the project. More about the DEP-11 draft could be found [here](https://wiki.debian.org/DEP-11).
More about the generator later, which is the product of the project. But will see what we achieve from the project.

####What we gain ?####
As a Debian user, we gain a lot from this project. The project makes it possible to have "Software Center" like applications
for Debian which has been missing. The most important aspect in realizing the above goal(i.e having a application manager) is 
the availability of data which is to be used by such softwares.
I hope this will help broaden the user-base of Debian, since hassle free package-installation can lead to increased
adoption.

####DEP-11 metadata generator####
This is what I have worked on as my Google Summer of Code project. I have made the generator which reads two main 
sources of metadata **.desktop** files and **appdata xml** files, and converts them into YAML format.
The generator will be a part of **Debian Archive Kit** which is used to manage debian archives in the ftp servers.
The generator also takes care of removing the stale cache, so that the servers are always up-to-date with the latest metadata.
It creates a tarball of icons of all the deb packages and also makes a cache of the screenshots, these can be used by
the AppStream Library to give users valuable informartion about a package.

**Further changes** are on the line and I'll keep posting!

###Some other important links###
* [Ximion's blog](http://blog.tenstral.net/)
* [Hughsie's AppStream repo](https://github.com/hughsie/fedora-appstream)
* [DAK's source and more](https://ftp-master.debian.org/#dak)

###Thanks for reading :-)###
