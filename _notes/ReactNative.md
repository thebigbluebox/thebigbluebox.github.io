---
title:  "React Native, first iOS framework"
date:   2016-6-22 9:36:00
description: Wow this is just like node!
---

# React Native Notes

## Core Components
Building blocks of React Native application, each component 

## Connecting to the data layer
React uses a MVC app architecture, but essentially rethinks the overall MVC pattern into something else

### MVC Model
* The model is the data
* The view is how the data is presented in an app
* The controller provides the logic for handling data within the app

However react lets you create multiple components which are combined to form a view but each component can also handle the logic that a controller might have provided
```
class Example extends React.Component {
    render() {
        // Code that renders the view of the existing data, and
        // potentially a form to trigger changes to that data through
        // the handleSubmit function
    }

    handleSubmit(e) {
        // Code that modifies the data, like a controller's logic
    }
};
```
Each component in a react app is also aware of two different types of data have different roles
* _props_ is data which is passed into a component when it is created, used as the 'options'  which you want for any given component. If you had a button component, an example prop would be the text of the button
A component can't change its props (ie. they're immutable)
* _state_ is data which can be changed over time by an component. If the button example above was a login/logout button then the state would store the current login status of the user and the button would be able to access this, and modify if it they clicked on the button to change their status

To reduce repetition, state -> owned by the highest parent component in the app's comp hierachy AKA _container components_
Meaning you won't put the state in the example button's state within the button component but it will exist in the parent view which contains the button and then use props to pass relevant parts of the state down to child components.
Data only flow one way -> Downwards through any given app.

## Storing data
We are using the Flux Architecture which is something facebook made to make application syncing easier
Flux exapnds on React's data relationship by introducing the concept of Stores, containers for app's state objects and a new workflow for modifying the state over time
* Store in a Flux app has a callback func which is registered with a Dispatcher
* Views (similar to React components) can trigger Actions [An object that has data on thing that just happend... it could include new data that was input into a form in the app] as well as an action type [a constant that describes the type of Action being performed]
* The action is then sent to the dispatcher
* The dispatcher propagates this action to all the various registered store callbacks
* If t he store can tell that it was affected by an action (because the action type is related to its data) it will update itself and therefore the contained state. Once updated it'll emit a change event 
* Special Views called _controller_ views (container components) will be listening for these change events and when they get one, they know they should fetch the new Store data
* Once fetched they call _setState()_ with the new data, causing the component inside the View to re-render

Since we are using Redux in the mock F8 application, the website also talks about this

Redux is a framework implementation of the Flux architecture but it also strips it down.
_There is no dispatcher in Redux, and there is only one store for the state of the entire app_

* React components can cause Actions to be triggered, for example by a button click
* Actions are an object (containing a type label and other data related to the Action) sent to the Store via the dispatch functiona
* The store then takes the action payload and sends it to the Reducers along with the current state tree (object that contains all state data in a particular structure)
* A reducer is a pure function that takes the previous state and an Action, then returns a new state based on any changes that the action might have indicated. A redux app can have one reducer, but most app will end up using several that each handle a different part of the state
* The store recieves this new state and replaces the current one with it. _When a state is updated, it is technically replaced_
* When the state changes, the Store triggers a change event
* Any react component that have subscribed to the change event make a call to retrieve the new state from the store
* The components are updated with the new state

Store is only concerned with holding the state, compoonents in the view are only concerned with displaying data and triggering Actions
Actions are only concerned with indicating that something in the state has changed and including data aobut what it was
Reducers are only concerned with combining an old state and mutating Actions into a new state

* Actions are the only way to trigger a state change, this centralizes this process away from the UI components, and because they are properly ordered by the Reducer it prevents race conditions
* state becomes essentially immutable and a series of states each representing an individual change are created this gives you a clear and easily traceable state history within your app

# Building the data layer
F8 uses a middle ware to perform persistant storage using Redux Persist

```
var middlewareWrapper = applyMiddleware(...);
var createF8Store = middlewareWrapper(createStore(reducers));

function configureStore(onComplete: ?() => void) {
  const rehydrator = autoRehydrate();
  const store = rehydrator(createF8Store);
  persistStore(store, {storage: AsyncStorage}, onComplete);
  ...
  return store;
}
```

_applyMiddleware()_ returns a function that will ehnace the store object
so we wrap Redux's createStore() function in the enhancer "applyMiddleWare", and the resulting enchanced store object is saved in createF8Store.