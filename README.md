# Wepic : WebdamLog Picture Manager

## Abstract from paper

We present the WebdamLog system for managing distributed
data on the Web in a peer to peer manner. We demonstrate
the main features of the system through an application
called Wepic for sharing pictures between attendees
of the sigmod conference. Using Wepic, the attendees will
be able to share, download, rate and annotate pictures in
a highly decentralized manner. We show how WebdamLog
handles heterogeneity of the devices and services used to
share data in such a Web setting. We exhibit the simple
rules that code the Wepic application and show how to easily
modify the Wepic program.

## Structure of the Wepic Application

The Wepic application will be seen from the end user as a website. The website itself will be made mobile friendly to accomodate the attendees who wish to access the website with their smartphone.
When the user creates a new account from a mobile device, this will start a WebdamLog instance (or Wepic instance) somewhere on the Webdam Cloud. This instance is what the user will use to upload,
download and manage pictures. 

The user first goes on the Wepic home page and enters his name. If he has never logged in before, a new account will be created for him, and he will be redirected to his personal home page. In the background,
a new wepic peer is created (with its own server). The redirection only happens once the server is ready. Once on his server, the user is prompted to setup his password, and this will reserve the Wepic instance for him. Once the password is setup, the user will be able to start using the application.

If the user has already logged in before, he would be redirected to his personal wepic peer (whose server is already running). He is then prompted for the password he has already setup. Note that identification is done by name at first, not by IP. Therefore, if a user enters his name and the name is already taken, he would be redirected to an existing account home page where he needs to log in. Of course, this would not be his account. Therefore, there should be an option on the Wepic homepage to specify that it is actually a new account that needs to be created. If the user manages to accidentally arrive on the home page of someone else, it has to be clear that this peer was already setup and that the user needs to setup a new one. A back button should be provided.

### What can the user do once logged in ?

The Wepic peer application contains three tabs : the application tab, the query tab and the program tab :

#### The Application Tab
The application tab is the first tab the user will see once he logs in. On this tab, the user has access to the 'traditional' functionalities of a picture manager :

- upload pictures.
- download pictures.
- see pictures from his contacts in the conference.
- send pictures by mail or FB.
- sort pictures
- rate pictures
- leave comments on pictures

These functionalities are using WebdamLog rules under the covers. The aim of the two other tabs is to show the WebdamLog 'under the covers' aspect of the application.

#### The Query Tab

The query tab lets the user write WebdamLog queries and the see the result of those queries.
First, a few examples of different queries an updates are suggested.  They are given with the rule syntax and a corresponding description in English.
Then, the user is given a text bar where he can specify a rule in WebdamLog. A GUI is given to help the user follow the WebdamLog syntax.
The GUI initially looks like this, [head_relation],[-new_rule],[submit] and [-add_block] are buttons : 

[head_relation] :- [- add_block]
[-new_rule]
[submit]

When the user clicks on the [head_relation] button, a text bar appears in place of the head relation with three slots [slot1]@[slot2]([slot3]) and description of what should be put in each
slot :
- Slot1 should contain a relation name.
- Slot2 should contain a peer name.
- Slot3 should contain attributes for the relation, seperated by commas. 

The server should be able to verify that the syntax is correct before sending the rule to the WebdamLog engine. The same interface can be used for queries and updates.

#### The Program Tab

The program tab lets the user see rules of his program and add new rules. The interface to add new rules is similar to that of the query tab. To make rule-writing easier, the user also sees local and known non-local relations on the right corner section of the app. These relations are sorted into two categories, sorted relations and derived relations. The user can also select one of these relations, and its contents and schema will be shown below.

### How it is implemented

Each webdamlog peer needs an address (host_ip and port). Therefore, if several peers are to be stored on the same host, different servers running on different ports are required. This is why Wepic does not only have one server, but has one server per peer (plus a server for the 'Manager').

#### The Wepic Manager
The Wepic Manager is responsible for spawning new peers (along with their own server and database). When the user first logs in, it is on the Manager peer that he first lands.  The manager peer also has an admin section. This admin section lets the administrator start/stop peer servers and create/destroy databases. When the Wepic manager is launched, a ''reset'' option can be used. This ''reset'' switch will stop all peer servers and destroy all peer databases. It is a way to clean the Wepic setup.

#### The Wepic Peer
The Wepic Peer is where the user application described above resides. Each peer has his own server, database and WebdamLog instance. Peers are specific to a user and spawned by the manager.

#### How to start a new Wepic instance (Peer or Manager)
Wepic uses Ruby and Rails as the main website technology. Creating a new Wepic instance corresponds to starting a rails server, but additional switches are available. These switches are 

