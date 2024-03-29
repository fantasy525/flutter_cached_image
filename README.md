## 这个是从Cached network image拷贝而来，只是替换了里面用到的Image widget
```dart
  _image(BuildContext context, ImageProvider imageProvider) {
    return widget.imageBuilder != null
        ? widget.imageBuilder(context, imageProvider)
        : FlutterCustomImage( // Image => FlutterCustomImage
            image: imageProvider,
            fit: widget.fit,
            width: widget.width,
            height: widget.height,
            alignment: widget.alignment,
            repeat: widget.repeat,
            color: widget.color,
            colorBlendMode: widget.colorBlendMode,
            matchTextDirection: widget.matchTextDirection,
            filterQuality: widget.filterQuality,
          );
  }
```
这个FlutterCustomImage来自这里[FlutterCustomImage](https://github.com/fantasy525/flutter_custom_image.git)
它可以避免在页面不可见时重新resolve Image，在缓存未命中时可以减少内存占用，具体可以参考FlutterCustomImage readme

**Breaking change with ImageProvider.load in Flutter 1.10**

The Flutter team made a breaking change with the ImageProvider in Flutter 1.10.15 (currently Master channel only).

If you are experiencing one of the following errors upgrade to [2.0.0-rc](https://pub.dev/packages/cached_network_image/versions/2.0.0-rc).

```
The method 'ScaledFileImage.load' has fewer positional arguments than those of overridden method 'ImageProvider.load'
```
```
The method 'CachedNetworkImageProvider.load' has fewer positional arguments than those of overridden method 'ImageProvider.load'
```


# Cached network image
Widget now uses builders for the placeholder and error widget and uses sqflite for cache management. See the [docs](https://pub.dartlang.org/documentation/cached_network_image/latest/cached_network_image/cached_network_image-library.html) for more information.

[![pub package](https://img.shields.io/pub/v/cached_network_image.svg)](https://pub.dartlang.org/packages/cached_network_image)
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.me/renefloor)

A flutter library to show images from the internet and keep them in the cache directory.

## How to use
The CachedNetworkImage can be used directly or through the ImageProvider.

```dart
CachedNetworkImage(
        imageUrl: "http://via.placeholder.com/350x150",
        placeholder: (context, url) => CircularProgressIndicator(),
        errorWidget: (context, url, error) => Icon(Icons.error),
     ),
 ```


````dart
Image(image: CachedNetworkImageProvider(url))
````

When you want to have both the placeholder functionality and want to get the imageprovider to use in another widget you can provide an imageBuilder:
```dart
CachedNetworkImage(
  imageUrl: "http://via.placeholder.com/200x150",
  imageBuilder: (context, imageProvider) => Container(
    decoration: BoxDecoration(
      image: DecorationImage(
          image: imageProvider,
          fit: BoxFit.cover,
          colorFilter:
              ColorFilter.mode(Colors.red, BlendMode.colorBurn)),
    ),
  ),
  placeholder: (context, url) => CircularProgressIndicator(),
  errorWidget: (context, url, error) => Icon(Icons.error),
),
```

## How it works
The cached network images stores and retrieves files using the [flutter_cache_manager](https://pub.dartlang.org/packages/flutter_cache_manager). 
