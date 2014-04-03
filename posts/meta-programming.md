---
title: Metaprogramming to DRY out code
permalink: metaprogramming-to-dry-out-code
languages: [ruby, rails]
summary: How to use the dark art of metaprogramming to keep code DRY
published: true 
project:
---

[Metaprogramming](http://rubymonk.com/learning/books/2-metaprogramming-ruby/chapters/32-introduction-to-metaprogramming/lessons/75-being-meta) is a powerful, yet often misunderstood and misused tool in dynamic programming languages like [Ruby](https://www.ruby-lang.org/en/). In essence, metaprogramming allows you to write code which generates code at runtime. Conceptually this can be very confusing for beginners, but is very powerful for keeping code [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself). 

In a project dealing with real estate listings, I have an API that serializes objects to JSON. Using Rails `ActiveModel::Serializer` I format several currency properties as US denominations. 

For example, the decimal `1534.00` would be formatted AS `$1,534.00`.

The following six methods format the properties, but this code is not as DRY as it could be. The method call to `number_to_currency` is repeated. If we wanted to add a `precision` parameter to all deposit properties, we would have to update six methods. 

```ruby
def security_deposit
  number_to_currency(object.security_deposit)
end

def pet_deposit
  number_to_currency(object.pet_deposit)
end

def earnest_deposit
  number_to_currency(object.earnest_deposit)
end

def cleaning_deposit
  number_to_currency(object.cleaning_deposit)
end

def credit_check_deposit
  number_to_currency(object.credit_check_deposit)
end
```

Using metaprogramming, we can DRY our code with the following:

```ruby
deposit_fields = %w(security_deposit pet_deposit earnest_deposit cleaning_deposit credit_check_deposit)
deposit_fields.each do |prop|
  define_method(prop) do |argument|
    number_to_currency(object.send(prop))
  end
end
```

By simply iterating on an array of property names, we can dynamically generate methods for each serialized property to return the desired currency format. With metaprogramming, we've condensed 15 LOC to 6 and enforced consistency in how deposit fields are formatted. Say we want to restrict the number of decimal places for deposit fields, only one change is needed. 