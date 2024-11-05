# Meta-Programming Pattern

## Problem
We want to gain more flexibility when defining new classes and create custom tailored objects on the fly.

## Solution
Ruby is a very dynamic language, but that doesn't apply only to typing— we can even define new methods in our classes at runtime thanks to **singleton methods**. If instead of adding one method we want to add a group of them, we can also use the `extend` method, which would have the same effect as including a module. Last but not least, with the `class_eval` method we can evaluate a string in the context of a class, which combined with string interpolations is a really great asset to create new methods at runtime.

## Example
Going back to the [Factory Pattern](factory.md) where we created flora and fauna habitats, we might think we need more flexibility. Back then, we had multiple factories that provided us with a fixed list of combinations to create different organisms. If we wanted to have full flexibility for creating any combination we come up with while avoiding a ton of classes that support every single combination, we can make use of singleton methods:

```ruby
def new_plant(stem_type, leaf_type)
  plant = Object.new
  if stem_type == :fleshy
    def plant.stem
      'fleshy'
    end
  else
    def plant.stem
      'woody'
    end
  end
  if leaf_type == :broad
    def plant.leaf
      'broad'
    end
  else
    def plant.leaf
      'needle'
    end
  end
  plant
end

plant1 = new_plant(:fleshy, :broad)
plant2 = new_plant(:woody, :needle)
puts "Plant 1's stem: #{plant1.stem} leaf: #{plant1.leaf}"
puts "Plant 2's stem: #{plant2.stem} leaf: #{plant2.leaf}"
```

We start with a plain Ruby `Object` and tailor it adding new methods. Instead of adding them one by one, we can add a group of them:

```ruby
def new_animal(diet, awake)
  animal = Object.new
  if diet == :meat
    animal.extend(Carnivore)
  else
    animal.extend(Herbivore)
  end
  if awake == :day
    animal.extend(Diurnal)
  else
    animal.extend(Nocturnal)
  end
  animal
end
```

In this example, the `extend` method does the same thing as including a module.

Now, let's imagine that we want to group together animals and trees that share a section of the jungle and keep track of their biological classifications. Although they look like two different programming problems, they are quite similar— both of them aim to group a set of objects. Ideally, we could stablish different kinds of relationships between objects on the fly like this:

```ruby
class Tiger < CompositeBase
  member_of(:population)
  member_of(:classification)
end

class Tree < CompositeBase
  member_of(:population)
  member_of(:classification)
end

class Jungle < CompositeBase
  composite_of(:population)
end

class Species < CompositeBase
  composite_of(:classification)
end

tony_tiger = Tiger.new('tony')
se_jungle = Jungle.new('southeastern jungle tigers')
se_jungle.add_sub_population(tony_tiger)
```

REVIEW: IM NOT SURE IF THIS IS WHAT YOU MEANT
!!!!!
!!!!!
=begin
The method `composite_of(:group)` would provide a method for including members to any `:group` we could think of by adding dynamic `add_sub_:group` methods to the class instances. In the same way, the method `member_of(:group)` would provide a method `parent_:group` that could leaf nodes so that they can know what group they are member of. It happens that all this is absolutely feasible with some meta programming:
=end
!!!!!
!!!!!


```ruby
class CompositeBase
  attr_reader :name

  def initialize(name)
    @name = name
  end

  def self.member_of(composite_name)
    code = %Q{
      attr_accessor :parent_#{composite_name}
    }
    class_eval(code)
  end

  def self.composite_of(composite_name)
    member_of composite_name
    code = %Q{
      def sub_#{composite_name}s
        @sub_#{composite_name}s = [] unless @sub_#{composite_name}s
        @sub_#{composite_name}s
      end
      def add_sub_#{composite_name}(child)
        return if sub_#{composite_name}s.include?(child)
        sub_#{composite_name}s << child
        child.parent_#{composite_name} = self
      end
      def delete_sub_#{composite_name}(child)
        return unless sub_#{composite_name}s.include?(child)
        sub_#{composite_name}s.delete(child)
        child.parent_#{composite_name} = nil
      end
    }
    class_eval(code)
  end
end 
```

`CompositeBase` is the base class for the rest of the components and implements the `member_of` and `composite_of` methods. By simply passing them the name of the group, they set up all the methods we need by constructing a string that defines them. The call to `class_eval` interprets the string in the context of the class, making them available.



Certainly! `class_eval` and `instance_eval` are useful in Ruby for altering classes and instances dynamically. Here’s an example of each to illustrate their usage.

### Example of `class_eval`
`class_eval` allows us to open up a class and define or modify methods on it at runtime.

