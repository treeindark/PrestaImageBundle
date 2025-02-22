PrestaImageBundle
===================

[![Build Status](https://scrutinizer-ci.com/g/prestaconcept/PrestaImageBundle/badges/build.png?b=master)](https://scrutinizer-ci.com/g/prestaconcept/PrestaImageBundle/build-status/master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/prestaconcept/PrestaImageBundle/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/prestaconcept/PrestaImageBundle/?branch=master)
[![Latest Stable Version](https://poser.pugx.org/presta/image-bundle/v/stable.png)](https://packagist.org/packages/presta/image-bundle)
[![Total Downloads](https://poser.pugx.org/presta/image-bundle/downloads.png)](https://packagist.org/packages/presta/image-bundle)

## Overview

PrestaImageBundle is a Symfony bundle providing tools to resize local/remote images before uploading them through a classic form.
It uses [Cropper][1] jQuery plugin.

## Installation

### Require the bundle as a Composer dependency

```shell script
composer require presta/image-bundle
```

### Configure the bundle

You must use the `bootstrap_5.html.twig` form theme into `config/packages/twig.yaml`.

```yaml
twig:
    form_themes:
        - "@PrestaImage/form/bootstrap_5.html.twig"
```

> Note: you can also use the `bootstrap_4.html.twig` or the `bootstrap_3.html.twig` form theme instead.

You must include the routing into `config/routes.yaml`:

```yaml
presta_image:
    resource: "@PrestaImageBundle/Resources/config/routing.yaml"
```

See VichUploader [documentation][2] to configure the bundle.

### Install assets

See Cropper [documentation][3] to install assets.

Don't forget to include the following assets in your page:

- `@PrestaImageBundle/Resources/public/css/cropper.css`
- `@PrestaImageBundle/Resources/public/js/cropper.js`

Note that [jQuery][4] and [Bootstrap][5] are required and must be included.

### How to: implementation examples

- [Webpack Encore][6]

## Usage

### Initialize cropper

```javascript
(function(w, $) {

    'use strict';

    $(function() {
        $('.cropper').each(function() {
            new Cropper($(this), true);
        });
    });

})(window, jQuery);
```

Note that you must skip the second parameter (or set it to `false`) if you are using a version of bootstrap < 5.

```javascript
// ...
new Cropper($(this));
// ...
```

### Use the form type

```php
<?php

use Presta\ImageBundle\Form\Type\ImageType;

public function buildForm(FormBuilderInterface $builder, array $options): void
{
    $builder
        ->add('image', ImageType::class)
    ;
}
```

Available options for the `ImageType`:

- `aspect_ratios` (`array`): a list of aspect ratio to apply when resizing an image
- `cropper_options` (`array`): a list of options supported by cropper (default: `['autoCropArea' => 1]`)
- `max_width` (`int`): the max width of the cropped image send to server (default: `320`)
- `max_height` (`int`): the max height of the cropped image send to server (default: `180`)
- `preview_width` (`string`): the max width to use when displaying the image preview - can be in px, % or other css value (default: `'320px'`)
- `preview_height` (`string`): the max height to use when displaying the image preview - can be in px, % or other css value (default: `'180px'`)
- `upload_button_class` (`string`): class of the button (default: `'btn btn-sm btn-primary'`)
- `upload_button_icon` (`string`): class of the button (default: `'fa fa-upload'`)
- `cancel_button_class` (`string`): class of the button (default: `'btn btn-default'`)
- `save_button_class` (`string`): class of the button (default: `'btn btn-primary'`)
- `download_uri` (`string`): the path where the image is located (default: `null`, automatically set)
- `download_link` (`bool`): whether the end user should be able to add a remote image by URL (default: `true`)
- `enable_locale` (`bool`): whether to enable the locale upload (default: `true`)
- `enable_remote` (`bool`): whether to enable the remote upload (default: `true`)
- `upload_mimetype` (`string`): format of the image to be uploaded (default: `image/png`)  
  (Note: If the chosen mimetype is not supported by the browser, it will silently fall back to `image/png`)
- `upload_quality` (`float`): quality (0..1) of uploaded image for lossy imageformats (eg. `image/jpeg`) (default: `0.92`) 
  
#### Notes

You can find Cropper options [here][7].

The `max_width` and `max_height` options are used to define maximum size the cropped uploaded image will be.
Bigger images (after cropping) are scaled down.

**Security Note:** NEVER rely on this size constraints and the format settings as 
they can be easily manipulated client side. ALWAYS validate the image-data, -size, -format 
at server side! 

## Contributing

Pull requests are welcome.

Thanks to
[everyone who has contributed](https://github.com/prestaconcept/PrestaImageBundle/graphs/contributors) already.

---

*This project is supported by [PrestaConcept](http://www.prestaconcept.net)*

**Lead Developer** : [@J-Ben87](https://github.com/J-Ben87)

Released under the MIT License

[1]: https://github.com/fengyuanchen/jquery-cropper
[2]: https://github.com/dustin10/VichUploaderBundle/blob/master/docs/usage.md
[3]: https://github.com/fengyuanchen/jquery-cropper#installation
[4]: https://jquery.com/download/
[5]: https://getbootstrap.com/docs/5.1/getting-started/download/
[6]: https://github.com/prestaconcept/PrestaImageBundle/blob/master/Resources/doc/webpack.md
[7]: https://github.com/fengyuanchen/cropperjs#options
