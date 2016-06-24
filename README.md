valuable
========

This caches all the values from the db on initialize and makes them availabe as class methods, so for a table like

###Role
id | name
---|:---|:---
1  | admin
2  | employee

you get convenience method like

Role.admin
Role.employee

Role.admin.id == 1 #true

```ruby
  def self.method_missing(method_sym, *arguments, &block)
    unless defined? ROLES
      roles_hash = Role.all.uniq.compact.inject({}) do |memo, i|
        memo[i.name.to_sym] = i
        memo
      end
      Role.const_set(:ROLES, roles_hash)
    end
    if Role::ROLES.keys.include?(method_sym)
      Role::ROLES[method_sym]
    else
      super
    end
  end
```
