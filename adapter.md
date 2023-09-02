# Adapter Pattern

The Adapter Pattern is a structural design pattern that allows objects with incompatible interfaces to work together.

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


