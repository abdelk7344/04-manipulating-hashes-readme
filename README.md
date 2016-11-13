# Manipulating Hashes: README

## Objectives

In this lesson, we'll be taking a closer look at multidimensional, or nested, hashes, and how to iterate through them.  We'll go through a few examples before you try it on the next lab.  

## More On Building Nested Hashes 

Let's say we have the following nested hash: 

```ruby
contacts = {
  "Jon Snow" => {
    name: "Jon",
    email: "jon_snow@thewall.we", 
    favorite_icecream_flavors: ["chocolate", "vanilla"],
    knows: nil
  },
  "Freddy Mercury" => {
    name: "Freddy",
    email: "freddy@mercury.com",
    favorite_icecream_flavors: ["strawberry", "cookie dough", "mint chip"]
  }
}
```
### Adding Information to Nested Hashes

#### Example 1:

Your good buddy Jon Snow has just tried mint chip ice cream for the first time. He loved it a lot and now you need to add "mint chip" to his list of favorite ice creams. We already know how to access the array of ice cream flavors that constitute the value of the `:favorite_icecream_flavors` key: 


```ruby
contacts["Jon Snow"][:favorite_icecream_flavors]
#  => ["chocolate", "vanilla"]
```

How can we add a flavor to the list? Well, `:favorite_icecream_flavors` is an array, so we can use the same syntax as above to access that array along with our old friend `<<` (the shovel method) to add an item to the array. Let's take a look: 

```ruby
contacts["Jon Snow"][:favorite_icecream_flavors] << "mint chip"

puts contacts 
#  => {
  "Jon Snow" => {
    name: "Jon",
    email: "jon_snow@thewall.we", 
    favorite_icecream_flavors: ["chocolate", "vanilla", "mint chip"],
    knows: nil
  },
  "Freddy Mercury" => {
    name: "Freddy",
    email: "freddy@mercury.com",
    favorite_icecream_flavors: ["strawberry", "cookie dough", "mint chip"]
  }
}
```

#### Example 2: 

Now let's say that you want to add a whole new key/value pair to your Jon Snow contact—his address. In a simpler hash with just one level of key value pairs, we've already learned how to add new key/value pairs. Here's a reminder: 

```ruby
hash = {first: "first value!", second: "second value!"}
hash[:third] = "third value!"

puts hash
#  => {first: "first value!", second: "second value!", third: "third value!"}
```

In a nested hash, we can add new key/value pairs in a similar way. We need to first access the level of the hash to which we want to add a key value pair, and then we can create a new key value on the second level using the same notation. Since we want the `"Jon Snow"` key to include a new key/value pair of his address, the implementation should look like this: 

```ruby
contacts["Jon Snow"][:address] = "The Lord Commander's Rooms, The Wall, Westeros"

puts contacts
#  => 
{
"Jon Snow" => {
   :name=>"Jon", :email=>"jon_snow@thewall.we", 
   :favorite_icecream_flavors=>["chocolate", "vanilla", "mint chip"], 
   :knows=>nil,
   :address=>"The Lord Commander's Rooms, The Wall, Westeros"}, 
"Freddy Mercury"=> { 
   :name=>"Freddy", 
   :email=>"freddy@mercury.com", 
   :favorite_icecream_flavors=> ["cookie dough", "mint chip"]
 }
}

```

## Iterating Over Nested Hashes

So far, we've only iterated over hashes that had one level—a series of key/value pairs on a single tier. What happens when we want to iterate over a multidimensional hash like the one above? Let's iterate over our nested hash one level at a time; iterating over the first level of our hash would look like this: 

```ruby
contacts = {
  "Jon Snow" => {
    name: "Jon",
    email: "jon_snow@thewall.we", 
    favorite_icecream_flavors: ["chocolate", "vanilla", "mint chip"],
    knows: nil
  },
  "Freddy Mercury" => {
    name: "Freddy",
    email: "freddy@mercury.com",
    favorite_icecream_flavors: ["strawberry", "cookie dough", "mint chip"]
  }
}

contacts.each do |person, data|
  puts "#{person}: #{data}"
end
```

This should return: 

```ruby
Jon Snow:      
{ :name=>"Jon", 
  :email=>"jon_snow@thewall.we", 
  :favorite_icecream_flavors=>["chocolate", "vanilla", "mint chip"],
  :knows=>nil
}

Freddy Mercury: 
{ :name=>"Freddy", 
:email=>"freddy@mercury.com", 
:favorite_icecream_flavors=>["strawberry", "cookie dough", "mint chip"]
}
```

On the first level, the keys are our contacts' names, "Jon Snow" and "Freddy", and our values are the hashes that contain a series of key/value pairs describing them. 

Let's iterate over the second level of our `contacts` hash. In order to access the key/value pairs of the second tier (i.e. the name, email, and other data about each contact), we need to iterate *down into* that level. So, we pick up where we left off with the previous iteration and we keep going: 

```ruby
contacts.each do |person, data|
  #at this level, "person" is Jon Snow or Freddy and "data" is a hash of key/value pairs
  #to iterate over the "data" hash, we can use the following line: 
  
  data.each do |attribute, value|
    puts "#{attribute}: #{value}"
  end
end
```

That should output the following: 

```ruby

name: Jon
email: jon_snow@thewall.we
favorite_icecream_flavors: ["chocolate", "vanilla", "mint chip"]
knows: nil

name: Freddy
email: freddy@mercury.com
favorite_icecream_flavors: ["strawberry", "cookie dough", "mint chip"]

```

Let's take this one step further and print out *just the favorite ice cream flavors*. Once again, we'll need to iterate down into that level of the hash, then we can access the favorite ice cream array and print out the flavors: 

```ruby
contacts.each do |person, data|
  #at this level, "person" is Jon Snow or Freddy and "data" is a hash of key/value pairs
  #to iterate over the "data" hash, we can use the following line: 
  
  data.each do |attribute, value|
    #at this level, "attribute" is describes the key of :name, :email, :favorite_icecream_flavors, or :knows
    #we need to first check and see if the key is :favorite_icecream_flavors,
    #if it is, that means the VALUE is an array that we can iterate over to print out each element
    
    if attribute == :favorite_icecream_flavors
      value.each do |flavor|
        # here, each index element in an ice cream flavor string
        puts "#{flavor}"
      end
    end
  end
end
```

This should output: 

```ruby
chocolate
vanilla
mint chip
strawberry
cookie dough
mint chip
```


In the next lab, you're going to iterate through the levels of this hash to operate on one of the ice cream flavor arrays. 

## Resources: 

* [Ruby Docs on Hashes](http://ruby-doc.org/core-2.2.0/Hash.html)