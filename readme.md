# Image Field with built-in cropper for Laravel Nova

##	:bangbang: This is a fork from this [package](https://github.com/64robots/nova-image-cropper).

This field extends Image Field adding a handy cropper to manipulate images. Can be configurable in the same way as a [File field in Nova](https://nova.laravel.com/docs/1.0/resources/file-fields.html).

### Demo

![Demo](http://g.recordit.co/9ubWDbqNNL.gif)

### Install

Run this command into your nova project:
`composer require soundsmarket/nova-image-cropper`

### Add it to your Nova Resource:

```php
use SoundsMarket\NovaImageCropper\ImageCropper;

ImageCropper::make('Photo'),
```

### Update form

In order to edit the existing image saved in the model, ImageCroper uses the preview method to return a base64 encoded image. You can either use the default implementation or override it as long as you return a base64 image.

```php
use SoundsMarket\NovaImageCropper\ImageCropper;

ImageCropper::make('Photo')
        ->preview(function () {
            if (!$this->value) return null;

            $url = Storage::disk($this->disk)->url($this->value);
            $filetype = pathinfo($url)['extension'];
            return 'data:image/' . $filetype . ';base64,' . base64_encode(file_get_contents($url));
        });
```

### Options

#### Avatar mode

You can add a rounded mask to the preview and the cropper

```php
ImageCropper::make('Photo')->avatar()
```

#### Custom aspect ratio

Define the fixed aspect ratio of the crop box.

- Type: Number
- Default: NaN

```php
ImageCropper::make('Photo')->aspectRatio(16/9)
```

For free ratio use:

```php
ImageCropper::make('Photo')->aspectRatio(0)
```

### Localization

Set your translations in the corresponding xx.json file located in `/resources/lang/vendor/nova`

```php
...

  "Edit Image": "Editar Imagen",
  "Cancel Crop": "Cancelar Recorte",
  "Change Image": "Cambiar Imagen",
  "Done": "Hecho",
  "Click here or drop the file to upload": "Click aquí o arrastra el archivo para comenzar la subida"
```
