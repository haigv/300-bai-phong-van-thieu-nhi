# 300 bài phỏng vấn thiếu nhi
List of VueJS Interview Questions

| No. | Questions |
|---- | ---------
|1  | [What is VueJS](#what-is-vuejs) |
|2  | [What are the major features of VueJS](#what-are-the-major-features-of-vuejs) |
|3  | [What are the lifecycle methods of VueJS](#what-are-the-lifecycle-methods-of-vuejs)|
|4  | [What are the conditional directives](#what-are-the-conditional-directives)|
|5  | [What is the difference between v-show and v-if directives](#what-is-the-difference-between-v-show-and-v-if-directives)|
|6  | [What is the purpose of v-for directive?](#what-is-the-purpose-of-v-for-directive)|
|7  | [How do you reuse elements with key attribute?](#how-do-you-reuse-elements-with-key-attribute)|
|8 | [Why should not use if and for directives together on the same element?](#why-should-not-use-if-and-for-directives-together-on-the-same-element)|
|9 | [Why do you need to use key attribute on for directive?](#why-do-you-need-to-use-key-attribute-on-for-directive)|
|10 | [How do you use for directive with a range?](#how-do-you-use-for-directive-with-a-range)|
|11 | [How do you implement two way binding?](#how-do-you-implement-two-way-binding)|
|12 | [How do you communicate from child to parent using events?](#how-do-you-communicate-from-child-to-parent-using-events)|
|13 | [What are slots?](#what-are-slots)|
|14 | [What are possible prop types?](#what-are-possible-prop-types)|


1.  ### What is VueJS?
    **Vue.js**  (pronounced /vjuː/, like view not /vuuu/) is a progressive framework not a lib.

    **[⬆ Back to Top](#table-of-contents)**

2.  ### What are the major features of VueJS?
    Below are the some of major features available with VueJS
    1. **Virtual DOM:** It uses virtual DOM similar to other existing frameworks such as ReactJS, Ember etc. Virtual DOM is a light-weight in-memory tree representation of the original HTML DOM and updated without affecting the original DOM.
    2. **Components:** Used to create reusable custom elements in VueJS applications.
    3. **Templates:** VueJS provides HTML based templates that bind the DOM with the Vue instance data
    4. **Light weight:** VueJS is light weight library compared to other frameworks.

    **[⬆ Back to Top](#table-of-contents)**

3.  ### What are the lifecycle methods of VueJS?
    Lifecycle hooks are a window into how the library you’re using works behind-the-scenes. By using these hooks, you will know when your component is created, added to the DOM, updated, or destroyed. Let's look at lifecycle diagram before going to each lifecycle hook in detail,

    <img src="https://github.com/haigv/300-bai-phong-van-thieu-nhi/blob/main/vuelifecycle.png" width="400" height="800">

    1. **Creation(Initialization):**
        Creation Hooks allow you to perform actions before your component has even been added to the DOM. You need to use these hooks if you need to set things up in your component both during client rendering and server rendering. Unlike other hooks, creation hooks are also run during server-side rendering.
        1. **beforeCreate:**
           This hook runs at the very initialization of your component. hook observes data and initialization events in your component. Here, data is still not reactive and events that occur during the component’s lifecycle have not been set up yet.
        ```javascript
            new Vue({
              data: {
               count: 10
              },
              beforeCreate: function () {
                console.log('Nothing gets called at this moment')
                // `this` points to the view model instance
                console.log('count is ' + this.count);
              }
            })
               // count is undefined
         ```
        2. created:
            This hook is invoked when Vue has set up events and data observation. Here, events are active and access to reactive data is enabled though templates have not yet been mounted or rendered.
        ```javascript
          new Vue({
            data: {
             count: 10
            },
            created: function () {
              // `this` points to the view model instance
              console.log('count is: ' + this.count)
            }
          })
             // count is: 10
        ```
        **Note:** Remember that, You will not have access to the DOM or the target mounting element (this.$el) inside of creation hooks
    2. **Mounting(DOM Insertion):**
        Mounting hooks are often the most-used hooks and they allow you to access your component immediately before and after the first render.
        1. beforeMount:
            The beforeMount allows you to access your component immediately before and after the first render.
        ```javascript
          new Vue({
            beforeMount: function () {
              // `this` points to the view model instance
              console.log(`this.$el is yet to be created`);
            }
          })
        ```
        2. mounted:
            This is a most used hook and you will have full access to the reactive component, templates, and rendered DOM (via. this.$el).  The most frequently used patterns are fetching data for your component.
        ```javascript
        <div id="app">
            <p>I’m text inside the component.</p>
        </div>
          new Vue({
            el: ‘#app’,
            mounted: function() {
              console.log(this.$el.textContent); // I'm text inside the component.
            }
          })
        ```
    3. **Updating (Diff & Re-render):**
        Updating hooks are called whenever a reactive property used by your component changes, or something else causes it to re-render
        1. beforeUpdate:
        The beforeUpdate hook runs after data changes on your component and the update cycle begins, right before the DOM is patched and re-rendered.
        ```javascript
        <div id="app">
          <p>{{counter}}</p>
        </div>
        ...// rest of the code
          new Vue({
            el: '#app',
            data() {
              return {
                counter: 0
              }
            },
             created: function() {
              setInterval(() => {
                this.counter++
              }, 1000)
            },

            beforeUpdate: function() {
              console.log(this.counter) // Logs the counter value every second, before the DOM updates.
            }
          })
        ```
        2. updated:
            This hook runs after data changes on your component and the DOM re-renders.
        ```javascript
        <div id="app">
          <p ref="dom">{{counter}}</p>
        </div>
        ...//
          new Vue({
            el: '#app',
            data() {
              return {
                counter: 0
              }
            },
             created: function() {
              setInterval(() => {
                this.counter++
              }, 1000)
            },
            updated: function() {
              console.log(+this.$refs['dom'].textContent === this.counter) // Logs true every second
            }
          })
        ```
    4. **Destruction (Teardown):**
        Destruction hooks allow you to perform actions when your component is destroyed, such as cleanup or analytics sending.
        1. beforeDestroy:
        `beforeDestroy` is fired right before teardown. If you need to cleanup events or reactive subscriptions, beforeDestroy would probably be the time to do it. Your component will still be fully present and functional.
        ```javascript
        new Vue ({
          data() {
            return {
              message: 'Welcome VueJS developers'
            }
          },

          beforeDestroy: function() {
            this.message = null
            delete this.message
          }
        })
        ```
        2. destroyed:
        This hooks is called after your component has been destroyed, its directives have been unbound and its event listeners have been removed.
        ```javascript
        new Vue ({
            destroyed: function() {
              console.log(this) // Nothing to show here
            }
          })
        ```

    **[⬆ Back to Top](#table-of-contents)**

4.  ### What are the conditional directives?
    VueJS provides set of directives to show or hide elements based on conditions. The available directives are: **v-if, v-else, v-else-if and v-show**
    
    **1. v-if:**  The v-if directive adds or removes DOM elements based on the given expression. For example, the below button will not show if isLoggedIn is set to false.
    ```javascript
    <button v-if="isLoggedIn">Logout</button>
    ```
    You can also control multiple elements with a single v-if statement by wrapping all the elements in a `<template>` element with the condition. For example, you can have both label and button together conditionally applied,
    ```javascript
    <template v-if="isLoggedIn">
      <label> Logout </button>
      <button> Logout </button>
    </template>
    ```
    **2. v-else:**  This directive is used to display content only when the expression adjacent v-if resolves to false. This is similar to else block in any programming language to display alternative content and it is preceded by v-if or v-else-if block. You don't need to pass any value to this.
    For example, v-else is used to display LogIn button if isLoggedIn is set to false(not logged in).
    ```javascript
    <button v-if="isLoggedIn"> Logout </button>
    <button v-else> Log In </button>
    ```
    **3. v-else-if:** This directive is used when we need more than two options to be checked.
    For example, we want to display some text instead of LogIn button when ifLoginDisabled property is set to true. This can be achieved through v-else statement.
    ```javascript
    <button v-if="isLoggedIn"> Logout </button>
    <label v-else-if="isLoginDisabled"> User login disabled </label>
    <button v-else> Log In </button>
    ```

    **4. v-show:** This directive is similar to v-if but it renders all elements to the DOM and then uses the CSS display property to show/hide elements. This directive is recommended if the elements are switched on and off frequently.
    ```javascript
    <span v-show="user.name">Welcome user,{{user.name}}</span>
    ```

    **[⬆ Back to Top](#table-of-contents)**

5.  ### What is the difference between v-show and v-if directives?
    Below are some of the main differences between between **v-show** and **v-if** directives,

    1. v-if only renders the element to the DOM if the expression passes whereas v-show renders all elements to the DOM and then uses the CSS display property to show/hide elements based on expression.
    2. v-if supports v-else and v-else-if directives whereas v-show doesn't support else directives.
    3. v-if has higher toggle costs while v-show has higher initial render costs. i.e, v-show has a performance advantage if the elements are switched on and off frequently, while the v-if has the advantage when it comes to initial render time.
    4. v-if supports `<template>` tab but v-show doesn't support.

    **[⬆ Back to Top](#table-of-contents)**

6.  ### What is the purpose of v-for directive?
    The built-in v-for directive allows us to loop through items in an array or object. You can iterate on each element in the array or object.
    1. **Array usage:**
    ```javascript
    <ul id="list">
      <li v-for="(item, index) in items">
        {{ index }} - {{ item.message }}
      </li>
    </ul>

    var vm = new Vue({
      el: '#list',
      data: {
        items: [
          { message: 'John' },
          { message: 'Locke' }
        ]
      }
    })
    ```
    You can also use `of` as the delimiter instead of `in`, similar to javascript iterators.

    2. **Object usage:**
    ```javascript
    <div id="object">
      <div v-for="(value, key, index) of user">
        {{ index }}. {{ key }}: {{ value }}
      </div>
    </div>

    var vm = new Vue({
      el: '#object',
      data: {
        user: {
          firstName: 'John',
          lastName: 'Locke',
          age: 30
        }
      }
    })
    ```

    **[⬆ Back to Top](#table-of-contents)**


7.  ### How do you reuse elements with key attribute?
    Vue always tries to render elements as efficient as possible. So it tries to reuse the elements instead of building them from scratch. But this behavior may cause problems in few scenarios.

    For example, if you try to render the same input element in both `v-if` and `v-else` blocks then it holds the previous value as below,
    ```javascript
    <template v-if="loginType === 'Admin'">
      <label>Admin</label>
      <input placeholder="Enter your ID">
    </template>
    <template v-else>
      <label>Guest</label>
      <input placeholder="Enter your name">
    </template>
    ```
    In this case, it shouldn't reuse. We can make both input elements as separate by applying **key** attribute as below,
    ```javascript
        <template v-if="loginType === 'Admin'">
          <label>Admin</label>
          <input placeholder="Enter your ID" key="admin-id">
        </template>
        <template v-else>
          <label>Guest</label>
          <input placeholder="Enter your name" key="user-name">
        </template>
    ```
    The above code make sure both inputs are independent and doesn't impact each other.

    **[⬆ Back to Top](#table-of-contents)**

8. ### Why should not use if and for directives together on the same element?
    It is recommended not to use v-if on the same element as v-for. Because v-for directive has a higher priority than v-if.

    There are two cases where developers try to use this combination,

     1. To filter items in a list

       For example, if you try to filter the list using v-if tag,

       ```javascript
         <ul>
           <li
             v-for="user in users"
             v-if="user.isActive"
             :key="user.id"
           >
             {{ user.name }}
           <li>
         </ul>
       ```
       This can be avoided by preparing the filtered list using computed property on the initial list
       ```javascript
         computed: {
           activeUsers: function () {
             return this.users.filter(function (user) {
               return user.isActive
             })
           }
         }
         ...... //
         ...... //
         <ul>
           <li
             v-for="user in activeUsers"
             :key="user.id">
             {{ user.name }}
           <li>
         </ul>
       ```
     2. To avoid rendering a list if it should be hidden

       For example, if you try to conditionally check if the user is to be shown or hidden

       ```javascript
         <ul>
           <li
             v-for="user in users"
             v-if="shouldShowUsers"
             :key="user.id"
           >
             {{ user.name }}
           <li>
         </ul>
       ```
       This can be solved by moving the condition to a parent by avoiding this check for each user
       ```javascript
         <ul v-if="shouldShowUsers">
           <li
             v-for="user in users"
             :key="user.id"
           >
             {{ user.name }}
           <li>
         </ul>
       ```

     **[⬆ Back to Top](#table-of-contents)**

9.  ### Why do you need to use key attribute on for directive?
     In order to track each node’s identity, and thus reuse and reorder existing elements, you need to provide a unique `key` attribute for each item with in `v-for` iteration. An ideal value for key would be the unique id of each item.

     Let us take an example usage,
     ```javascript
     <div v-for="item in items" :key="item.id">
       {{item.name}}
     </div>
     ```
     Hence, It is always recommended to provide a key with v-for whenever possible, unless the iterated DOM content is simple.

     **Note:** You shouldn’t use non-primitive values like objects and arrays as v-for keys. Use string or numeric values instead.

     **[⬆ Back to Top](#table-of-contents)**

10.  ### How do you use v-for directive with a range?
     You can also use integer type(say 'n') for `v-for` directive which repeats the element many times.
     ```javascript
     <div>
       <span v-for="n in 20">{{ n }} </span>
     </div>
     ```
     It displays the number 1 to 20.

     **[⬆ Back to Top](#table-of-contents)**

11.  ### How do you implement two-way binding?
     You can use the `v-model` directive to create two-way data bindings on form input, textarea, and select elements.

     Lets take an example of it using input component,
     ```vue
     <input v-model="message" placeholder="Enter input here">
     <p>The message is: {{ message }}</p>
     ```
     Remember, v-model will ignore the initial `value`, `checked` or `selected` attributes found on any form elements. So it always use the Vue instance data as the source of truth.

     **[⬆ Back to Top](#table-of-contents)**

12.  ### How do you communicate from child to parent using events?
     If you want child wants to communicate back up to the parent, then emit an event from child using `$emit` object to parent,
     ```javascript
     Vue.component('todo-item', {
       props: ['todo'],
       template: `
         <div class="todo-item">
           <h3>{{ todo.title }}</h3>
           <button v-on:click="$emit('increment-count', 1)">
             Add
           </button>
           <div v-html="todo.description"></div>
         </div>
       `
     })
     ```
     Now you can use this todo-item in parent component to access the count value.
     ```vue
     <ul v-for="todo in todos">
       <li>
         <todo-item
           v-bind:key="todo.id"
           v-bind:todo="todo"
           v-on:increment-count="total += 1"
         /></todo-item>
       </li>
     </ul>
     <span> Total todos count is {{total}}</span>
     ```

     **[⬆ Back to Top](#table-of-contents)**

13.  ### What are slots?
     Vue implements a content distribution API using the <slot> element to serve as distribution outlets for content created after after the current Web Components spec draft.

     Let's create an alert component with slots for content insertion,
     ```javascript
     Vue.component('alert', {
       template: `
         <div class="alert-box">
           <strong>Error!</strong>
           <slot></slot>
         </div>
       `
     })
     ```
     Now you can insert dynamic content as below,
     ```vue
     <alert>
       There is an issue with in application.
     </alert>
     ```

     **[⬆ Back to Top](#table-of-contents)**


14.  ### What are possible prop types?
     You can declare props with type or without type. But it is recommended to have prop types because it provides the documentation for the component and warns the developer for any incorrect data type being assigned.
     ```javascript
     props: {
       name: String,
       age: Number,
       isAuthenticated: Boolean,
       phoneNumbers: Array,
       address: Object
     }
     ```
     As mentioned in the above code snippet, you can list props as an object, where the properties’ names and values contain the prop names and types, respectively.

     **[⬆ Back to Top](#table-of-contents)**

