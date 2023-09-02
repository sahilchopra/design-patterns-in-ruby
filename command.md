# Command Pattern
The Command Pattern is a behavioral design pattern that encapsulates a request as an object, thereby allowing for parameterization of clients with queues, requests, and operations. It also provides the ability to support undoable operations.

## Problem
We want to perform some specific task without knowing how the whole process works or having any information about the receiver of the request.

## Solution
The Command pattern decouples the object that needs to perform a specific task from the one that knows how to do it. It encapsulates all the needed information to do the job into its own object including: who the receiver(s) is(are), the methods to invoke, and the parameters. That way, any object that wants to perform the task only needs to know about the command object interface.

## Example


```ruby
# 1 Define a Command Interface
class Command
  def execute
    raise NotImplementedError, "#{self.class} must implement the execute method"
  end
end


# 2 Create Concrete Command Classes

class LightOnCommand < Command
  def initialize(light)
    @light = light
  end

  def execute
    @light.turn_on
  end
end

class LightOffCommand < Command
  def initialize(light)
    @light = light
  end

  def execute
    @light.turn_off
  end
end

# 3 Create an Invoker

class RemoteControl
  def initialize
    @commands = []
  end

  def add_command(command)
    @commands << command
  end

  def press_button
    @commands.each(&:execute)
  end
end


# 4 Create Receiver Objects
class Light
  def turn_on
    puts 'Light is on'
  end

  def turn_off
    puts 'Light is off'
  end
end


# 5 Client Code:
light = Light.new
light_on = LightOnCommand.new(light)
light_off = LightOffCommand.new(light)

remote = RemoteControl.new
remote.add_command(light_on)
remote.add_command(light_off)

remote.press_button
```
