#Codereading Thor

## Why was Thor created
- Who created it
- Why did they want to make thor.
- What problems does it solve etc etc

## Examples of using command line and ruby executables
Use rails as example

- **simple example:** rake routes
- **action plus param**: rails new shiny_app
- **action plus boolean options:** rails some_action --some_boolean_param
- **action plus array of options:** rails generate controller Products index show new create
- **action plus an alias:** rails g controller Posts

## Command line parsing before thor 
- Show the pain point thor solves.
- Example of a simple ruby script that accepts and parses args from command line. 
- Show how it can get complicated.

## Getting started

Read the useage on [thor's wiki](https://github.com/wycats/thor/wiki "Thor's wiki").

Try creating some trivial examples using parameters and opiton types (:boolean, :string etc)

###Crack open the hood.

Where to start?

Easy way is to just follow the code as it is run. Decide on a trivial thor app, and then insert a raise to see the stacktrace of all the methods called upto that point. 

To do this...

1 Create a trivial thor file

In a directory of your choice create this file

```
#example.thor
class App < Thor
  desc "boom", "explodes stuff"
  def boom
    raise "boom" #heres the raise to give us a stacktrace
  end
end
```	

2 Run

`thor app:boom`

You should get a stacktrace roughly similiar to this 

```
/Users/adam/Code/Temporary/thor_codereading/thor.thor:4:in `boom': boom (RuntimeError)
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/lib/thor/task.rb:27:in `run'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/lib/thor/invocation.rb:120:in `invoke_task'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/lib/thor.rb:275:in `dispatch'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/lib/thor/base.rb:425:in `start'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/lib/thor/runner.rb:36:in `method_missing'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/lib/thor/task.rb:29:in `run'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/lib/thor/task.rb:126:in `run'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/lib/thor/invocation.rb:120:in `invoke_task'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/lib/thor.rb:275:in `dispatch'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/lib/thor/base.rb:425:in `start'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/bin/thor:6:in `<top (required)>'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/bin/thor:19:in `load'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/bin/thor:19:in `<main>'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/bin/ruby_noexec_wrapper:14:in `eval'
	from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/bin/ruby_noexec_wrapper:14:in `<main>'
```
The stacktrace is ordered with the last method called being first. i.e.
The top line is the last method called - the method with our raise call.
The bottom line is the birthpoint of the program.

Look for the first lines mentioning the thor codebase

These two lines are the first to do so

```
from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/bin/thor:19:in `load'
from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/bin/thor:19:in `<main>'
```


but on inspection of /bin/thor line 19 don't exist - **Does anyone know why this happen???**

anyway just skip them and move up the stacktrace untill you find something relevant. Luckily

```
from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/bin/thor:6:in `<top (required)>'
```

show us the entry point. Not surprisingly it's the exec file at thor/bin/thor 

```ruby
#thor/bin/thor
#!/usr/bin/env ruby
# -*- mode: ruby -*-

require 'thor/runner'
$thor_runner = true
Thor::Runner.start
```

this is run whenever you run `thor blahblahblah` on the command line.

Thor::Runner.start is easy to find in your editor, just look at the stacktrace, it tells us it's at

from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/lib/thor.rb:275:in 'dispatch'
from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/ **lib/thor/base.rb:425** :in 'start'
from /Users/adam/.rvm/gems/ruby-1.9.3-p194-Ruby@codereading/gems/thor-0.16.0/bin/thor:6:in '<top (required)>'


```ruby
#lib/thor/base.rb:425

      # Parses the task and options from the given args, instantiate the class
      # and invoke the task. This method is used when the arguments must be parsed
      # from an array. If you are inside Ruby and want to use a Thor class, you
      # can simply initialize it:
      #
      #   script = MyScript.new(args, options, config)
      #   script.invoke(:task, first_arg, second_arg, third_arg)
      #
      def start(given_args=ARGV, config={})
        config[:shell] ||= Thor::Base.shell.new
        dispatch(nil, given_args.dup, nil, config)
      rescue Thor::Error => e
        ENV["THOR_DEBUG"] == "1" ? (raise e) : config[:shell].error(e.message)
        exit(1) if exit_on_failure?
      rescue Errno::EPIPE
        # This happens if a thor task is piped to something like `head`,
        # which closes the pipe when it's done reading. This will also
        # mean that if the pipe is closed, further unnecessary
        # computation will not occur.
        exit(0)
      end
```


##Sidebars 
Explain topics in more detail that are mentioned in the explanation but which are a little off topic 

### Sidebar - How to inspect whats going on inside a gem 
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


### Sidebar - How to hack away at a gem safely
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

which will tell you the directory of the current gemset (i.e. codereading )

where you'll find a directory "gems"

and a directory below it for thor 

### Sidebar - Rubygems, Paths and command line scripts.
Nice to have a sidebar explaining how typing thor (or any other ruby command) on the command line is piped to the gems code

**WARNING - inacurate info to proceed - please edit**

Rubygems, the system that manages your gems, places a gems file in your system's path. So when you type thor, rubygems looks for a file that can respond to such a call. If it doesnt find anything you'll get an error. But since thor is installed this file will get called. 










`

`




