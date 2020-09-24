---
layout: post
title:      "Redux & Redux Flow"
date:       2020-09-24 23:07:09 +0000
permalink:  redux_and_redux_flow
---


As an aspiring frontend engineer I have tried to make it a point to really understand Frontend languages and frameowrks such as Javascript, React & Redux. If you haven't gotten to the client side curriculum yet these terms may seem like a foreign langauge. Javascript is pretty well known at this point so ill briefly touch on what React is then get on to explaining redux more thoroughly. React is an open source Javascript library or framework for building UI interfaces or UI components. With React there is a built in object known as "state", the state object is something that can be stored and kept track of that belong to a particular component. As your application grows and you need to be keeping track of more and more is where Redux comes in handy. 

What is redux? Redux is also an open source Javascript library or framework and was built solely for managing state in your application. With redux comes what is known as "the store". The store is where you're storing all of your state. I like to think of it as a grocery store, you can go to the grocery store and get the groceries you need to eat where as the Redux store you have access to whatever piece of state you need to use in any component. 

The flow of how exactly all this works seems simple on the surface but was actually pretty confusing for me until seeing it working a couple times in my final project. To change the state in our application the flow kind of goes like this..

1) We have an action creator that...
2) Produces an action(think of it like an object in javascript), the action gets fed to Dispatch
3) Dispatch(function) does just like it sounds it dispatches the action and forwards the action to our Reducer 
4) Our Reducer(function) determines and creates our new state. 


Here is an example step by step in code how the flow works, 

Step 1 & 2) - Action Creator & Dispatch the action
export const getPcas = () => { 
    
    return(dispatch) => {
        return fetch('http://localhost:3000/pcas')
        .then(resp => resp.json()) 
        .then(resp => {
            dispatch({ type: 'GET_PCAS', payload: resp})
        })
    }
}

Dispatch has two properties on it, type, and payload. Type is what type of object it is, in this scenario ^ GET_PCAS is fetching my index endpoint to get all of my salesman and render it back to the frontend. The payload property is what is bundled into your action object and passed around your reducer. Here, it will be all of my salesman (pcas). 

Step 3 & 4 - Action forwarded to our Reducer, and Create new state
export default ( state = [], action) => {
    switch(action.type) {
        case 'GET_PCAS':
        return [...state, action.payload]
        case 'GET_PCAS_GROWERS_CROPS': 
        return action.payload
        default:
            return state
    }
}

This an example of a reducer, as you can see in the first line state = [], I am setting the default state to an empty array. Using a switch statement in case the type is 'GET_PCAS', we are going to first use the spread operator to copy our original state so we dont mutate the state directly (that is a NO-NO in redux) and gives us the ability to add or overwrite onto that state. So when there is a type of 'GET_PCAS' we copy the oroginal state, and add onto it with the our payload property. Which then populates the array with all of the salesman and in my case could iterate over it and use it and display it in my web app how i wanted. 


