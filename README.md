# FlutterGotchas
This is a list of "Gotchas" that I've encountered in [Flutter](https://flutter.io/)

## Widgets

### Use a [Material](https://docs.flutter.io/flutter/material/Material-class.html) behind a [InkWell](https://docs.flutter.io/flutter/material/InkWell-class.html)

* If you want the Material Design "ink splash" effect, use the Inkwell, but make sure that you wrap it in a Material widget.
* If you use a container instead and try to add a background color, then the splash effect will be supersede by the background.
* The ink splash will cooperate with a Material widget's `color` parameter.

```dart
new Material(
    color: Colors.blue[500], //Ink Splash will properly overlay 
    child: new InkWell( ... )
);

```
