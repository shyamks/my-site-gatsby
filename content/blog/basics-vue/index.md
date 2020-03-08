---
title: Basics of Vue - Part 1
date: "2020-03-04T22:12:03.284Z"
description: "I started off with vue this week and tracking my progress here."
---

I started off with vue this week and tracking my progress here.

Started the project with the Vue-cli.

```
$ npm i -g vue-cli
$ vue init webpack my-app
```

To run the app
```
$ cd my-app
$ npm run dev

```


Lets start off with the component structure.

A vue file is composed of three parts.

1. **Template** - view of the vue file
2. **Script** - logic of the view
3. **Style** - css of the view 

A template and script has many characteristics to it apart from the html it is composed of. Lets look at some of them

1. Binding using `v-bind` and `v-model`
    
    This is a way through which the template and the script interact with each other.
    ```
    <template>
        <div v-bind:class="classObject"></div> 
        // or
        <div :class="classObject"></div>

        <input v-model="message">
        <p>The input has {{ message }}</p>
    </template>
    ```

    ```
    <script>

    export default {
        data() {
           return {
                message: '',
                classObject: {
                    active: true,
                    'text-danger': false
                }
            }
        }
    }
    
    </script>
    ```

    State variables are returned by the `data()` method.
2. Rendering conditional using `v-if`, `v-else-if`, `v-else`
    ```
    <template>
        <div v-if="cool == me"> Yawss </div> 
        <div v-else-if="cool == you"> Blah</div> 
        <div v-else> Wah wah</div> 
    </template>
    ```

    ```
    <script>
        export default {
        data() {
           return {
                cool: 1,
                me: 2,
                you: 3,
            }
        }
    }
    </script>
    ```

3. Rendering lists using `v-for`
    ```
    <template>
        <div v-for="task in tasks" :key="task.id">
        </div>
    </template>
    ```

    ```
    <script>
        export default {
        data() {
           return {
                tasks: [
                    {
                        id: 1,
                        title: 'Bleh the blah'
                    }
                    {
                        id: 2,
                        title: 'Bleh the blah'
                    }
                ]
            }
        }
    }
    </script>
    ```
4. Event handling using `v-on` and native DOM events

    ```
    <template>
      <button v-on:click="greet">Greet</button>
    </template>
    ```

    ```
    <script>
    export default {
        data() {
            return {
                name: 'Guest'
            }
        }
        methods: {
            greet: function (event) {
                // `this` inside methods points to the Vue instance
                alert('Hello ' + this.name + '!')
                // `event` is the native DOM event
                if (event) {
                    alert(event.target.tagName)
                }
            }
        }
    }
    
    </script>
    ```

So far, in the script part we've covered `data()` which has the state variables and `methods` which has general methods concerning a Vue component. There are other concepts such as `computed`, `watch` and lifecycle hooks.

**Computed**

Any of the complex operations which has to be done on every change of a state variable could be calculated inside `computed`. 

Basic Example
```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```
```
<script>
export default {

  data() {
    return {
        message: 'Hello'
    }
  },
  computed: {
        // a computed getter
        reversedMessage: function () {
            // `this` points to the vm instance
            return this.message.split('').reverse().join('')
        }
    }
}

</script>
```

The advantage of this over a general method is that the **value is cached based on the reactive dependencies**. A computed property will only re-evaluate when some of its reactive dependencies have changed. This means as long as message has not changed, multiple access to the reversedMessage computed property will immediately return the previously computed result without having to run the function again.

**Watch**

`watch` is similar to `computed`, but it is **wrt one state variable whereas `computed` is wrt any of the state variables**.

Example
To get the full name using watch it would be 
```
<script>
  data() {
      return {
        firstName: 'Foo',
        lastName: 'Bar',
        fullName: 'Foo Bar'
    }
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
</script>
```

whereas with `computed`, it would look like 

```
<script>
  data(){
      return {
        firstName: 'Foo',
        lastName: 'Bar'
    }
  } ,
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
</script>
```

Communication between components, transitions are concepts which I will be exploring next.   

