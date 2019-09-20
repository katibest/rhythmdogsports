---
layout: post
post_author: Dustin Kocher
current_gaslighter: false
categories:
- Development
tags:
- Web Hooks
- React
post_title: React Hooks at a Glance
publish_date: 2013-08-07 13:30:00 +0000
feature_post_image: ''
post_images: []
slug: react-hooks-at-a-glance
permalink: /blog/:slug
---

## Why Hooks?

Why should you use hooks? What benefits do they provide?  
They give you the ability to connect and reuse stateful logic, removing the need for render props and higher-order components. * They help take the logic that is spread out among the many lifecycle methods of a complex component and relocate it into smaller independent functions. * They reduce confusion on when or why you need a component to be a class versus a function.

## useState at a Glance

The useState hook allows you to give state to a functional component - meaning you will no longer need to worry about making a class component if you want to have state in a functional component. Let’s look at a small example and explain what is going on.

    import React, { useState } from "react";
    import _ from "lodash";

    export default function Tasks(props) {
      const [newTask, setNewTask] = useState();
      const [tasks, setTasks] = useState([]);

      function addTask() {
        setTasks([...tasks, newTask]);
        setNewTask("");
      }

      return (
        <div>
          <input
            type="text"
            value={newTask}
            onChange={e => setNewTask(e.target.value)}
          />
          <button onClick={addTask}>Add</button>
          <div>
            {_.map(tasks, task => (
              <div>{task}</div>
            ))}
          </div>
        </div>
      );
    }


In this example we are making a task list. The user types in a task, clicks the Add button, and a task is added to the task list. So what is going on with the react hook `**useState**`?

* Every time `**useState()**` is called two things are returned. A new state variable and a function to update the state variable. As you can see `**const \[newTask, setNewTask\] = useState()**` creates a new state variable and using array deconstruction we call the new state variable `**newTask**` and the function to update this variable `**setNewTask**`
* You can give a state variable an initial value by passing in an argument to `**useState**`. On the following line we give the state variable `**tasks**` an initial value of an empty array, `**const \[tasks, setTasks\] = useState(\[\])**`. Here it is an array but it can be anything, a string, integer, or an object.
* To use or display the state variables all you need to do is call the state variables `**tasks**` or `**newTask**` directly.
* To update a state variable let’s look at the `**addTask()**` function. Inside this function you can see we are updating both the `**tasks**` state variable and `**newTasks**` state variable. In both examples you can see we just pass in a new value to the update function and react will re-render the component with the new value. One thing to point out is you will want to follow the same rules you always have of not mutating the state variable directly but recreating it and adding to it. As you can see in `**setTasks(\[...tasks, newTask\])**` we create a new array by deconstructing the `**tasks**` state variable and append the `**newTask**` state variable to the end.


## useEffect at a Glance

The `**useEffect**` hook combines several of the most commonly used lifecycle hooks (`**componentDidMount**`, `**componentDidUpdate**`, and `**componentWillUnmount**`) used in classes into one central API. Now let’s add on to the previous example a `**useEffect**`hook:

    import React, { useState, useEffect } from "react";
    import _ from "lodash";
    import axios from "axios";

    export default function Tasks(props) {
      const [newTask, setNewTask] = useState();
      const [tasks, setTasks] = useState([]);

      useEffect(() => {
        axios({
          method: "get",
          url: "https://api.jsonbin.io/b/5c6af0c09dfbb91d718215d8",
          params: {}
        }).then(response => {
          setTasks(response.data.tasks);
        });

        /*return function cleanup() {
          //put cleanup logic here
        }*/
      }, []);

      ...

    }


This uses the `**axios**` package to asynchronously fetch tasks and then set the state variable `**tasks**` to the returned list of tasks. So what should you be looking at in the `**useEffect**` hook?

* The first thing to notice is that `**useEffect**` is called inside of the component. This gives it access to everything that is declared in the component including the state variables created by the `**useState**` hook or any other variables generated from hooks such as `**useReducer**` or `**useContext**`. To be clear, you will not have the ability to call any of these hooks directly in `**useEffect**`. For example you can’t do `**const \[var, func\] = useState()**` inside of the `**useEffect**` hook. You can only call hooks directly inside the function component.
* `**useEffect**` takes two arguments: a function and an array.
* The function is what will be called when the `**useEffect**` hook is called. Here we are calling out to an API endpoint to retrieve a list of tasks. As you see commented out you can return a function to perform cleanup logic. This is great if for example you are dealing with pub/sub connections and you need to clean up those connections.
* The second is an optional parameter which is an array. This tells the `**useEffect**`hook when it should be called. If nothing is passed in here the `**useEffect**` hook will be called on every render. Passing in an empty array will only run this hook on mount and unmount. Passing in an array with the state variable, for example `**tasks**`, will call the hook every time it is updated.
* You can have as many `**useEffect**` hooks in a component as needed. Ideally you should create one for each set of related pieces of logic that you need. For example I would create a new `**useEffect**` in the scenario were I wanted to update the API endpoint every time `**tasks**` is updated.

## Conclusion

React has done a lot through the years to improve itself. I hope this gives you some insight into how to use hooks and the value they bring. I personally feel on top of the benefits previously mentioned, this will help in making code easier to read but also the logic of a component and its lifecycles easier to follow.
