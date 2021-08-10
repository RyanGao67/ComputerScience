# Rails 
[https://github.com/RyanGao67/First_Rails_20210809](https://github.com/RyanGao67/First_Rails_20210809)

# Install Ruby

**ebenv cheat sheet**
[https://devhints.io/rbenv](https://devhints.io/rbenv)

```
sudo apt update    
sudo apt install git curl autoconf bison build-essential \
    libssl-dev libyaml-dev libreadline6-dev zlib1g-dev \
    libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev
    
    
sudo apt install rbenv

rbenv init # follow the instruction you see after hit enter

echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile   
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile

Then, rather than running $ ~/.rbenv/bin/rbenv init, I ran $ source ~/.bash_profile. I was then able to install ruby-build:

$ git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build

After installing Ruby ($ rbenv install 2.7.0), remember to set it as the default version ($ rbenv global 2.7.0 in my case).

NOTE: replace .bash_profile with .bashrc if using Ubuntu.

curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-doctor | bash

```

On Mac if `rbenv global *.*.*` does not change the version: 

Check that PATH contains `$HOME/.rbenv/shims` and `$HOME/.rbenv/bin`

`$ env | grep PATH`      
Also check that you have the following in your ~/.bash_profile if using bash or ~/.zshenv if using zsh

```
export PATH="$HOME/.rbenv/bin:$PATH"       
eval "$(rbenv init -)"        
```
NOTE: Make sure it's the last setting in your ~/.bash_profile . I ran into an issue where I installed a program that updated my .bash_profile and reset PATH.

Finally, make sure your $HOME folder doesn't have a .ruby-version file that you may have created by accident if you were to have done $ rbenv local <ruby-version> in your $HOME folder. Doing $ rbenv global <ruby-version> modifies the $HOME/.rbenv/version file, and the existence of a .ruby-version file in the $HOME folder would override the version set by $HOME/.rbenv/version.

From the docs:

Choosing the Ruby Version When you execute a shim, rbenv determines which Ruby version to use by reading it from the following sources, in this order:

The RBENV_VERSION environment variable, if specified. You can use the rbenv shell command to set this environment variable in your current shell session.

The first .ruby-version file found by searching the directory of the script you are executing and each of its parent directories until reaching the root of your filesystem.

The first .ruby-version file found by searching the current working directory and each of its parent directories until reaching the root of your filesystem. You can modify the .ruby-version file in the current working directory with the rbenv local command.

The global ~/.rbenv/version file. You can modify this file using the rbenv global command. If the global version file is not present, rbenv assumes you want to use the "system" Ruby—i.e. whatever version would be run if rbenv weren't in your path.

# The nil value

The nil value is used to express the notion of a "lack of an object". 

It is also used by Garbage Collector ---- which implements a mark-and-sweep algorithm ---- to decide if an object should be destroyed or not. 

Unlike other languages, the nil is not used to express emptiness, false or zero. 

As everything in Ruby is object, the nil value refers to the non-instantiable NilClass class. 

```ruby
irb> "I'm an object".nil?
 => false
irb> a = nil
 => nil
irb> a.nil?
 => true
```
# OOP
Ruby is an object-oriented programming language(OOP) that uses classes as blueprints for objects. Everything is Ruby is object, and object has two main properties: states and behaviors. 

Ruby classes are the blueprints that establish what attributes and behaviours that an object should have. 

attributes also known as states

behaviors also known as methods


# attr_accessor, attr_writer, attr_reader
```ruby
class Food
  def initialize(protein)
    @protein = protein
  end
end

bacon = Food.new(21)
bacon.protein

# NoMethodError: undefined method `protein'

```

To address this problem, you can do the following: 

```ruby
class Food 
  def protein
    @protein
  end
end

bacon.protein
# 21
```

The above is the getter function. Setter function is similar: 
```ruby
class Food
  def protein=(value)
    @protein = value
  end
end
bacon.protein = 25
```


Now:Is there a better way to define this kind of method?
```ruby
class Food
 attr_accessor :protein
 def initialize(protein)
   @protein = protein
 end
end
```

For this example, it creates:

protein     
protein=     
These are the same methods we created before…     

But now you don’t have to type them out. It’s a shortcut!

Three of them to be exact:

attr_accessor   
attr_reader    
attr_writer    
What are the differences between them?

attr_accessor creates both the READER & WRITER methods.

attr_reader creates only the reader.      
attr_writer creates only the writer.

In other words:

With attr_reader you can only read the value, but not change it. With attr_writer you can only change a value but not read it.

```ruby
attr_reader :name, :value, :ready?
```
Multiple instance variables

```ruby
Those methods let you set instance vars in a more indirect way: imagine you have a class Person

class Person < ActiveRecord::Base
  attr_accessible :first_name, :last_name

  def full_name
    [@first_name, @last_name].join
  end

  def full_name=(name)
    @first_name, @last_name = name.split(" ")
  end
end
Then you can do something like this:

p = Person.new
p.full_name = "John Doe"
p.first_name # => "John"
p.last_name # => "Doe"
p.full_name # => "John Doe"
```

If you are wandering why full_name= method is like this, it is just a syntactic sugar. 



# Class and Instance Variables in Ruby
Used declare variables within a class. There are two main types: class variables, which have the same value across all class instances (i.e. static variables), and instance variables, which have different values for each object instance.

```
Syntax
$variableName #global scope
@@classVariable #class scope, static across all class instances
@instanceVariable #class object scope, value specific to object instance

#class variable getter, called on the class itself (hence self keyword)
def self.getClassVar
    @@classVariable
end

#Ruby has shortcuts for instance variable getters and setters

#both getter and setters
attr_accessor :instanceVariable
#getter
attr_reader :instanceVariable
#setter
attr_writer :instanceVariable
```
Notes
Class and instance variables are private , so getters and setters are necessary to access the variables outside the class.

Instance variables are not inherited.


Example
```ruby
class Car
    @make
    @@wheels = 4

    def initialize(make)
        @make = make
    end

    def self.wheels
        @@wheels
    end
    attr_accessor :make
end

#outside the class
civic = Car.new("Honda")

#instance variable, called on the object
>>civic.make
=>"Honda"
#class variable, called on the class itself
>>Car.wheels
=>4
```



# When not use parenthesis in ruby function calling

Ruby allows you to leave out parenthesis, in general, resist this temptation.    
Parenthesis make the code easier to follow. General Ruby style is to use them, except in the following cases:

* Always leave out empty parentheses
* The parentheses can be left out of a single command that is surrounded by ERb delimiters -- the ERb markers make sure the code is still readable
* A line that is a single command and a single simple argument can be written without the parenthesis. Personally, I find that I do this less and less, but it's still perfectly readable. I tend not to like single lines in regular ruby code that have multiple arguments and no parentheses.
* A lot of Ruby-based Domain Specific Languages (such as Rake) don't use parenthesis to preserve a more natural language feel to their statements.


# Mixin
Ruby is a programming language that only allows single inheritance. Fortunately Ruby provides this functionality as Mixins. In today's tutorial we are going to be looking at using Mixins, why you would use them and when you should use them over classical inheritance. 

A mixin is basically just a Module that is included into a class, when you mixin a Module into a class, the class will have access to the methods of the Module. 

The methods of a Module that are mixed into a class can either be Class methods or Instance methods. 

###Using Modules methods as Instance Methods

To add Module methods as instance methods on a Class you should include the Mixin as part of the Class.

For example, imagine you had this incredibly useful Module:

```ruby
module Greetings  
def hello  
puts "Hello!"  
end

def bonjour  
puts "Bonjour!"  
end

def hola  
puts "Hola!"  
end  
end  
```

To add these methods as instance methods on a class, you would simply do this:

```ruby
class User  
include Greetings  
end  
Now you will have access to the methods on any instance of that Class:

philip = User.new  
philip.hola  
=> Hola!
```
But if you try to call the methods as Class Methods, you will get an error:

User.hola  
=> undefined method ‘hola’ for User:Class (NoMethodError) 



### Using Modules methods as Class Methods
To add Module methods as Class Methods, instead of using include you would use extend:

```ruby
class User  
extend Greetings  
end  
Now you can call the methods on the Class:

User.hola!  
=> Hola!  
```
But not on an instance of the Class:
```ruby
philip = User.new  
philip.hola  
=> undefined method ‘hola’ for #<User:0x007fbd5b9ae438> (NoMethodError)  
```

### class variable

The @@variables aren't class variables. They are class hierarchy variables, i.e. they are shared between the entire class hierarchy, including all subclasses and all instances of all subclasses. (It has been suggested that one should think of @@variables more like $$variables, because they actually have more in common with $globals than with @ivars. That way lies less confusion. Others have gone further and suggest that they should simply be removed from the language.)

Ruby doesn't have class variables in the sense that, say, Java (where they are called static fields) has them. It doesn't need class variables, because classes are also objects, and so they can have instance variables just like any other object. All you have to do is to remove the extraneous @s. (And you will have to provide an accessor method for the class instance variable.)

```ruby
class A
  def self.init config
    @config = config
  end

  def self.config # This is needed for access from outside
    @config
  end

  def config
    self.class.config # this calls the above accessor on self's class
  end
end
```
Let's simplify this a bit, since A.config is clearly just an attribute_reader:

```ruby
class A
  class << self
    def init config
      @config = config
    end

    attr_reader :config
  end

  def config
    self.class.config
  end
end
```
And, in fact, A.init is just a writer with a funny name, so let's rename it to A.config= and make it a writer, which in turn means that our pair of methods is now just an accessor pair. (Since we changed the API, the test code has to change as well, obviously.)

```ruby
class A
  class << self
    attr_accessor :config
  end

  def config
    self.class.config
  end
end

class B < A; end
class C < A; end

B.config = "bar"
p B.new.config  # => "bar"
p C.new.config  # => nil

C.config = "foo"
p B.new.config  # => "bar"
p C.new.config  # => "foo"
```
However, I can't shake the feeling that there is something more fundamentally iffy about the design, if you need this at all.


### self 

What is self?
You may have heard people say that everything in Ruby is an object. If that's true it means that every piece of code you write "belongs" to some object.

self is a special variable that points to the object that "owns" the currently executing code. Ruby uses self everwhere:

For instance variables: @myvar
For method and constant lookup
When defining methods, classes and modules.
In theory, self is pretty obvious. 

Inside of an instance method
In the code below, reflect is an instance method. It belongs to the object we created via Ghost.new. So self points to that object.

```ruby
class Ghost
  def reflect
    self
  end
end

g = Ghost.new
g.reflect == g # => true
```


Inside of a class method
For this example, reflect is a class method of Ghost. With class methods, the class itself "owns" the method. self points to the class.

```ruby
class Ghost
  def self.reflect
    self
  end
end

Ghost.reflect == Ghost # => true
```

It works the same with "class" methods inside of modules. For example:

```ruby
module Ghost
  def self.reflect
    self
  end
end 
Ghost.reflect == Ghost # => true
```

Remember, classes and modules are treated as objects in Ruby. So this behavior isn't that different from the instance method behavior we saw in the first example.


Inside of a class or module definition
One feature of Ruby that makes it such a good fit for frameworks like Rails is that you can execute arbitrary code inside class and module definitions. When you put code inside of a class/module definition, it runs just like any other Ruby code. The only real difference is the value of self.

As you can see below, self points to the class or module that's in the process of being defined.

```ruby
class Ghost
  self == Ghost # => true
end 

module Mummy
  self == Mummy # => true
end 
```


Instance methods
Even though the reflect method was defined in the module, its self is the instance of the class it was mixed into.

```ruby
module Reflection
  def reflect
    self
  end
end 

class Ghost
  include Reflection
end

g = Ghost.new
g.reflect == g # => true
```

Class methods
When we extend a class to mix in class methods, self behaves exactly like it does in normal class methods.

```ruby
module Reflection
  def reflect
    self
  end
end 

class Ghost
  extend Reflection
end

Ghost.reflect == Ghost # => true
```

Inside the metaclass
Chances are you've seen this popular shortcut for defining lots of class methods at once.

```ruby
class Ghost
  class << self 
    def method1
    end

    def method2
    end
  end
end
```
The class << foo syntax is actually pretty interesting. It lets you access an object's metaclass - which is also called the "singleton class" or "eigenclass." I plan on covering metaclasses more deeply in a future post. But for now, you just need to know that the metaclass is where Ruby stores methods that are unique to a specific object.

If you access self from inside the class << foo block, you get the metaclass.

```ruby
class << "test"
  puts self.inspect
end


# => #<Class:#<String:0x007f8de283bd88>
```
Outside of any class
If you're running code outside of any class, Ruby still provides self. It points to "main", which is an instance of Object:

puts self.inspect # => main
