In Ruby, you can implement the Dependency Injection (DI) pattern by injecting dependencies into a class rather than having the class create them internally. This promotes decoupling and makes your code more flexible and testable. Here's a basic example:

```ruby
class EmailSender
  def send_email(to, message)
    # Logic to send an email
    puts "Sending email to #{to}: #{message}"
  end
end

class OrderProcessor
  def initialize(email_sender)
    @email_sender = email_sender
  end

  def process_order(order)
    # Process the order
    # ...
    
    # Send an email notification
    @email_sender.send_email(order.customer_email, "Your order has been processed.")
  end
end

# Usage
email_sender = EmailSender.new
order_processor = OrderProcessor.new(email_sender)

order = { customer_email: 'customer@example.com', /* order details */ }
order_processor.process_order(order)
```

In this example, we have two classes: `EmailSender` and `OrderProcessor`. `OrderProcessor` depends on `EmailSender`, and we inject the `EmailSender` instance into `OrderProcessor`'s constructor. This way, you can easily replace `EmailSender` with a different implementation or a mock for testing purposes without modifying `OrderProcessor`.

This demonstrates the basic idea of Dependency Injection in Ruby. It allows you to decouple components, making your code more maintainable and testable.
