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
Using [Bundler](http://gembundler.com/) is not required for Pakyow, but it does help with gem versioning and deployment. The `Gemfile` has been broken out into 3 sections, which are described below.

##Global
 *Note that pakyow was installed in the global gemset in the pervious section. It is includedin the `Gemfile` for deployment purposes. Also, specific versions of gems can be "locked"  if needed, ensuring that deployment is consistent.*
 
Since this is a pakyow application it should be included in the `Gemfile`.  [pakyow-auth](https://github.com/metabahn/pakyow-auth) is the authentication module (more on that later). Because `pakyow-auth` has yet to be releasd as a gem, it is installed directly from the github repository. [rake](http://rake.rubyforge.org/) and [rack](http://rack.rubyforge.org/) are familiar gemsets and will not be covered here. [data_mapper](http://rubydoc.info/gems/datamapper/1.2.0/frames) is a meta gem that includes the dependencies for the [DataMapper](http://datamapper.org/) ORM. [dm-tags](http://dm-tags.rubyforge.org/) is a gem that integrates with DataMapper to provide model tagging capability.

    gem 'data_mapper'
    gem 'dm-tags'
    gem 'pakyow-auth' , :git => "git://github.com/metabahn/pakyow-auth.git"
    gem 'pakyow'
    gem 'rack'
    gem 'rake'
    


##Production
Gems in this section are only installed when pakyow is deployed in `production` mode. [dm-postgres-adapter](https://github.com/datamapper/dm-postgres-adapter) binds DataMapper to the  Postgresql databse. [pg](https://bitbucket.org/ged/ruby-pg/wiki/Home) is the ruby wrapper used to communicate with the production [PostgreSQL](http://www.postgresql.org/) database.

    group :production do
      gem 'dm-postgres-adapter'
      gem 'pg'
    end


##Development
Development gems include those required for debugging our application. Additionally, [dm-sqlite-adapter](https://github.com/datamapper/dm-sqlite-adapter) is included to provide a binding between DataMapper and the SQLite(http://sqlite.org/) database. Finally, during development the [thin](http://code.macournoyer.com/thin/) webserver is used. The use of "thin" is a personal preference and not a Pakyow requirement.

    group :development do
      gem 'linecache19'
      gem 'ruby-debug-base19x'
      gem 'ruby-debug19'
      gem 'dm-sqlite-adapter'
      gem 'thin'
    end


[^1]:http://pakyow.com/manual#section_1


