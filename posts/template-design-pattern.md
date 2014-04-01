---
title: Template Design Pattern: Referencing Subclass Constants in the Base Class
permalink: template-design-pattern
languages: [ruby, rails]
summary: A sample post to show the options
published: true 
project:
---

The [Template Design Pattern](http://reefpoints.dockyard.com/ruby/2013/07/10/design-patterns-template-pattern.html) is a powerful pattern where a skeleton abstract class serves as the blueprint from which concrete classes are derived from in order to inherit common functionality. In the following example, my base class has a method which uses a constant.  This constant `DownloadKlass`, however, is not defined in the base class, but is expected to be defined in each subclass.  Unfortunately the code throws an exception that `DownloadKlass` can not be found. 

(base.rb)

```ruby
class Base
  def download
    DownloadKlass.create!(count: count, downloaded_at: now)
  end
  [...]
end
```

In each subclasses, I define the constant `DownloadKlass`

```ruby
class Residential < Base
  DownloadKlass = ResidentialDownload
end
```

It turns out that you need to reference base class constants via `self.class`

```ruby
def download
        ...
  self.class::DownloadKlass.create!(count: count, downloaded_at: now)
end
```