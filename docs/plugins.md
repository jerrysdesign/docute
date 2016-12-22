---
search: english
---

# Plugins

Plugins give you extra features without bloating docute itself.

<p class="warning">
  All plugins should be load before `config.js`!
</p>

## List of Plugins

### DocSearch

DocSearch is a free service from [algolia.com](http://algolia.com), it might be the easiest way to add search to your documentation.

To use this plugin, [apply for the program](https://community.algolia.com/docsearch/) to get an API key and indexName first. 

Then simply load `docsearch.js` before all other scripts:

```html
<script src="https://unpkg.com/docute/plugins/docsearch.js"></script>
<!-- put every other script behind -->
<script src="config.js"></script>
<script src="https://unpkg.com/docute/dist/docute.js"></script>
```

Finally configure it in your config.js:

```js
self.$config = {
  // the docsearch variable is provided by docsearch plugin
  plugins: [
    docsearch({
      apiKey: 'You API Key',
      indexName: 'Your Index Name',
      // algolia docsearch allows you to search with tag filter
      tags: ['english', 'zh-Hans', 'zh-Hant']
    })
  ],
  // this plugin does require a url too
  // it should be the url of your website, without / at the end
  url: 'https://docute.js.org'
}
```

You can check out the [algolia docsearch config](https://github.com/algolia/docsearch-configs/blob/master/configs/docute.json) for docute.js.org, we have `english` `zh-Hans` `zh-Hant` tags for now. By default the search bar in your doc will search all tags, but you may only allow it to search relevant tag for specified page. For example, search `english` tag on English page only.

Then use front-matter to do this:

```markdown
---
search: english
---
Now, search on this page will only give you results which is tagged by `english`
```

The `search` could also be an array so that it can search multiple tags:

```markdown
---
search: 
  - zh-Hans
  - zh-Hant
---
Easy?
```

### Disqus

Integrate Disqus to your docs, it will show after the main content.

First include the plugin via `script` tag:

```html
<script src="https://unpkg.com/docute/plugins/disqus.js"></script>
```

Add configure it in `config.js`:

```js
self.$config = {
  plugins: [
    disqus({
      shortname: 'SHORT_NAME' // replace this with your own shortname
    })
  ]
}
```

We may add `inline comment` support in the future, just like the way medium.com did.

## Write A Plugin

A plugin is simply a function that takes `context` as arguments.

```js
// config.js
self.$config = {
  plugins: [
    function myPlugin(context) {
      // context.store
      // context.router
      // context....
    }
  ]
}
```

If your plugin needs options, use a higher-order function instead:

```js
self.$config = {
  plugins: [
    myPlugin(options)
  ]
}

function myPlugin(options) {
  return function (context) {
    // context.store
    // context.router
  }
}
```

<p class="tip">
  You can also access `window.fetch` here without any polyfill, we already did that for you!
</p>

### Context

The argument `context` has following properties:

- store: the vuex store instance
- router: the vue-router instance
- registerComponent: register a component to a specific position of the document

#### registerComponent

The function requires two arguments: `registerComponent(position, component)`

##### position

Value: `oneOf(['content:start', 'content:end'])`

##### component

Type: `VueComponent`

Any Vue component can be accepted as component argument.