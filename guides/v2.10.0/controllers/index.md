## Controllers

Controllers behave like a specialized type of Component that is rendered by
the router when entering a Route.

The controller receives a single property from the Route – `model` – which is
the return value of the Route's `model()` method.

To define a Controller, run:

```shell
ember generate controller my-controller-name
```

The value of `my-controller-name` must match the name of the Route that renders
it. So a Route named `blog-post` would have a matching Controller named
`blog-post`.

You only need to generate a Controller if you want to customize its
properties or provide any `actions`. If you have no customizations, Ember will
provide a Controller instance for you at run time.

Let's explore these concepts using an example of a route displaying a blog
post. Presume a `BlogPost` model that is presented in a `blog-post` template.

The `BlogPost` model would have properties like:

* `title`
* `intro`
* `body`
* `author`

Your template would bind to these properties in the `blog-post`
template:

```handlebars {data-filename=app/templates/blog-post.hbs}
<h1>{{model.title}}</h1>
<h2>by {{model.author}}</h2>

<div class="intro">
  {{model.intro}}
</div>
<hr>
<div class="body">
  {{model.body}}
</div>
```

In this simple example, we don't have any display-specific properties
or actions just yet. For now, our controller's `model` property acts as a
pass-through (or "proxy") for the model properties. (Remember that
a controller gets the model it represents from its route handler.)

Let's say we wanted to add a feature that would allow the user to
toggle the display of the body section. To implement this, we would
first modify our template to show the body only if the value of a
new `isExpanded` property is true.

```handlebars {data-filename=app/templates/blog-post.hbs}
<h1>{{model.title}}</h1>
<h2>by {{model.author}}</h2>

<div class='intro'>
  {{model.intro}}
</div>
<hr>

{{#if isExpanded}}
  <button {{action "toggleBody"}}>Hide Body</button>
  <div class="body">
    {{model.body}}
  </div>
{{else}}
  <button {{action "toggleBody"}}>Show Body</button>
{{/if}}
```

You can then define what the action does within the `actions` hook
of the controller, as you would with a component:

```javascript {data-filename=app/controllers/blog-post.js}
import Ember from 'ember';

export default Ember.Controller.extend({
  actions: {
    toggleBody() {
      this.toggleProperty('isExpanded');
    }
  }
});
```
