## **Adrienne Rawlinson**


### **1. What does the command “bundle gem foodie” do?**

Creates a Gem using Bundler named ‘foodie’.


### **2.In what folder do we put our test files?**

Spec


### **3.What do we need to write to add the activesupport (version 4.0.0) dependency to our gemspec?**

spec.add_dependency "activesupport", "4.0.0"


### **4.What steps need to be taken to write a generator?**

in foodie/lib/cli.rb  // All info below will be defined in our CLI library for our foodie module.

	require 'foodie/generators/recipe'  // Tells CLI we will require a generator for recipe class. Require file for new class.

	desc "recipe", "Generates a recipe scaffold"  // Descibes 'recipe' as scaffolding for food formulas.
	def recipe(group, name)  // Define 'recipe' as having a group and name.
	  Foodie::Generators::Recipe.start([group, name])  // When run, calls up the recipe with group and name.
	end

require 'thor/group' // Tells CLI we reqire Thor/group...even though we have not inherited it yet.
module Foodie  // In our foodie module...
  module Generators  // for our generator in...
    class Recipe < Thor::Group  // our 'recipe' class we inherit Thor from the group...
      include Thor::Actions  // and include Thor Actions for funtionality.

      argument :group, :type => :string  // Augments Recipe Class to include 'group' using string of text.
      argument :name, :type => :string  // Augments Recipe Class to include 'name' using string of text.
    
      def create_group  // Creates and Defines 'group' for CLI as...
        empty_directory(group)  // an empty directory.
      end

      def copy_recipe  // creates and defines our 'recipe' as...
        template("recipe.txt", "#{group}/#{name}.txt")  // text being copied to our group and name template.
      end

      def self.source_root  // define source root so we can...
        File.dirname(__FILE__) + "/recipe"  // make sure the file is in the right place within the directory.
      end
    end
  end
end

            [cli.rb  ] [cmd]  [arguments]
bundle exec bin/foodie recipe dinner steak  // Calling up our steak dinner recipe. Yum.
