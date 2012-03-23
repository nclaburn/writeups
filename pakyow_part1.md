#Learning the Pakyow Web Framework Part 1 

##Curator
I'm a fan of [Merlin Mann](http://www.merlinmann.com/) and his [Hipster PDA](http://www.43folders.com/2004/09/03/introducing-the-hipster-pda). The thing is, I wanted an online place to jot down notes, todos, whatever. I wanted it to be free, simple, and look like my 3x5 note cards. I wanted to "curate" these items so I decided to create [Curator](https://github.com/nclaburn/curator). I thought I would document progress for myself, and maybe help someone else out along the way.

## Introducing Pakyow
I always felt like Rails was somewhat overkill for my projects, which range from very small to medium size. The thing is, I never wanted to take the time to learn some of the other lightweight frameworks like [Sinatra](http://www.sinatrarb.com/) since I had so much time invested in Rails. Then some members of my local Ruby group  announced they were releasing a web framework. They called it [Pakyow](http://pakyow.com). I was intrigued with the description: "Pakyow is an open-source framework for building web apps in Ruby. It enables you to get straight to work by letting data be data, views be views, and code be code. Simple." After reading the Pakyow [manual](http://pakyow.com/manual) I decided to learn the framework. Here are my reasons:

1.  Pakyow is lightweight
2.  I like the idea of no Ruby code in my views. See Bryan's blog post [Keep your peanutbutter out of my chocolate](http://notmagic.org/2012/01/23/peanut_butter_chocolate). This meant I could focus on advancing my  HTML, CSS and Javascript skills and not worry about the framework.
3.  The creators are members of my local Ruby group.

## About this writeup
This writeup is meant to provide the reader with an example of how to use Pakyow to create a simple web based application. Curator leverages numerous gems in addition to the Pakyow framework. Usage and configuration of gems, will be left as an exercise for the reader.

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

#bundler
To use the gems defined in the `Gemfile`, `bundler` must be run.

    bundle install

#Database
DataMapper is used as the database layer for two reasons. First, it is lighter weight than ActiveRecord. Second, [pakyow-auth](https://github.com/metabahn/pakyow-auth), which provides authentication, currently requires DataMapper. The following sections describe how DataMapper is setup inside of Curator. Production configuration of Postgres will not be covered.

##Setup 
[SQLite](http://sqlite.org/) will be used as the development database. To configure DataMapper add the following to `config/application.rb`

    DataMapper.setup(:default, "sqlite://#{Dir.pwd}/development.sqlite")
    DataMapper::Logger.new("#{Dir.pwd}/logs/dm.log", :debug)

##Initialization
DataMapper will be initialized in the `initialize` method of the application. This will allow DataMapper to finalize the models and migrate the database when the application starts.

    def initialize
      super
      DataMapper.finalize
      DataMapper.auto_upgrade!

#Model
Curator has "cards". These cards have a `name`, some `content`, and a `date`. The card model is shown below.

    class Card
      include DataMapper::Resource
      attr_accessor :id, :title, :body, :user
  
      property :id,          Serial # auto-increment integer key
      property :name,        String 
      property :content,     Text
      property :date,    Date
      property :created_at,  DateTime

      has_tags :tags
      has 1, :user, :through => Resource
    end

##Associations
Cards have may or may not have `tags`. The `dm-tags` takes care of parsing the comma delimited tags as well as storing/retrieving them to/from the database. Each card belongs to one user. This association  is demonstrated by the `has 1, :user, :through => Resources` association. For more information on associations see the DataMapper [documentation](http://datamapper.org/docs/associations.html) on the subject.

#Views
From the Pakyow [manual](http://pakyow.com/manual#section_3): "Views in Pakyow contain no logic and simply define a structure around the data to be presented." That sentence pretty well sums it up. Views are HTML, plain and simple. This divorcing of the view from logic allows front end designers to conentrate on the presentation of the data, while back end developers implement business logic. This is the polar opposite to the way Rails [implements](http://guides.rubyonrails.org/layouts_and_rendering.html) views. I am making an assumption that most readers are familiar with the [MVC](https://en.wikipedia.org/wiki/Model-view-controller) concept. The following section describe how a view is created in Pakyow. As a counterpoint example I wanted, first, to show how the view may look in Rails.

##The Rails Way
Rails views are `erb`files, a mix of Ruby and HTML. The main view of Curator is the collection of index cards. Below is one way to implement the display of multiple cards using Rails `erb`.

    <div id='card_pane' class="droppable">
      <% @cards.each do |card| %>
        <div class='small_index_card yellow_card card_display ui-widget-content draggable'>          
          <div class='card_name'>
            <p><%= card.name %></p>
          </div>          
          <div class='card_content'>
            <p><%= card.content %></p>
          </div>          
          <div class='card_tags'>
            <p><%= card.tag_collection %></p>
          </div>          
          <div class='card_date'>
            <p><%= card.date %></p>
        </div>
      <% end %>
    </div>

The code iterates through the collection of cards creating new instances for display. Each card is displayed using the same HTML and CSS. Data, code, and HTML are mixed.


## The Pakyow Way
Below is the equivalent view code in Pakyow.

    <div id='card_pane' class="droppable">
      <div class='small_index_card yellow_card card_display ui-widget-content draggable'>          
        <div class='card_name'>
          <p  name='card[name]'></p>
        </div>          
        <div class='card_content'>
    	  <p name='card[content]'></p>
        </div>          
        <div class='card_tags'>
          <p name='card[tag_collection]'>
         </div>          
        <div class='card_date'>
          <p  name='card[date]'></p>
        </div>
    </div>
    
"But that will only display a single card, the Rails way displays all of the cards." Not so. This is where the power of the purely HTML view can be seen. In Pakyow the view only describes how the data is being displayed not how many times the data is displayed. In the case of Curator's index cards, each card is displayed the exact same way. The logic that determines the number of cards to display is pushed lower down the stack and  kept out of the view.

The `name` attributes on on the `<p>` elements is bound to the `card` model using Binders. Pakyow uses this binding information to populate the HTML view before it is displayed.



#Binder
*Question, are "Helpers" the Rails equivalent?*

Binders in Pakyow provide a way to mainuplate data for presentation. Sometimes the presentation of model data differs from how it is represented in the model. Using the [example](http://pakyow.com/manual#section_9) found in the Pakyow manual: A user's full name is needed for display. Instead of using HTML, CSS or Javascript tricks to concatenate the `first_name` and `last_name` fields, a binder should be used. The binder provides the `full_name` method, returning the combination of the first and last name. 

## Pakyow Example
    class User < Pakyow::Presenter::Binder
      binder_for :user
      def full_name
      	   bindable.first_name + '  ' + bindable.last_name
       end  
    end
    
## Curator
Curator uses AJAX to create new cards and update existing cards. Instead of using javascript to concatenate together the url to process these commands, a Binder is used. Below is a portion of the view that allows users to update a card. The `itemprop` attribute will be replaced with the appropriate attribute when the Binder processes the view.

      <form id='card_form' itemprop='card[update_card_link]' method='post'>  
         <span id='_card_fields'></span>    
         <input type="hidden" name="_method" value="put">  
      </form>

Below is the source for Curator's `CardBinder`. Three methods are defined in `CardBinder`: one for editing a card, one for linking to a card, and one for updating a card. When the `itemprop=card[update_card_link]` attribute, as shown above, is processed by the Binder, the `update_card_link` method will be called. `update_card_link` will replace `itemprop=card[update_card_link]` with with `action="/cards/3"`, where `3` is the `id` of the updated card. 
    
    class CardBinder < Pakyow::Presenter::Binder
      binder_for :card  
      def card_link    
      	   { :href => "/cards/#{bindable.id}" } 
       end  
       def edit_card_link    
          { :href => "/cards/edit/#{bindable.id}" }  
       end
       def update_card_link    
          { :action => "/cards/#{bindable.id}" }  
       end
    end 
    
#Controller

[^1]:http://pakyow.com/manual#section_1


