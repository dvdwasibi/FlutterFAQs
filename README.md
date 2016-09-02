# FlutterFAQs
Pro and Semi-pro tips for [Flutter](https://flutter.io/).

*Please contribute if you encounter anything too. Just file a PR!*


## Widgets

### Use a [Material](https://docs.flutter.io/flutter/material/Material-class.html) behind a [InkWell](https://docs.flutter.io/flutter/material/InkWell-class.html)

* If you want the Material Design ["ink splash"](https://material.google.com/motion/material-motion.html#material-motion-how-does-material-move) effect, use the `Inkwell`, but make sure that you have it as the child of a `Material` widget.
* If you use a `Container` instead and try to add a background color, the splash effect will be superseded by the background.
* The ink splash will cooperate with a `Material` widget's `color` parameter.

```dart
new Material(
    color: Colors.blue[500], //Ink Splash will properly overlay
    child: new InkWell( ... )
);

```
### [CircleAvatar](https://docs.flutter.io/flutter/material/CircleAvatar-class.html) will not radially clip its background image.
* `CircleAvatar` (despite the name) will not actually clip a `backgroundImage` to a circle: [Issue](https://github.com/flutter/flutter/issues/5306)
* The most performant approach is to probably pre-process any avatar image beforehand and clip it to a circle.
* However this is not always possible and brings additional overhead.
* Instead you can create a `Container` with equal width and height (to ensure that the clipped radial area is a perfect circle) and add a [`ClipOval`](https://docs.flutter.io/flutter/material/ClipOval-class.html) widget as the child of that container. Then you can place the image as the child of the `ClipOval`;

```dart
// ClipOval will radially clip based on the parent
// If you have a rectangle with different height & width, then you will
// get an oval. If you have a square, you will get a circle.
new Container(
  width: 40.0,
  height: 40.0,
  child: new ClipOval(
    child: new Image( ... )
  )
)
```

## Layout

### Use [Stack](https://docs.flutter.io/flutter/widgets/Stack-class.html) to mimic "Absolute Positioning" in web land
* If you come from a web-dev background and you want to position a widget relative to another widget, the `Stack` widget is your friend.
* Example:
  * Say you want to add an "annotation icon" to a `CircleAvatar`:   
  * ![](https://github.com/dvdwasibi/FlutterGotchas/blob/master/assets/images/stack.png?raw=true)
  * Create a `Stack` with the avatar as the first child. The *'z-index'* of each stack child will be based on order of appearance in the child list.
  * Because you want to position the icon relative to the avatar, you need to wrap it inside a [`Positioned`](https://docs.flutter.io/flutter/widgets/Positioned-class.html) widget which will place it relative to the stack. Add that as the next child in the list.

```dart
new Stack(
  children: <Widget>[
    new CircleAvatar( ... ),
    // This will place the icon at the bottom right hand corner
    new Positioned(
        right: 2.0,
        bottom: 2.0,
        child: new Icon(...)
    )
  ]
);
```


## Testing

### Use [WidgetTester.pump](https://docs.flutter.io/flutter/flutter_test/WidgetTester/pump.html) if you have to test the end results of a user interaction that leads to many animations.
* You will need to "run through" the animations for a single interaction that lead to a sequence of events before something is done.
* An example is testing the effects of a `onDismissed` callback for a [`Dismissable`](https://docs.flutter.io/flutter/widgets/Dismissable-class.html) widget (think swipe to delete).
* After you use the [`WidgetTester`](https://docs.flutter.io/flutter/flutter_test/WidgetTester-class.html) to swipe the widget, the following events happen before the `onDismissed` function is called:
  * The slide has to start
  * The slide has to end
  * The widget has to start shrinking vertically
  * The widget has to finish shrinking
  * The widget has to removed from the tree
* And only then does the `onDismissed` function gets called.
* So make sure to call `WidgetTester.pump` for each event before testing the completed logic of the callback.

```dart
// Simulate the swipe
await tester.fling( ... )

// PUMP IT UP!!!
await tester.pump(); // start the slide
await tester.pump(const Duration(seconds: 1)); // finish the slide and start shrinking...
await tester.pump(); // first frame of shrinking animation
await tester.pump(const Duration(seconds: 1)); // finish the shrinking and call the callback...
await tester.pump(); // rebuild after the callback removes the entry

// Then test logic of completion of swipe
// ...
```

## Bonus

### A group of flutter devs working together is called a Kaleidoscope
 ![FlutterFly](https://github.com/dvdwasibi/FlutterGotchas/blob/master/assets/images/butterfly.png?raw=true)

### Listen to Hustlin'/Flutterin' by Rick Ross to reduce bug count.
* Listen to [Hustlin'](http://genius.com/Rick-ross-hustlin-lyrics) by Rick Ross
* Sing along but replace all instances of `Hustlin'` with `Flutterin'`
* Expect to see about 30-50% less bugs in your Flutter code
* Repeat and prosper

![Flutterin'](https://pbs.twimg.com/media/Cq5ICdkUkAEFt0Y.png:large)
