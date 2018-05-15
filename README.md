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

# Installation


The preferred way to install this extension is through Composer.

Either run php composer.phar require mubat/yii2-inherit-model-behavior "~1.0"

or add "mubat/yii2-inherit-model-behavior": "~1.0" to the require section of your composer.json


## Usage examples
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
            'relationMethod' => 'savedImage',
        ];
        return $behaviors;
    }
    
    /** @return \yii\db\ActiveQuery */
    public function getSavedImage()
    {
        return $this->hasOne(Image::class, ['id' => 'image_id']);
    }

```