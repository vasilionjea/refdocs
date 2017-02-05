# Ember Addons

This is a guide for developing a fictional `ember-loading-button` addon. During development we can test the addon 
from the generated dummy app. As we develop it, we can also test it from an actual Ember application by following 
a standard process of creating _symbolic_ links and including the addon's vendor and bower dependencies to the 
consuming Ember application.

### Create the addon
`ember addon ember-loading-button`

### Add a bower dependency
We're pretending that our fictional addon depends on _moment.js_:

1. `bower install moment --save` - adds the dependency to `bower.json` and installs the files under `bower-components/` folder.
2. Shim dependency for ES6 imports: `ember g vendor-shim moment`

### Import vendor/bower dependencies in dummy app's build
```js
// ember-cli-build.js
app.import('bower_components/moment/moment.js');
app.import('vendor/shims/moment.js');
```

### Generate button component
`ember g component loading-button`

### Use component in dummy app
1. In the dummy _application.hbs_ add `{{#loading-button}}Click here{{/loading-button}}`.
2. Remove `ember-welcome-page` from _package.json_ anad remove the `{{welcome-page}}` component from the dummy's _application.hbs_ template.
3. Run `ember s`


# Existing app
While an addon is being developed, it can be tested with an actual existing Ember application. The basic process involves two main steps:

1. Creating symbolic links between the consuming app's `node_modules/` directory and the addon's `node_modules/` directory.
2. Installing the addon's Bower dependencies in the consuming application.

### Link the addon and the app
1. Run `npm link` from the root directory of the addon in order to put a _symbolic link_ under your global `node_modules/` directory. The global _node_modules/_ directory is located in `/usr/local/lib/node_modules/`.
  * Link result: `/usr/local/lib/node_modules/ember-loading-button -> /Users/YOU/workspace/ember-loading-button`
2. Run `npm link <addon-name>` from the consuming app's root directory to create another symbolic link in the app's `node_modules` folder that points to your addon's `node_modules` folder.
  * Add the addon as a dependency to the consuming app's _package.json_ file: `"ember-loading-button": "*"`
  * Link result: `/Users/YOU/workspace/addon-consumer/node_modules/ember-loading-button -> /usr/local/lib/node_modules/ember-loading-button -> /Users/YOU/workspace/ember-loading-button`

### Install addon's bower dependencies in the consuming app
Installing your addon's Bower dependencies in the consuming application is done via a _default Blueprint_ in the `afterInstall` hook. This actually brings in the addon's `bower_components/` to the consuming app's `bower_components/`.

1. Create the default blueprint with `ember g blueprint ember-loading-button`. The default blueprint will be automatically run after install (_in development it must be manually run after linking_).
2. In the default blueprint's index file using the `afterInstall` hook, add the following to install _moment.js_:
```js
afterInstall: function(options) {
  return this.addBowerPackagesToProject([ { name: 'moment' } ]);
}
```
3. From the consuming app's root directory, manually run the addon's default blueprint using `ember g ember-loading-button`.
4. Import your `bower_components/` and additional `vendor/` assets into the EmberApp's build file. This actually makes Broccoli aware to include the files in the final build.
  * Using the `included` hook, import your Bower dependencies in the consuming application's build:
  ```js
  included: function(app) {
    this._super.included.apply(this, arguments);
    app.import(app.bowerDirectory + '/moment/moment.js');
    app.import('vendor/shims/moment.js');
  }
  ```
5. In the consuming app execute `ember s` to verify the addon is being correctly consumed and working.


# Resources
  * https://ember-cli.com/extending/#link-to-addon-while-developing
  * https://ember-cli.com/extending/#default-blueprint
  * https://ember-cli.com/extending/#importing-dependency-files