```ruby
class Person
end

# Adding methods dynamically to the Person class using class_eval
Person.class_eval do
  def greet
    "Hello! My name is #{@name}."
  end

  def name=(name)
    @name = name
  end
end

person = Person.new
person.name = "Alice"
puts person.greet # => "Hello! My name is Alice."
```

Here, `class_eval` dynamically adds `greet` and `name=` methods to the `Person` class. This is especially useful for adding methods to classes without directly editing their source code.

### Example of `instance_eval`
`instance_eval` lets you open up a specific instance of a class to define methods or access instance variables directly within its context.

```ruby
class Car
  def initialize(model)
    @model = model
  end
end

car = Car.new("Tesla")

# Using instance_eval to define a method directly on the car instance
car.instance_eval do
  def info
    "This car is a #{@model}."
  end
end

puts car.info # => "This car is a Tesla."
```

In this example, `instance_eval` is used to add an `info` method only to the `car` instance. It directly accesses the instance variable `@model`, even though it’s usually inaccessible from outside the class. 

These two methods are powerful tools in Ruby's meta-programming, allowing you to dynamically modify classes and instances in flexible and expressive ways.



Meta-programming in Ruby allows code to write code dynamically at runtime, which is powerful for creating flexible, reusable, and concise code. Here are a few examples to demonstrate some common meta-programming techniques in Ruby:

### 1. **Dynamic Method Creation with `define_method`**
   With `define_method`, you can dynamically create methods at runtime. This is useful if you need many similar methods without writing each one individually.

   ```ruby
   class Person
     # Creates getter methods for each attribute
     [:name, :age, :location].each do |attribute|
       define_method(attribute) do
         instance_variable_get("@#{attribute}")
       end

       define_method("#{attribute}=") do |value|
         instance_variable_set("@#{attribute}", value)
       end
     end
   end

   person = Person.new
   person.name = "Alice"
   person.age = 30
   person.location = "New York"

   puts person.name      # => "Alice"
   puts person.age       # => 30
   puts person.location  # => "New York"
   ```

### 2. **Using `method_missing` to Handle Undefined Methods**
   The `method_missing` method can intercept calls to undefined methods and handle them dynamically. This can be useful for delegating methods or creating flexible, dynamic attributes.

   ```ruby
   class OpenStructLike
     def initialize
       @data = {}
     end

     def method_missing(name, *args)
       attribute = name.to_s
       if attribute.end_with?("=")
         @data[attribute.chomp("=")] = args.first
       else
         @data[attribute]
       end
     end
   end

   obj = OpenStructLike.new
   obj.name = "Bob"
   obj.age = 25

   puts obj.name # => "Bob"
   puts obj.age  # => 25
   ```

   Here, `method_missing` is used to allow setting and getting arbitrary attributes on the fly.

### 3. **Accessing and Changing `self` with `class_eval` and `instance_eval`**
   `class_eval` and `instance_eval` are powerful tools for evaluating code in the context of a specific class or instance, respectively.

   ```ruby
   class DynamicGreeting
     def initialize(greeting)
       @greeting = greeting
     end
   end

   # Adding a method to an instance with instance_eval
   greeting = DynamicGreeting.new("Hello")
   greeting.instance_eval do
     def greet(name)
       "#{@greeting}, #{name}!"
     end
   end

   puts greeting.greet("Alice") # => "Hello, Alice!"
   ```

   With `instance_eval`, we added a `greet` method directly to the `greeting` object instance, which uses the instance variable `@greeting` defined in the original class.

### 4. **Defining Class-Level Methods Dynamically**
   You can dynamically define class-level methods using `singleton_class`.

   ```ruby
   class Animal
     def self.create_species(*species)
       species.each do |species_name|
         define_singleton_method(species_name) do
           "I am a #{species_name}"
         end
       end
     end
   end

   # Dynamically creating species methods
   Animal.create_species(:cat, :dog, :lion)

   puts Animal.cat  # => "I am a cat"
   puts Animal.dog  # => "I am a dog"
   puts Animal.lion # => "I am a lion"
   ```

   Here, `create_species` dynamically creates singleton methods (`cat`, `dog`, `lion`) on the `Animal` class.

### 5. **Attributes with `attr_accessor`**
   Ruby’s built-in `attr_accessor` method is a meta-programming example itself, as it dynamically creates getter and setter methods for specified instance variables.

   ```ruby
   class Book
     attr_accessor :title, :author
   end

   book = Book.new
   book.title = "1984"
   book.author = "George Orwell"

   puts book.title  # => "1984"
   puts book.author # => "George Orwell"
   ```

By using meta-programming in Ruby, you can write concise, reusable code that adapts dynamically at runtime, reducing repetition and increasing flexibility.
