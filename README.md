valuable
========

This caches all the values from the db on initialize and makes them availabe as class methods, so for a table like

Role
id | name 
1  | admin
2  | employee

you get convenience method like

Role.admin
Role.employee

Role.admin.id == 1 #true

```ruby
class Role < ActiveRecord::Base
  
  def self.const_missing(const)
    if const == :ROLES
      Role.const_set(:ROLES, Role.all.collect(&:name).uniq.compact)
      Role::ROLES
    else
      super
    end
  end

end
```
