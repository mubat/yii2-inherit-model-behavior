# yii2-inherit-model-behavior
Use this behavior to connect inherit ActiveRecord with (one-to-many relation)

In parent ActiveRecord should be column that uses to store ID of inherit ActiveRecord.

Extension supports 2 request formats:
    
* default Yii2 input data, like:
```php
//$_POST[]
[
    'Object' => [
        'option_1' => 'value',
        'option_2' => 'another value',
    ]
]
```
* simple input names (usable for API), like:
```php
//$_POST[]
[
    'option_1' => 'value',
    'option_2' => 'another value',
]
```

You can disable inherit object deletion if need.

## Installation


The preferred way to install this extension is through Composer.

Either run `php composer.phar require mubat/yii2-inherit-model-behavior "~1.0"`

or add `"mubat/yii2-inherit-model-behavior": "~1.0"` to the require section of your composer.json


## Usage examples
* Options:
    * __`dependClass`__ [__required__ _string_] - target class name;
    * __`dependClassInitConfig`__ [_array_] - some init configuration for target class. See `\yii\app\Yii::createObject()` 
    * __`virtualOption`__ [__required__ _string_] - option name that will be use at project
    * __`relationMethod`__ [_string_] - getter that returns with `\yii\db\ActiveQuery` object. By default, `get[virtualOption]()`
    * __`primaryKeyName`__ [_string_] - key name at inherit model. Default `"id"`
    * __`linkAttribute`__ [_string_] - column name in owner table for connect with inherit table. By default, `[virtualOption]_id`
    * __`createDependObjectOnEmpty`__ [_boolean_] - you can disable empty inherit object creation if need. By default, _true_
    * __`simpleRequest`__ [_boolean_] - How need to parse simple options: like 'bar' (_true_) or like 'Foo[bar]' (_false_) at request. By default, _false_ 
    * __`deleteWithOwner`__ - with this option you can disable/enable run `delete()` action on inherit object. _Default: true_ (It also can change status during processing on the fly)
    
* Usage:
```php
    public function behaviors()
    {
        $behaviors = parent::behaviors();
        $behaviors['image'] = [
            'class' => InheritModelBehavior::class,
            'dependClass' => Image::class, //required
            'virtualOption' => 'image', //required
            'linkAttribute' => 'image_id',
            'relationMethod' => 'getSavedImage',
            'simpleRequest' => true,
            'deleteWithOwner' => true,
        ];
        return $behaviors;
    }
    
    /** @return \yii\db\ActiveQuery */
    public function getSavedImage()
    {
        return $this->hasOne(Image::class, ['id' => 'image_id']);
    }
```

