#Codereading Thor

## Why was Thor created
Who created it
Why did they want to make thor.
What problems does it solve etc etc

## Examples of using command line and ruby executables
Use rails as example

- *simple example:* rake routes
- *action plus param*: rails new shiny_app
- *action plus boolean options:* rails some_action --some_boolean_param
- *action plus array of options:* rails generate controller Products index show new create
- *action plus an alias:* rails g controller Posts

## Command line parsing before thor 
Show the pain point thor solves.
Example of a simple ruby script that accepts and parses args from command line. 
Show how it can get complicated.

## Getting started

### How to view a gems source code.
gem unbundle gem_name

downside is this is just a copy of the actual gem. Any changes you make are not reflected.

to view the gem itself 

**how do you do this?**

### How to inspect whats going on inside whilst on the go
puts statements
require
pry
ruby-debug

### How to use these little tools with gems
Dont want to mess with gems you are using in other projects.
Use rvm to create a gemset specific for codereading

`rvm gemste create codereading`

get thor
`gem install thor`

to view in your editor

`

`




