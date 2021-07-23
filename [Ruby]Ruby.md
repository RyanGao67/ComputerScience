# The nil value

The nil value is used to express the notion of a "lack of an object". 

It is also used by Garbage Collector ---- which implements a mark-and-sweep algorithm ---- to decide if an object should be destroyed or not. 

Unlike other languages, the nil is not used to express emptiness, false or zero. 

As everything in Ruby is object, the nil value refers to the non-instantiable NilClass class. 


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


