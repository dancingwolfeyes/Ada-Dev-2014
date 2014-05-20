### **1. What does the command “bundle gem foodie” do?**

Creates a Gem using Bundler named ‘foodie’.


### **2.In what folder do we put our test files?**

Spec


### **3.What do we need to write to add the activesupport (version 4.0.0) dependency to our gemspec?**

spec.add_dependency "activesupport", "4.0.0"


### **4.What steps need to be taken to write a generator?**

define a generator class just like we define a CLI class:
desc "recipe", "Generates a recipe scaffold"
def recipe(group, name)
  Foodie::Generators::Recipe.start([group, name])
end
We will need to require the file for this new class, which we can do by putting this line at the top of lib/foodie/cli.rb:
require ‘foodie/generators/recipe'

To define this class, we inherit from Thor::Group 
include the Thor::Actions module to define helper methods for our generator. 
put it in a new namespace called "generators", making the location of this file lib/foodie/generators/recipe.rb:
require 'thor/group'
module Foodie
  module Generators
    class Recipe < Thor::Group
      include Thor::Actions

      argument :group, :type => :string
      argument :name, :type => :string
    end
  end
end

define methods in the class. Let's define a create_group method inside this class.

def create_group
  empty_directory(group)
end

To put the file in this directory, we will use the template method. We will define a copy_recipe method to do this now:
def copy_recipe
  template("recipe.txt", “#{group}/#{name}.txt")
end

Test run our generator! (We can do this without using Cucumber by running bundle exec bin/foodie recipe dinner steak, but just this once.) Generally we'd test it solely through Cucumber. When we run this command we'll be told all of this:
create  dinner
Could not find "recipe.txt" in any of your source paths. Please invoke Foodie::Generators::Recipe.source_root(PATH) with the PATH containing your templates. Currently you have no source paths.

define the source_root method for our generator. 
define it as a class method in Foodie::Generators::Recipe like this:
def self.source_root
  File.dirname(__FILE__) + "/recipe"
end

create the template, which we can put at lib/foodie/generators/recipe/recipe.txt:
##### Ingredients #####
Ingredients for delicious <%= name %> go here.


##### Instructions #####
Tips on how to make delicious <%= name %> go here.

When we run bundle exec cucumber features all our features will be passing!
3 scenarios (3 passed)
7 steps (7 passed)
