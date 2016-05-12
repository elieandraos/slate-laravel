# Eloquent Model Observers

To fire another functionality after the model is created or saved, observers class pop as a solution.

## Observer Class

```php
<?php namespace App;

use Log; //logs will be found in storage/logs/ directory

class MealObserver {

	public function created($meal)
	{
		Log::info('Meal Created');
	}

	public function saved($meal)
	{
		Log::info('Meal Saved');
	}
}
?>
```

In this example, we are considering that we have a Meal model.
Artisan doesn't offer a generate options for the observers. Manually create a class as follows:

## Service Provider


```php
<?php namespace App\Providers;

//...
use App\MealObserver;
use App\Models\Meal;

class MealServiceProvider extends ServiceProvider {

	public function boot()
	{
		Meal::Observe( new MealObserver );
	}

	//...
}
?>
```


Create a service provider that will observe the model in its boot method.

`php artisan make:provider MealServiceProvider`

Hook up the new service provider created to your application. 
In <b>config/app.php</b> add the class to the providers array

`App\Providers\MealServiceProvider::class`



