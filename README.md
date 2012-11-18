#Codereading Thor

## Why was Thor created
- Who created it
- Why did they want to make thor.
- What problems does it solve etc etc

## Examples of using command line and ruby executables
Use rails as example

- *simple example:* rake routes
- *action plus param*: rails new shiny_app
- *action plus boolean options:* rails some_action --some_boolean_param
- *action plus array of options:* rails generate controller Products index show new create
- *action plus an alias:* rails g controller Posts

## Command line parsing before thor 
- Show the pain point thor solves.
- Example of a simple ruby script that accepts and parses args from command line. 
- Show how it can get complicated.

## Getting started

### How to view a gems source code.
`gem unbundle gem_name`

Downside is this is just a copy of the actual gem. Any changes you make are not reflected.

To view the gem itself ... **how do you do this?**

### Sidebar - How to inspect whats going on inside whilst on the go
#### use puts statements
simple example

#### raise "Boom"
And get a nice stack trace of every single file, method that was called upto that point
#### pry gem
2/3 sentence introduction 
Link to a screencase / homepage

####ruby-debug
2/3 sentence introduction
Link to a screencase / homepage

#### Anyone else got any nice hacks?
Please add em here.


### Sidebar - How to play with a gem's code
You definately don't want to mess with gems you are using in other projects.

You need a temporary copy of a gem.

Use rvm to create a gemset specific for codereading

```
rvm gemset create codereading
rvm gemset use codereading
gem install thor
```

To view in your favourite text editor you'll need to know the location of the gem. Rvm stores gems in the current rvm gemset directory. By default it's the *global* directory. But we just created a codereading gemset.

To find the codereading gemset directory and all it's gems

`rvm gemdir` 

where you'll find a directory "gems"

and a directory for thor







`

`




