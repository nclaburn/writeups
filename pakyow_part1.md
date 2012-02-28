#Learning the Pakyow Web Framework Part 1 

##Curator
I'm a fan of [Merlin Mann](http://www.merlinmann.com/) and his [Hipster PDA](http://www.43folders.com/2004/09/03/introducing-the-hipster-pda). The thing is, I wanted an online place to jot down notes, todos, whatever. I wanted it to be free, simple, and look like my 3x5 note cards. I wanted to "curate" these items so I decided to create [Curator](https://github.com/nclaburn/curator). I thought I would document progress for myself, and maybe help someone else out along the way.

## Introducing Pakyow
I always felt like Rails was somewhat overkill for my projects, which range from very small to medium size. The thing is, I never wanted to take the time to learn some of the other lightweight frameworks like [Sinatra](http://www.sinatrarb.com/) since I had so much time invested in Rails. Then some members of my local Ruby group  announced they were releasing a web framework. They called it [Pakyow](http://pakyow.com). I was intrigued with the description: "Pakyow is an open-source framework for building web apps in Ruby. It enables you to get straight to work by letting data be data, views be views, and code be code. Simple." After reading the Pakyow [manual](http://pakyow.com/manual) I decided to learn the framework. Here are my reasons:

1.  Pakyow is lightweight
2.  I like the idea of no Ruby code in my views. See Bryan's blog post [Keep your peanutbutter out of my chocolate](http://notmagic.org/2012/01/23/peanut_butter_chocolate). This meant I could focus on advancing my  HTML, CSS and Javascript skills and not worry about the framework.
3.  The creators are members of my local Ruby group. 

##Documentation

Pakyow's [manual](http://pakyow.com/manual) describes the steps necessary to get Pakyow installed, how a Pakyow application is structured, and finally API details of the framework. What is noticably missing is a getting started walkthrough with accompanying program. Hopefully, this series of posts will fill in the gaps.

#Getting Started
##Install
First, install Pakyow[^1]. This is easily accomplished since Pakyow is a gem.

<code>gem install pakyow</code>

##Create the project
In typical Ruby style, this is easy.

<code>pakyow new curator</code>

At this piont you have a working Pakyow application. Type <code>pakyow server</code>  to start the application.

#Gemfile


[^1]:http://pakyow.com/manual#section_1


