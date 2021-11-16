# Super

## Learning Goals

- Use `super` to call methods on a parent class from the child class

## Introduction

So far, we've seen the benefits of using inheritance to create a group of
classes that share certain characteristics and behaviors. However, up until now,
the implementation of shared characteristics has been somewhat rigid. If class
`Student` inherits from class `User`, we can choose to either allow the
`Student` class to inherit a certain method from `User` or overwrite that method
with another implementation that is specific to `Student`.

But what if there is a method in the parent class that we want our child to
share _some_ of the functionality of? Or what if we want our child class to
inherit a method from the parent and then augment it in some way? We can achieve
this with the use of the `super` keyword.

## Using `super` to Supercharge Inheritance

Let's say we are working on an education app in which users are either students
or teachers. We have a parent, `User` class, that both our `Student` and
`Teacher` classes inherit from.

Our `User` class has a method, `log_in`, that sets an instance variable,
`@logged_in` equal to `true`.

```ruby
class User

  def log_in
    @logged_in = true
  end
end
```

However, when a student logs into our app, we need to not only set their logged
in attribute to `true`, we need to set their "in class" attribute to true. We
could simply edit the `#log_in` method in the `User` class to account for this.
But that doesn't make sense here. Remember that both `Student` and `Teacher`
inherit from `User`. Teachers don't need to indicate that they are "in class",
so we don't want to alter the `#log_in` method of our parent class and
inadvertently give teachers some behavior that they don't want or need.

Instead, we can augment, or supercharge, the `#log_in` method _inside_ of the
`Student` class.

Let's take a look:

```ruby
class Student < User
  def log_in
    super
    @in_class = true
  end
end
```

Here, we re-define the `#log_in` method and tell it to inherit any functionality
of the `#log_in` method defined in the parent, or "super", class, which is
`User`.

In the `#log_in` method above, the `super` keyword will **call on the `#log_in`
method as defined in the super class**. _Then_, the additional code that we're
adding into our `Student#log_in` method will also run. We have therefore
supercharged our `#log_in` method, for the `Student` class only.

You can see for yourself by adding a `puts` method in the `#log_in` methods for
both the `User` and `Student` classes. Run this code in IRB or in a new Ruby
file:

```rb
class User
  def log_in
    puts "User#log_in called"
    @logged_in = true
  end
end

class Student < User
  def log_in
    puts "Student#log_in called"
    super
    @in_class = true
  end
end

oneil = Student.new
oneil.log_in
# "Student#log_in called"
# "User#log_in called"
#  => true
```

As you can see by the output of running `#log_in` on a `Student` instance, the
`super` keyword calls the method of the same name that's defined in the `User`
superclass.

## Calling `super` With Arguments

The `#log_in` method defined above didn't take any arguments, but often, we'll
need to call methods and provide some data as arguments. For example, let's
modify our `User` class to give it a `name` attribute:

```rb
class User
  attr_accessor :name

  def initialize(name)
    @name = name
  end

  def log_in
    @logged_in = true
  end
end
```

Let's also modify our `Student` class so that it can be initialized with a
`name` and a `grade` attribute:

```rb
class Student < User
  attr_accessor :name, :grade

  def initialize(name, grade)
    @name = name
    @grade = grade
  end

  def log_in
    super
    @in_class = true
  end
end
```

While we can still create new students with our updated class definition, it's
not particularly DRY — both the `Student` and `User` classes have an
`attr_accessor` defined for the `@name` instance variable, and both are
responsible for setting the `@name` instance variable when a new instance is
created.

Ideally, we'd like to be able to call the `#initialize` method on the `User`
class when a new student is created.

`super` lets us do just that! We can call `super` with an argument from the
`Student#initialize` method, which will call the `User#initialize` method with
that argument. Try updating the example code like so:

```rb
class User
  attr_accessor :name

  def initialize(name)
    puts "User#initialize called"
    @name = name
  end

  def log_in
    @logged_in = true
  end
end

class Student < User
  attr_accessor :grade

  def initialize(name, grade)
    puts "Student#initialize called"
    super(name)
    @grade = grade
  end

  def log_in
    super
    @in_class = true
  end
end

oneil = Student.new("O'neil", 10)
# Student#initialize called
# User#initialize called
# => #<Student:0x00007f914705a9a8 @name="O'neil", @grade=10>
```

Just like in the previous example, `super` is used to call a method on the
superclass with the same name as the subclass — the only difference is
that this time we are passing in arguments that are required by the method
defined in the superclass.

## Conclusion

Often when you are dealing with code that involves inheritance, you'll need
to augment the functionality defined in the superclass with some additional
behavior in the subclass. The `super` keyword allows you to do just that!

## Resources

- [How to User The Ruby Super Keyword](https://www.rubyguides.com/2018/09/ruby-super-keyword/)
# -phase-3-ruby-oo-inheritance-super
