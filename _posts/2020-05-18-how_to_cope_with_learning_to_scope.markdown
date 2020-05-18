---
layout: post
title:      "How to cope with learning to scope"
date:       2020-05-18 04:18:20 +0000
permalink:  how_to_cope_with_learning_to_scope
---



The hardest part with writing a blog is starting, so here I go. For the last 3 weeks I've been working on my Ruby on Rails (mod 3) project for flatiron school. I decided to build a fake CRM application. For those of you who do'nt know what a CRM is or stands for it is a Customer Relationship Management (CRM) s a technology for managing all your company's relationships and interactions with customers and potential customers. One thing it does is help companies stay connected to customers. One requirement of the rails project is to implement a scope or query method and use it in the application. While never using one once before or ever seeing really anything like it I had to deep dive to really understand what it was. Here's what I learned. 

First of all, you might be wondering what a scope method even is, a scope method is a custom query that you can define inside of a select model to retrieve either an instance/instances and you can also use them to pull data pertaining to attributes of a model.  Scope methods are a lot like class methods but there is a big difference at the same time. Scope methods return a relation object which means you can call another scope or active record querying method on it and kind of chain them together if you wanted. The specific query method I used for my application was order. I wanted to figure out a way where I can take my organization model (or company) and retireve the three highest revenue generating companies and display those three in my organization index view. 

In order to to complete the task of ordering the companies with the three highest revenues the first thing I had to do was define the scope method In my organization model. 

```     scope :highest, -> {order("revenue DESC").limit(3)} ```


with the line of code above we are defining the method scope called :highest, using a lambda ( -> ) to point to what query method we want to use in my case I used order. After that inside the next set of parenthesis inside of the quotes is where I pick the name of the attribute I used which was revenue. Taking all of the organizations in the database and setting them up to order by descending which starts at the top and goes down and then limiting the output of the oragnizations to only show the top 3 companies. 

After doing that the next step is to go to that particular controller and find the action of the view you want to display these changes to. In my case it was the index action. 

```  def index 
        @organizations = Organization.highest
    end ```
		
		
		
		Here I'm setting the instance variable @organizations to make it more flexible and so I'm able to call on it inside the index view and display the top 3 revenue generators. 

		The last and final step to display it is to use an erb tag ( <% %> ) inside of that you call on the @organizations instance variable that we created above and iterate over it and display the data in the view 
		
		
		``` <% @organizations.each do |org| %>
    <ul style="list-style-position:inside; border-style: solid; border-width: 4px; border-radius: 15px; width: 300px; height: 550px; padding: 10px;" >  
            <h4><li> Name:</li> </h4>
             <%= org.name %> 
    <br><br>

    <h4><li> Amount of revenue:</li> </h4>
         <%= org.revenue %>    
    <br><br>

    <h4><li> Country:</li> </h4>
         <%= org.country %>    
    <br><br>

    <h4><li> State:</li> </h4>
         <%= org.state %>  
    <br><br>

    <h4><li> Address:</li> </h4>
         <%= org.address %>    
    <br><br>

    <h4><li> Customer:</li> </h4>
         <% if org.customer == true %>     
         yes
        <% else %> 
        no
    <% end %> 

    <br><br>


     <%= link_to "Edit Organization", edit_organization_path(org) %>   
    <br><br>

     <%= link_to "Delete Organization", organization_path(org), method: :delete, data: { confirm: "Really?" }%>    
    <br><br>

    <br><br>
</ul>
<br><br>
<% end %> ``` 

Just like that we get the top 3 organizations that generated the most money.  


		
		








