### Wiretree#get(key)

Get module or group of modules `key`.
This method will render all required wiretree plugins into modules

```js
tree.get( 'myModule' );
// returns the module myModule

// getting myModule throught a group
var group = tree.get( 'myGroup' );
var myModule = group.myModule;
```



**Parameters**

**key**:  *String*,  name of plugin to get

**Returns**

*Object*,  rendered plugin

### Wiretree#add(key, value, group)

Add a module or wiretree plugin into the tree. Wiretree plugins won't be resolved until you get them.
Returns a list of module dependencies in an `array`.

**Example**:

```
// Add and get a simple module
var addon = 2;

tree.add( addon, 'mod' );
// => []

tree.get( 'mod' );
// => 2

// Add and get a wiretree plugin (a module with dependencies)
var plugin = function (mod) { return mod + 2; };

tree.add( {wiretree: plugin}, 'plugin' );
// => ['mod']

tree.get( 'plugin' );
// => 4
```

Passing a `group` will add the module to it. `localName` is the key for the group, equals passed `key` by default

```js
tree.add( 1, 'homeCtrl', 'control', 'home');
tree.get( 'homeCtrl' );
// => 1

var control = tree.get( 'control' );
return control.home;
// => 1
```



**Parameters**

**key**:  *String*,  name for the plugin

**value**:  *type*,  plugin

**group**:  *String*,  (optional) name of group to add the plugin

**Returns**

*Array*,  list of dependencies names

### Wiretree#load(key, route, group)

Load a module or wiretree plugin from `route` in disk and add it into the tree. Wiretree plugins won't be resolved until you get them.
Returns a list of module dependencies in an `array`.

**Example**:

module.js:
```
module.exports = function () {
return 2;
};
```

plugin.js:
```
module.exports.wiretree = function (mod) {
return mod + 2;
};
```

index.js:
```js
tree.load( './module.js', 'mod' );
// => []

tree.get( 'mod' );
// => 2

tree.load( './plugin.js', 'plugin' );
// => ['mod']

tree.get( 'plugin' );
// => 4
```

Passing a `group` will add the module to it. `localName` is the key for the group, equals passed `key` by default

```js
tree.load( './module.js', 'homeCtrl', 'control', 'home');
tree.get( 'homeCtrl' );
// => 1

var control = tree.get( 'control' );
return control.home;
// => 1
```



**Parameters**

**key**:  *String*,  name for the plugin

**route**:  *String*,  path to plugin

**group**:  *String*,  (optional) name of group to add the plugin

**Returns**

*Array*,  list of dependencies names

### Wiretree#folder(route, options)

Load and add every file in the folder `route`.

Filename without extension is `key` and `localName` for every file, but prefixes and suffixes can be
added to the `key` through `options.prefix` and `options.suffix` with camelCase style. These transformations
not affects the `localName` in groups.

Returns list of `key`s in an `array`

```js
tree.folder( './myFolder' );
// => ['myMod', 'myPlugin']

var options = {
group: 'myGroup',
prefix: 'pre',
suffix: 'Ctrl'
}

tree.folder( './myFolder', options);
// => ['preMyModCtrl', 'preMyPluginCtrl']

tree.get( 'myGroup');
// => {myMod: [object Function], myPlugin: [object Function]}
```



**Parameters**

**route**:  *String*,  path to folder

**options**:  *Object*,  

