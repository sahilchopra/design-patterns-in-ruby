# Adapter Pattern

## Problem
We want an object talk to some other object but their interfaces don't match.

## Solution
We simply wrap the **adaptee** with our new **adapter** class. This class implements an interface that the invoker understands, although all the work is performed by the adapted object.

## Example
```ruby
# Step 2: Define the Target Interface
class SystemInterface
  def execute
    raise NotImplementedError, "Subclasses must implement this method"
  end
end

# Step 3: Create the Adapter Class
class SystemAdapter < SystemInterface
  def initialize(adaptee)
    @adaptee = adaptee
  end

  # Step 4: Implement the Adapter Methods
  def execute
    @adaptee.perform_action
  end
end

# Step 1: Existing class with an incompatible interface
class OldSystem
  def perform_action
    puts "Old system is performing an action."
  end
end

# Existing class with another incompatible interface
class NewSystem
  def execute_action
    puts "New system is executing an action."
  end
end

# Step 5: Use the Adapter
old_system = OldSystem.new
new_system = NewSystem.new

adapter_for_old_system = SystemAdapter.new(old_system)
adapter_for_new_system = SystemAdapter.new(new_system)

adapter_for_old_system.execute
adapter_for_new_system.execute


````



Let's think of a class that takes two open files (a reader and a writer) and encrypts a file.

```ruby
class Encrypter
  def initialize(key)
    @key = key
  end

  def encrypt(reader, writer)
    key_index = 0
    while not reader.eof?
      clear_char = reader.getc
      encrypted_char = clear_char ^ @key[key_index]
      writer.putc(encrypted_char)
      key_index = (key_index + 1) % @key.size
    end
  end
end
```

But what happens if the data we want to secure happens to be in a string, rather than in a file? We need an object that looks like an open file, that is, supports the same interface as the Ruby `IO` object. We can create an `StringIOAdapter` to achieve so:

```ruby
class StringIOAdapter
  def initialize(string)
    @string = string
    @position = 0
  end

  def getc
    if @position >= @string.length
      raise EOFError
    end
    ch = @string[@position]
    @position += 1
    return ch
  end

  def eof?
    return @position >= @string.length
  end
end
```

Now we can use a `String` as if it were an open file (it only implements a small part of the `IO` interface, essentally what we need).

```ruby
encrypter = Encrypter.new('XYZZY')
reader= StringIOAdapter.new('We attack at dawn')
writer=File.open('out.txt', 'w')
encrypter.encrypt(reader, writer)
```
