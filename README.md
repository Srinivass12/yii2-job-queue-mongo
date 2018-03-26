# yii2-job-queue-mongo

Requirements
------------

[Mongo](https://www.mongodb.com/)

[yii2-mongodb](https://github.com/yiisoft/yii2-mongodb)

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist Srinivass12/yii2-queue "master@dev"
```

or add

```
"Srinivass12/yii2-queue": "master@dev"
```

to the require section of your `composer.json` file.


Usage
-----

To use this extension, simply add the following code in your application configuration:

```php
return [
    //....
    'components' => [
        'queue' => [
            'class' => 'srini\queue\MongoQueue',
        ],
        'mongo' => [
            'class' => 'yii\mongo\Connection',
            'hostname' => 'localhost',
            'port' => 6379,
            'database' => 0
        ],
    ],
];
```



First create a Job process class

```php
namespace console\jobs;

class MyJob
{
    public function run($job, $data)
    {
        var_dump($data);
    }
} 
```

and than just push job to queue

```php
// Push job to the default queue and execute "run" method
Yii::$app->queue->push('\console\jobs\MyJob', ['a', 'b', 'c']);

// or push it and execute any other method
Yii::$app->queue->push('\console\jobs\MyJob@myMethod', ['a', 'b', 'c']);

// or push it to some specific queue
Yii::$app->queue->push('\console\jobs\MyJob', ['a', 'b', 'c'], 'myQueue');

// or both
Yii::$app->queue->push('\console\jobs\MyJob@myMethod', ['a', 'b', 'c'], 'myQueue');

```  

Map console controller in your app config

```php
return [
    ...
    'controllerMap' => [
        'queue' => 'srini\queue\console\controllers\QueueController'
    ],
    ...
];
```

Examples of using queue controller:

```
# Process a job from default queue and than exit the process
./yii queue/work

# continuously process jobs from default queue
./yii queue/listen

# process a job from specific queue and than exit the process
./yii queue/work myQueue

# continuously process jobs from specific queue
./yii queue/listen myQueue
```
