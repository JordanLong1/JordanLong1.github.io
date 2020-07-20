---
layout: post
title:      "The Great Debug"
date:       2020-07-20 07:24:08 +0000
permalink:  the_great_debug
---


As I was working through my javascript section project towards the end and actually the last "bug" that I needed to fix was surprisingly one of the most difficult that I encountered during the whole project. To give a little overview for my project I did an online shopping cart clothing store type. A much more simple c-commerce style. I had products and the ability to add and remove the products from the cart and so forth. The bug I faced was figuring out how when I had different products in the cart, how I could remove the certain div for the certain product from the cart if there was no more of those products left in the cart. Sounds simple, but let me tell you it was not. Here's what I did. 

First things first I had to take a step back and think okay, how can I assign the div for the product in the cart with the correct product ID, so I went through my code throwing in console logs in certain places to see what I was getting and where I had to go from there. I found where I created the div with  let cartProduct = document.createElement('div');  I then found where I was last getting the cart object WITH the product inside of it and grabbing that particular product id and setting it to the cartProduct variable above. I used the .setAttribute() method in javascript to set the "data-id" to the products id like so  `` cartProduct.setAttribute('data-id', productInfo.id);  

Next is where the real "fun" began, when I would hit the minus button of that particular product in the cart I put an event listener on it for click. When that minus button was clicked it'd send a patch request to the update action in the backend, updating the cart info on the backend and rendering a Json response back to the frontend with the information. Here's how the next steps went. 

1) take the cart object from the response and accessing the product information from that cart object use the .find() javascript method to find the product id in the response and compare it to the productInfo id that was assigned to the div for the product in the cart. The find() method returns the value of the first element in the provided array that satisfies the provided testing function. 
2) create a variable divsInCart that would select all of the divs(querySelectorAll) of the products inside of the cart. The Document method querySelectorAll() returns a static (not live) NodeList representing a list of the document's elements that match the specified group of selectors.
3) The next step was to take the nodeList that was returned from using querySelectorAll and convert that into an array that could be iterated on. Using javascripts Array.from() method I created a variable called "divArr" and set it equal to the array.from() passing the divsInCart variable that we used query selector on to convert it to the array. 
4) Next I had to use the .find() method once again to find the id of the div of the product in the cart, and make sure it was equal to the products Id. 
5) Then I grabbed the parent element which in this case was my side navigation div and using the removeChild() method I would pass in the result from step 4.
6) the last and final step was to throw all of this into a conditional statement and it all looked like this : 


``
            .then(updatedCart => {
                let foundProduct = updatedCart.products.find(p => p.id === productInfo.id)
                if (!foundProduct) {
                    let divsInCart = document.querySelectorAll('.product-in-cart');
                    let divArr = Array.from(divsInCart)
                    let foundDiv = divArr.find(div => div.dataset.id == productInfo.id)
                    let parentDiv = document.getElementById('side-nav-id'); 
                    parentDiv.removeChild(foundDiv)
                } 



After an intense debug session this was one of those shoot out of the chair super fast yelling with your hands in the air bug fixes. Considering it was my last major bug to fix until completing my project maybe that had a little bit to do with the celebration but if you havent experienced the big celebration bug fix I promise you that you will some day. 



`


