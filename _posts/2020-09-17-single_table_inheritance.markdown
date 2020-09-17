---
layout: post
title:      "Single Table Inheritance "
date:       2020-09-17 20:05:47 +0000
permalink:  single_table_inheritance
---


Finally finishing up my React-Redux final project here for flatiron I was contemplating writing this blog on a couple of different things. When I started this project there was one feauture I knew for a fact that was going to be the biggest headache to implement and that feature was the usage of multiple user types. In my case a salesman (PCA or Pest Control Advisor) and a grower or farmer in the agricultural industry. After looking around at options I stumbled across single table inheritance and knew it was perfect for what i wanted to do. 

First things first what is single table inheritance one might ask. Single-table inheritance (STI) is the practice of storing multiple types of values in the same table, where each record includes a field indicating its type, and the table includes a column for every field of all the types it stores. Basicslly to put it in example form I have a parent model User and two child models Pca (salesman) and Grower. 

class User < ApplicationRecord
    
    has_secure_password

    validates :username, :email, uniqueness: true
    validates :username, :first_name, :last_name, :email, :password_digest, :type, :bio, presence: true

end

Above is the parent model that as you can see nothing too crazy going on there seems like a regular model with validations. Now when implementing the two child models its just a little different. 

class Pca < User 
    has_many :growers
       
end

class Grower < User 
    belongs_to :pca
    has_many :crop_infos

end

The only difference on the outside looking in is that the parent is inheriting from ApplicationRecord where as the children are inheriting from the User model. When using STI you have to be sure you add a "type" column onto the table because that is what makes the magic happen. Depending on the type of User in this case results in which user type instance is going to be created and persisted to the database. To give context here is a look at my users table. 

create_table "users", force: :cascade do |t|
    t.string "username"
    t.string "first_name"
    t.string "last_name"
    t.string "email"
    t.string "password_digest"
    t.string "type"
    t.string "bio"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

Now one thing to note when using STI I would recommend using it IF both child models are sharing all or close to all the same attributes and columns otherwise I'd recommend using MTI (muliple table inheritance). With the help of some rails magic the type column is everything. In my case if i distinguish the type to be "Pca" rails creates an instance of A Pca and the same with a grower. On my frontend I have a dropdown when you sign up for an account for PCA and Grower and depending on which you choose it sends that to the API and it then creates that particular instance. 

Here is the example from my sign up form, i put the two different types into a dropdown 

import React from 'react' 

const DropDowns = (props) => {

    return (
    <div>
        <div className={props.divClassName}>
            <label>{props.label}</label>
            <select name={props.inputName} value={props.inputVal} onChange={props.onChange}>
            <option></option>
            <option name="Pca" value="Pca">PCA</option>
            <option name="Grower" value="Grower">Grower</option>
            </select>
        </div>
    </div>
    )

}



    export default DropDowns

so when the name="Pca" and type value="Pca" it sends that to the backend and the backend creates the Pca instance and then does the same for grower if that type is chosen. After that I take the response from the backend and dispatch a LOGIN_USER type and when I go to render which user information I want to display I add a little conditional logic to say hey if the users type is Pca show them this, else show them the growers homepage. 

All in all if you're looking to implement multiple user types into your application I would say this is the easiest to implement although it was a big headache to get it working but what isn't? Like I said before if your child models share the same or almost all of the same attributes Single Table Inheritance is the way to go!






