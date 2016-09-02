# FlutterGotchas
This is a list of "Gotchas" that I've encountered in [Flutter](https://flutter.io/).

## Widgets

### Use a [Material](https://docs.flutter.io/flutter/material/Material-class.html) behind a [InkWell](https://docs.flutter.io/flutter/material/InkWell-class.html)

* If you want the Material Design ["ink splash"](https://material.google.com/motion/material-motion.html#material-motion-how-does-material-move) effect, use the `Inkwell`, but make sure that you have it as the child of a `Material` widget.
* If you use a `Container` instead and try to add a background color, then the splash effect will be supersede by the background.
* The ink splash will cooperate with a `Material` widget's `color` parameter.

```dart
new Material(
    color: Colors.blue[500], //Ink Splash will properly overlay
    child: new InkWell( ... )
);

```
### [CircleAvatar](https://docs.flutter.io/flutter/material/CircleAvatar-class.html) will not radially clip it's background image.
* `CircleAvatar` (despite the name) will not actually clip a `backgroundImage` to a circle if the image: [Issue](https://github.com/flutter/flutter/issues/5306)
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
