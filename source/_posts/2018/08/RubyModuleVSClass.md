---
title: The Distinguish Of Module , Class When Nested In Ruby
tags:
  - Ruby
date: 2018-08-06
---

For Module can't be instance , so can't call it directily , include it to class , new the class and call module.
<!-- more -->
```ruby
module M1
  module M2
    class C3
      def two
        p 'this is two'
      end
    end
    def one
      p 'this is one'
    end
  end
end
 
class C1
  include M1
  include M1::M2
end
 
class C2
  include C1::M2
end
C1::C3.new.two
C1.new.one
C2.new.one
 
#result
#"this is two"
#"this is one"
#"this is one"
```