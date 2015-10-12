# MVC (Model-View-Controller) Pattern

MVC is an architectural pattern that encourages improved application organization through a separation of concerns. In enforces the isolation of data (Model) from the user interfaces (Views), with a third component (Controllers), traditionally managing logic and user-input. Separating concerns inside an application is a very important idea. Our application needs to change as frequently as we change our socks: at least once a year.

Layering an application and maintaining modularity reduces the impact of the change. The less each layer knows about the other layers, the better. The pattern was suggested and designed by Trygve Reenskaug in the late 1970s during his time working on Smalltalk-80. It was not until 1988 that the pattern was documented in an article *[A cookbook for using the model-view-controller user interface paradigm](http://www.researchgate.net/publication/248825145_A_cookbook_for_using_the_model_-_view_controller_user_interface_paradigm_in_Smalltalk_-_80)*.

The MVC pattern is implemented in wide variety of JavaScript frameworks, allowing developers to easily add structure to their applications without great effort. These frameworks include AngularJS, Backbone, Ember.js etc. The importance of avoiding spaghetti code (code that is very difficult to read and maintain due to lack of structure) it's important for the modern JavaScript developer to understand what this pattern provides.

MVC is composed of three core components:

## Models

Models manage the data for an application. In other words, it's where the application's data objects are stored. They are concerned with neither the user-interface nor presentation layer. They represent forms of data that an application may require. In many applications the model is contained in some form of database. Ideally, the model is the only mutable (changable) part of the pattern.

The model is usually modeled as a simple container for information. Typically, there are no real functions in the model. It simply contains fields and may also may contain validation.

So rather than this:

```js
var user = users['bar'];
deleteUser(user);
```

We can do this:

```js
var user = User.find('bar');
user.delete();
```

In the first example, `deleteUser` is in the Global scope and global variables and functions should be kept to an absolute minimum. In the second example, the `delete()` function is namespaced behind User instances, as are all stored records. This way we are keeping the global variables to a minimum, exposing fewer areas to potential conflicts.

When using models in real-world applications we want our models to be persistent. Persistence allows us to edit and update models with knowledge that its most recent state will be save either in memory, in a localStorage or synchronized with a database.

## Views

Views are a visual representation of models that present a filtered view of their current state. JavaScipt views are about building and maintaining a DOM element. Apart form single conditional statement in templates, the views shouldn't contain any logic. Like models, views also should be decoupled from the rest of the application. Views shouldn't know anything about controllers and models. Mixing up views and logic is one of the surest paths of disaster.

A view typically observes a model and is notified when the model changes, allowing the view to update itself accordingly. Users are able to interact with the views and read and edit the models. As the view is the presentation layer, we generally present the ability to edit and update in a user-friendly fashion.

One way of creating views is via JavaScript. You can create DOM elements using `document.createElement()`, setting their contents and appending them to the page. When it's time to redraw the view, just empty the view and repeat the process:

```js
var views = document.getElementById('views');
views.innerHTML = ''; // Empty the element

var container = document.createElement('div');
container.id = 'user';

var name = document.createElement('span');
name.innerHTML = data.name;

container.appendChild(name);
views.appendChild(container);
```

### Templating

JavaScript templates (such as Mustache and Handebars.js) are often used to define templates for views as markup (stored externally or within a script tags with a custom type text/template) containing template variables. Variables may be used with a special variable syntax (example {{name}}) and frameworks are typically smart enough to accept data in a JSON form such that we only need to be concerned with maintaining clean modules and clean templates.

Here is a simple example of templating using Handebars.js

```html
<li class="user">
    <h2>{{name}}</h2>
    <img class="personal photo" src="{{src}}" />
    <div class="personal-data">
        {{personaldata}}
    </div>
</li>
```

It's important to remember that **templates are not view**. A template _might_ a declarative way to specify part of even a whole view object so that it can be generated from the template specification.

To summarize, views are a visual representation of our application data.

## Controllers

Controllers are the glue between views and models. Controllers receive events and input from views, process them and update the views. The controller will add event listeners to views when the page loads such as detecting when a form is submitted or buttons are clicked. Then when the user interacts with your application, the events trigger actions inside the controllers.

Here is a simple example using jQuery:

```js
var Controller = {};

(Controller.users = function($) {
    var nameClick = function() {
        /* ... */
    };

    // Attach event listeners on page load
    $(function() {
        $("#view .name").click(nameClick);
    })
}(jQuery));
```
We are creating a `users` Controller that is namespaced under the Controller variable. Then, we're using an anonymous function to encapsulate scope, preventing variable pollution of the global scope. When the page loads, we are adding the click event listened to a view element.


# What MCV give us

The separation of concerns in MVC facilitates simples modularization of an application's functionality and enables:

- easier maintenance
- decoupling models and views means that it is significantly more straight-forward to write unit tests
- duplication of model and controller code is minimize
