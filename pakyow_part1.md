#Learning the Pakyow Web Framework Part 1 

I always felt like Rails was somewhat overkill for my projects, which range from very small to medium size. The thing is, I never wanted to take the time to learn some of the other lightweight frameworks like Sinatra since I had so much time invested in Rails. Then some members of my local Ruby announced they were releasing a web framework. They called it [Pakyow](http://pakyow.com). I was intrigued with the description: "Pakyow is an open-source framework for building web apps in Ruby. It enables you to get straight to work by letting data be data, views be views, and code be code. Simple." After reading the Pakyow [manual](http://pakyow.com/manual) I decided to learn the framework. Here are my reasons:

1.  Pakyow is lightweight
2.  I like the idea of no Ruby code in my views. See Bryan's blog post [Keep your peanutbutter out of my chocolate](http://notmagic.org/2012/01/23/peanut_butter_chocolate)
3.  The developers are members of my local Ruby group. 

I wanted to document my experience of creating an application using Pakyow in the hopes of helping others get started with the framework. 

##Documentation

Pakyow's [manual](http://pakyow.com/manual) describes the steps necessary to get Pakyow installed, how a Pakyow application is structured, and finally API details of the framework. What is noticably missing is a getting started walkthrough with accompanying program. Hopefully, this series of posts will fill in the gaps.

##Getting started
### Curator

Pakyow is distributed as a gem, therefore installation is trivial. Creating a new project is trivial as well.