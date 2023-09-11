# Laravel Nova 4 Signature

This [Nova](https://nova.laravel.com/) field lets you save a signature as a field for your resource.
It's based on the work of [lab404/nova-signature](https://packagist.org/packages/lab404/nova-signature) that works only with Nova 3 as fare as I know.

Behind the scenes [WangShayne/vue3-signature](https://github.com/WangShayne/vue3-signature) is used.

Note that vue3-signature doesn't expose all signature pad component so to get it working it was required to patch library to add support for "begin" and "end" event 
[lab404/nova-signature](https://github.com/lab404/nova-signature)


## Installation

`composer require eom/nova-signature`

## Usage

In your nova resource make the field `Eom\NovaSignature\NovaSignature`.
This field fits perfectly with a `TEXT` column and can be used both for resources and actions.

```php
public function fields()
{
    return [
        // ...
        NovaSignature::make('Signature'),
        // ...
    ];
}
```

## Configuration

Showed example values are the default ones.

```php
NovaSignature::make('Signature')
    ->color('black') // (string) Any css compatible value: "red", "rgb(0, 0, 0)", ...
    ->bgColor('white') // (string) Same as 'color'
    ->width('100%') // (string) Any css compatible value: "100%", "200px", ...
    ->height('250px') // (string) Same as 'width'
```

### Validation

You can use the `signature` validation rules to check if the signature is valid.
We also provide the `validSignature` method that adds `nullable` and `signature` rules.

```php
// With validSignature() 
NovaSignature::make('Signature')
    ->validSignature() // 'nullable' and 'signature' rules

// Your own rules
NovaSignature::make('Signature')
    ->rules('signature', ...) 
```

### Licence

MIT
