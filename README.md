# Box (name taken, find a better one)

Ruby library for setting up test data

## Set up your test data by using Boxes

```ruby
# spec/support/boxes/user_box.rb
class UserBox < ApplicationBox
  def record
    User.new name: "Paul",
      email: "paul@thoughtbot.com"
  end

  # Instead of traits
  def admin
    # setters returns self so you can chain calls on the box
    admin = true
  end

  # Instead of transient attributes/traits
  def tagged_with(tag)
    # This method returns self so you can chain calls on the box
    after_build do
      TagBox.create(user: @record)
    end
  end
end

# Instead of nested factories
class Admin < UserBox
  def record
    super.tap do |user|
      user.admin = true
    end
  end
end
```

## Using it in a test

```ruby
UserBox.build
UserBox.build(name: "Dave")
UserBox.create
UserBox.create(name: "Dave")
UserBox.new.admin.tagged_with("manager").build(name: "Dave")
UserBox.new.admin.tagged_with("manager").create(name: "Dave")
```
