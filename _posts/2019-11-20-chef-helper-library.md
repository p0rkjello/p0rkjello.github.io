---
layout: post
title: 'Chef helper library' 
author: 'Andrew Bounds'
tags:
- chef
- automation
- ruby
---

### Chef helpers / Custom modules

I found myself in need of a custom [guard](https://docs.chef.io/resource_common.html) for a Chef recipe using the [registry_key](https://docs.chef.io/resource_registry_key.html) resource. I was able to create a module to eliminate code repetition. The following is a simplified example.

To get started, generate a new cookbook `chef generate cookbook example_helper`.

There isn't a `chef generate` for [libraries](https://docs.chef.io/libraries.html). Manually create the `libraries` directory and a new ruby file. In this example, I have created a file named `my_helper.rb`.

Within the `my_helper.rb` file I created a new method called `in_file?`. The `in_file?` method will search the text file `'c:\temp\my_helper.txt'` for a specific word and return true if found.
Additionally, the method will need to be included in the recipe, resource, etc. To prevent clobbering any existing method with the same name include it only where needed.

***example_helper > libraries > my_helper.rb***

```ruby
# libraries/my_helper.rb

module My
  module Helper
    def in_file?(word)
      # example code not for actual use
      file = 'c:\temp\my_helper.txt'
      if File.exist?(file)
        File.readlines(file).grep(/#{word}/).any? # return truth
      end
    end
  end
end

# Send the helper to the required resource
Chef::Resource::RegistryKey.send(:include, My::Helper)
```

### Attribute file

Create a new attribute file `chef generate attribute default`. I populated the attribute file with some values to loop over.

***example_helper > attributes > default.rb***

```ruby
# attribute/default.rb

default['example_helper']['regkey']['key1'] = 'value1'
default['example_helper']['regkey']['key2'] = 'value2'
default['example_helper']['regkey']['key3'] = 'value3'
default['example_helper']['regkey']['key4'] = 'value4'
```

### Cookbook recipe

The recipe iterates through the cookbook attributes and creates a new registry key. The guard uses the `in_file?` method created above. If the text file contains the attribute name the __registry_key__ resource will be skipped for that iteration.

***example_helper > recipes > default.rb***

```ruby
# recipe/default.rb

return unless platform_family?('windows')

node['example_helper']['regkey']&.each do |name, val|
  registry_key "HKLM\\System\\CurrentControlSet\\Control\\DoesNotExist\\#{name}" do
    action :create
    recursive true
    values [{ name: 'Enabled', type: :dword, data: val }]
    not_if { in_file?(name) }
  end
end
```

### Example word list file

Add the words we want to trigger the `in_file?` method. In this case, we will add two of the four attribute values.

***c:\temp\my_helper.txt***

```plaintext
key2
key4
```

### Run the Chef Client

Run the Chef cookbook `chef-client -z -o example_helper`. Using the helper along with the guard registry keys key2 and key4 will be skipped.

```console
➜  chef-client -z -o example_helper

Recipe: example_helper::default
  * registry_key[HKLM\System\CurrentControlSet\Control\DoesNotExist\key1] action create
    - create key HKLM\System\CurrentControlSet\Control\DoesNotExist\key1
    - set value {:name=>"Enabled", :type=>:dword, :data=>"value1"}
  * registry_key[HKLM\System\CurrentControlSet\Control\DoesNotExist\key2] action create (skipped due to not_if)
  * registry_key[HKLM\System\CurrentControlSet\Control\DoesNotExist\key3] action create
    - create key HKLM\System\CurrentControlSet\Control\DoesNotExist\key3
    - set value {:name=>"Enabled", :type=>:dword, :data=>"value3"}
  * registry_key[HKLM\System\CurrentControlSet\Control\DoesNotExist\key4] action create (skipped due to not_if)

Running handlers:
Running handlers complete
Chef Infra Client finished, 2/4 resources updated in 03 seconds
```