-h : displays the help of the wepic-specific switches.
-p : specifies on which port the server should be launched.
-u : specify the user name of the instance. If none specified or if the user name given is "manager", a manager instance will be started. Otherwise, a peer instance is started using the given username.
reset : the reset switch (manager-specific) that has been discussed earlier.
-db : specify a database id, which will be used to name the database specific for that user. If none specified, it will be the same as the username by default.

Example of how a peer is launched by the manager :

  rails s -u julestestard -p 10000

#### Facebook & Mail Wrapper
Still needs to be discussed further.

## Project requirements

- Git
- Ruby (1.8.7) - Required gems in Gemfile : need to run bundle install
- RMagick (required for paperclip, seperate from gem) - dependent libs
- MySQL - required libs


## Setup on Cendrillon

Installed
- Yum package downloader installed using urpmi.
- Make installed ( - Python, required for Make).
- Git installed.
- Imagemagick installed with support for PNG, WMF, JPEG, TIFF, GIF.


Remaining
- git repo clone webdamsystem.git(getting permission denied exception (need to take care of ssh credentials).
- install MySql
- install all gems

## Database Setup
- Setting up the database setup of mysql. You can find more information about how to setup mysql [http://cicolink.blogspot.fr/2011/06/how-to-install-ruby-on-rails-3-with.html|here].
- You then need to call bundle exec from within the wepic application. Given the mysql gem is given in the Gemfile, this should install it.
- You need to setup a mysql user for the wepic application (if you don't already have one):
  $mysql
  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 913
  Server version: 5.5.28-0ubuntu0.12.04.2 (Ubuntu)
  
  Copyright (c) 2000, 2012, Oracle and/or its affiliates. All rights reserved.
  
  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.
  
  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.  
  
  mysql> CREATE USER your_user IDENTIFIED BY your_password;

- Now You need to config your database.yml file. Create or access to the '${Rails.root}/config/database.yml' file. Make sure the configs are :

  development:
    adapter: mysql2
    encoding: utf8
    reconnect: false
    database: development_MANAGER
    pool: 5
    username: your user
    password: your password
    socket: /var/run/mysqld/mysqld.sock
    timeout : 5000
  
  -  Warning: The database defined as "test" will be erased and
  -  re-generated from your development database when you run "rake".
  -  Do not set this db to the same as development or production.
  test:
    adapter: mysql2
    encoding: utf8
    reconnect: false
    database: test_MANAGER
    pool: 5
    username: your user
    password: your password
    socket: /var/run/mysqld/mysqld.sock
    timeout : 5000
  
  production:
    adapter: mysql2
    encoding: utf8
    reconnect: false
    database: production_MANAGER
    pool: 5
    username: your user
    password: your password
    socket: /var/run/mysqld/mysqld.sock

## Database API

Wepic requires a very dynamic database environment. Database are created, destroyed and tables are inserted or dropped at run-time. As a result, the database API gives option to create/destroy databases and tables. To make this task easier, the Rails ''ActiveRecord'' library is taken advantage of and the ActiveRecord design pattern is implemented. Whenever a new database is created, a corresponding class is made available. This class contains the ''relation_classes'' hashmap. Each value of ''relation_classes'' is an ActiveRecord class corresponding to a table in the database. These classes are meant to be directly mapped to the equivalent WebdamLog relations. 

- The create_or_connect_db method is used when the server is started and creates the database (if it does not already exist). It also creates the initial database class with a single relation called WL_Schema. This relation records the schemas of the relations within the database. This information could theoretically also be obtained from the database itself using mysql-specific methods, but requires extra parsing work and isn't flexible. Using a table to store schemas also offers more freedom to specify additional metadata about the created tables, and the storage spaced use is not excessive.
- Relations are created using the create_relation method of the database class. This will insert a table in the database and create the corresponding active_record class.
- Relations can be destroyed using the destroy method of the database class. Databases can be destroyed using the destroy method of the Database module.
- Relation classes implement all of the methods of the Rails Active_Record classes. The test files ''test/unit/database_test.rb'' and ''test/unit/multiple_database_test.rb'' show more about how these tests are implemented.

Notes :
- Images are stored in the database as well, but use several tuples. In fact, one 'image' field in a WLSchema corresponds to six fields in the database : 
-- image_file_name : name for the file
-- image_content_type : type for the file (JPEG, TIFF, PNG...)
-- image_file_size : size in bytes of the file
-- image_file : binary content for the original image
-- image_small_file : binary content for a 300x300 pixel version of the picture
-- image_thumb_file : binary content for a 150x150 pixel version of the picture
- The database name for the manager will always be ${rails_env}_MANAGER. All other database names are written ${rails_env}_${db_id}. The database is created upon start up. No migrations are necessary. All databases and their tables are created upon rails startup (if not already existent). Connection to these databases is also defined at  launch, and only once.

## TODO
- Facebook wrapper
- Mail wrapper
