# Backbone.viewOptions

A mini [Backbone.js](http://backbonejs.org/) plugin to declare and set options on views. Supports default values and required options.

## Benefits

* Use a simple declarative syntax to attach white-listed initialization options directly to your view objects. 
* Supply default values for particular options.
* Declare required options as such, so an exception is raised immediately if one is not supplied.
* Change options post-initialization via `view.setOptions()`.
* Can be used with any view class, including those in [Marionette](https://github.com/marionettejs/backbone.marionette), etc.

## Usage

```javascript
// Call Backbone.ViewOptions.add() and view.setOptions() in our base view constructor so
// that the view options functionality is added to all our views and options are attached.
BaseView = Backbone.View.extend( {
	initialize : function( options ) {
		Backbone.ViewOptions.add( this );  // initializes view options functionality on this view
		this.setOptions( options );  // set the view's options from initialization options
		...
	}
} );

ButtonView = BaseView.extend( {
	options : [ "label" ]

	initialize : function() {
		// Options that are white-listed in the view's "options" 
		// property are automatically attached to the view object.
		console.log( this.label );
	}
} );

// Outputs "OK" to the console.
myButtonView = new ButtonView( { "label" : "OK" } );

WidgetView = BaseView.extend( {
	options : [
		"type!",  // Use a trailing exclamation mark to indicate that an option is required.
		{ "label" : "OK" }  // Use this object syntax to give an option a default value.
	]

	render : function() {
		console.log( this.label );
	}
} );

// Outputs "OK" to the console (because the "label" option defaults to "OK").
myWidgetView = new WidgetView( { "type" : "button" } ).render();

myWidgetView.setOptions( { "label" : "Save" } );
myWidgetView.render();  // Outputs "Save" to the console.

// Throws an exception (because the required "type" option is missing).
myOtherWidgetView = new WidgetView( { "label" : "Cancel" } ).render();
```

## Methods and Properties

#### `Backbone.ViewOptions.add( view )`

Generally used in your base view's `constructor` or `initialize` method, this function adds the view options functionality to the supplied view:

```javascript
initialize : function( options ) {
	Backbone.ViewOptions.add( this );
	this.setOptions( options );  // now we can (and should) call view.setOptions()
	...
}
```

#### `view.options` property

An "option declarations" array should be supplied as the `options` property of the view class. Each element in the array must be a string or an object.
<<<<<<< HEAD
* A string element simply white-lists the name of an option that should be attached to the view when it is supplied in `view.setOptions()`'s `optionsHash` (see below). The name may optionally be followed by an exclamation mark, which indicates a "required" option.
=======
* A string element simply white-lists the name of an option that should be attached to the view when it is supplied in `view.setOptions()`'s `optionsHash` (see below). The name may optionally be followed by an explanation mark, which indicates a "required" option.
>>>>>>> 09ebd638223052f00b8065074a05c6bdada1cc81
* An object element may be used to give an option a default value, the key of the object being the option's name and the value its default value.

You may alternatively supply a function that _returns_ an array as `view.options`, very much like how you may supply a function that returns a hash for the built-in backbone `view.events` property.

#### `view.setOptions( optionHash )`

Sets the view's options to the values in `optionHash` as appropriate, given the option declarations in `view.options`. If a "required" option is not supplied (and it is not already on the view) an exception is raised.

#### `view.getOptionNames()`

Returns an array containing all view properties that are options (i.e. the options declared in `view.options`).

#### `view.onOptionsChanged( changedOptions )` callback

This method, if it exists on a view, is called when option(s) _that are already present on the view object_ are changed via `view.setOptions()`. `changedOptions` is a hash of the changed options and their new values.
