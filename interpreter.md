# Interpreter Pattern

The Interpreter Pattern is a behavioral design pattern that is used to define a grammar for a language and provides an interpreter to interpret sentences in that language. 


```ruby

# Terminal Expression
class Number
  def initialize(value)
    @value = value
  end

  def interpret
    @value
  end
end

# Non-Terminal Expression
class Add
  def initialize(left, right)
    @left = left
    @right = right
  end

  def interpret
    @left.interpret + @right.interpret
  end
end

# Create the Context:

class Context
  def initialize
    @variables = {}
  end

  def set_variable(name, value)
    @variables[name] = value
  end

  def get_variable(name)
    @variables[name]
  end
end
# Define the Parser:
class Parser
  def parse(tokens)
    # Implement the parsing logic here to create the AST
  end
end

# Use the Interpreter:
context = Context.new
context.set_variable('x', 10)
context.set_variable('y', 5)

expression = Add.new(Number.new(context.get_variable('x')), Number.new(context.get_variable('y')))
result = expression.interpret

puts "Result: #{result}" # Output: Result: 15

```

## Problem
We need a specialized language to solve a well defined problem of know domain.

We need a specialized language to solve a well defined problem of no (as in
nil) domain.

We need a specialized language to solve a well defined problem of a known domain.

## Solution
Interpreters normally work in two phases: **parsing** and **evaluating**. The parser reads the data and creates a data structure called **abstract syntax tree (AST)**, which includes the same information but represented in a tree of objects. Then, the AST is evaluated against external conditions.
In the AST, the leaf nodes (**terminals**) are the most basic building blocks of the language. The nonleaf nodes (**nonterminals**) represent the higher order concepts in the language. After providing the external conditions (**context**), the AST is evaluated recursively.

## Example
Let's consider a file search tool that we'd like to use with a simple query language. The most basic operation is returning all the files and could be implemented like this:

```ruby
require 'find'

class Expression
  # Common expression code will go here soon...
end

class All < Expression
  def evaluate(dir)
    results= []
    Find.find(dir) do |p|
      next unless File.file?(p)
      results << p
    end
    results
  end
end
```

Also, we'd need to fetch files whose name match a certain pattern:

```ruby
class FileName < Expression
  def initialize(pattern)
    @pattern = pattern
  end

  def evaluate(dir)
    results= []
    Find.find(dir) do |p|
      next unless File.file?(p)
      name = File.basename(p)
      results << p if File.fnmatch(@pattern, name)
    end
    results
  end
end

expr_all = All.new
files = expr_all.evaluate('test_dir')
```

Some other interesting operations are looking for files bigger than a given size or searching writable files:

```ruby
class Bigger < Expression
  def initialize(size)
    @size = size
  end

  def evaluate(dir)
    results = []
    Find.find(dir) do |p|
      next unless File.file?(p)
      results << p if( File.size(p) > @size)
    end
    results
  end
end

class Writable < Expression
  def evaluate(dir)
    results = []
    Find.find(dir) do |p|
      next unless File.file?(p)
      results << p if( File.writable?(p) )
    end
    results
  end
end
```

These basic operations are the **terminals** of our AST. Let's build the first **nonterminal** that negates the operation:

```ruby
class Not < Expression
  def initialize(expression)
    @expression = expression
  end

  def evaluate(dir)
    All.new.evaluate(dir) - @expression.evaluate(dir)
  end
end
```

Now we can find files that are not writable, for example:

```ruby
expr_not_writable = Not.new( Writable.new )
readonly_files = expr_not_writable.evaluate('test_dir')
```

If instead of negating an operation we'd like to combine the results of two of them or get files that meet two conditions, we could build the **OR** nd **AND** nonterminal:

```ruby
class Or < Expression
  def initialize(expression1, expression2)
    @expression1 = expression1
    @expression2 = expression2
  end

  def evaluate(dir)
    result1 = @expression1.evaluate(dir)
    result2 = @expression2.evaluate(dir)
    (result1 + result2).sort.uniq
  end
end

class And < Expression
  def initialize(expression1, expression2)
    @expression1 = expression1
    @expression2 = expression2
  end

  def evaluate(dir)
    result1 = @expression1.evaluate(dir)
    result2 = @expression2.evaluate(dir)
    (result1 & result2)
  end
end

big_or_mp3_expr = Or.new( Bigger.new(1024), FileName.new('*.mp3') )
big_or_mp3s = big_or_mp3_expr.evaluate('test_dir')
```

You might have noticed that **Interpreter** also implements the **Composite** pattern, which gives us flexibility to add new operations or to combine the existing ones:

```ruby
complex_expression = And.new(
                      And.new(Bigger.new(1024), FileName.new('*.mp3')),
                      Not.new(Writable.new ))
complex_expression.evaluate('test_dir')
```
