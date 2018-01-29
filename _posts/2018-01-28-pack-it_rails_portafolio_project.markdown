---
layout: post
title:      "Pack-it! Rails Portfolio Project"
date:       2018-01-28 23:57:00 -0500
permalink:  pack-it_rails_portafolio_project
---


Task managers seemed like a great way to meet the VARIOUS requirements for my Rails project. Since I had a trip coming up, I thought it would be a great idea for my application to be a trip manager. Coming up with the name for it was the easiest part, how many times have you asked yourselves while sitting in a plane.. "Toothbrush, did I pack it?"... haha. I know I do that all the time! This app lets you create an account, then a trip and then add items to your packing list, then you can mark your trip as complete.  

My Active Record associations...


```
User
has_many :trips


Trip
belongs_to :user
has_many :packing_lists
has_many :items, through: :packing_lists


Packing List 
belongs_to :trip
belongs_to :item


Item
has_many :packing_lists
has_one :trip, through: :packing_lists
```


One of the requirement for this project is to include a nested form where a model writes to an associated model through a custom attribute writer. The confusion was real after a month break from rails, and it probably took me about 2 weeks of working on the same thing to figure out how to properly meet this requirement. I decided to have the Trips model write to the Items model, two models associated through a join model, which is the PackingList (should've called it TripItems to avoid confusion). 

#### Nested Forms & Custom Attribute Writers

Creating a Nested Form will allow for each instance of Trip to have associated Items to it (two models, not directly related), so rather than going through the join model, we can directly build items on to our trip. We do this by adding a custom attribute writter in our Trip model page, and adding the accepts_nested_attributes_for macro. 


From the rails docs on this macro...
> Nested attributes allow you to save attributes on associated records through the parent. By default nested attribute updating is turned off and you can enable it using the #accepts_nested_attributes_for class method. When you enable nested attributes an attribute writer is defined on the model.

However, this model needs to be customized to not create a new Item if it already exist, and that's where the custom attribute writer method comes in. 


```
class Trip < ApplicationRecord

accepts_nested_attributes_for :items 


def items_attributes=(item_attributes)
    item_attributes.values.each do |item_attribute|
        new_item = Item.find_or_create_by(item_attribute)
        self.items << new_item
    end
  end
	
end
```

The above method customizes the way each Item is created, we're only creating items that dont already exist with the current name using  the find_or_create rails method, which finds the first record with the given attributes or creates a record with the attributes if one is not found. Then we call self.items, which returns an array of Item objects, and then we call the shovel (<<) method to add our newly found or created Item object to the array of trip items.  

Now that we've added our custom attribute writter method to our Trip model, let's take a moment to update our Trip params in our Trips controller and remember our Trip associations... 

Our trip has many items, but not directly which is why we added the nested form.  

```
Trip
has_many :items, through: :packing_lists
```

#### We like options:

*  To associate existing items to our Trip I would add :item_ids => [], to trip params in our Trip controller, notice that the params :item_ids has the value of an array, because many items can belong to a trip... ;)

Trip params after adding :item_ids => []

```
{"name"=>"Blah", "duration"=>"Blah!", "start_date" => "Blah", "status" => "0",  "item_ids"=>["2", "3", "4"]}
```

Trip Controller

```
def trip_params
    params.require(:trip).permit(:name, :duration, :start_date, :status, :item_ids => [])
  end
```

Within our Trip View New Form (This will create a checkbox field for each Item in our database)

```
<%= form_for ([current_user, trip]) do |f| %>

... other code

<%= f.collection_check_boxes :item_ids, Item.all, :id, :name %> 
<%= f.submit %>
<% end %>
```

But what about creating new Items?

*  To create a new Item, I need to add a text_field to my form to enter the name of the new item, the fields_for helper can be used to easily achieve that.  The value of the name should be nested under our trip_params in our Trip controller.  


```
def trip_params
    params.require(:trip).permit(:name, :duration, :start_date, :status, :item_ids => [], items_attributes: [:name])
  end
```

Within our Trip View New Form (This will create a new field to enter new item)

```
<%= form_for ([current_user, trip]) do |f| %>

... other code

<%= f.collection_check_boxes :item_ids, Item.all, :id, :name %> 
<%= f.fields_for :items, trip.items.build do |trip_fields| %>
	<%= trip_fields.text_field :name %>
<% end %>

<%= f.submit %>
<% end %>
```


And thats it! I was having a few issues creating empty items and not being able to update them or delete them, but after modifying my #accepts_nested_attributes_for macro a bit, I got things to work the way I wanted. Here is what it looked like in the end. 

```
accepts_nested_attributes_for :items, reject_if: proc {|attributes| attributes['name'].blank?}, allow_destroy: true
```

You can read more about this in the Rails docs here ...

http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html






