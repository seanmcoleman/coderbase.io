---
title: Defining Multiple, Non-Compound SQL Indices with Active Record Migrations
permalink: template-design-pattern
languages: [ruby, rails]
summary: A sample post to show the options
published: true 
project:
---

With [Active Record migrations](http://guides.rubyonrails.org/migrations.html), you can define indices as part of the database migration. At first I thought you could define multiple indices by passing in an array of column symbols.

For example:

```ruby
add_index :table_name, [:column_a, :column_b]
```

Unexpectadly, this creates a compound index of both `column_a` and `column_b`.

In order to get separate indices, you must define them separately:

```ruby
add_index :table_name, :column_a
add_index :table_name, :column_b
```