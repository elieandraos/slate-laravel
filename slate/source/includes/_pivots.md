# Pivot Models

working with many-to-many relations requires the presence of an intermediate table. 
Eloquent provides some very helpful ways of interacting with this table. 
Let's assume our User object has many Meals objects that it is related to. 

##Schema

```php
<?php 

 //...

 public function up()
 {
 	Schema::create('meal_user', function( Blueprint $table) {

 		$table->increments('id');
 		//foreign keys
 		$table->integer('user_id')->unsigned()->index();
 		$table->foreign('user_id')->refrences('id')->on('users')->onDelete('cascade');
 		$table->integer('meal_id')->unsigned()->index();
 		$table->foreign('meal_id')->refrences('id')->on('meals')->onDelete('cascade');

 		//other columns 
 		$table->timesteamp('created_at');
 		$table->boolean('is_allowed');

 		//In case the foreign key needs to be unique
 		// $table->unique(['user_id', 'meal_id']);
 	});
 }

 //...
```


Generate a migration with artisan. By convention, the table names are sorted alphbetically.
In our example it will be create_meal_user_table (meal comes before user).

`php artisan make:migration create_meal_user_table --create-meal_user`


##Models relations

> Meal Model

```php
<?php
//...

public function users()
{
	return $this->belongsToMany('App\Models\User');
}
?>
```

> User Model

```php
<?php
//...

public function meals()
{
	return $this->belongsToMany('App\Models\Meal')->withPivot('created_at', 'is_allowed');
}
?>
```

You can access the pivot table with the `belongsToMany()` relation.

If your pivot table contains extra attributes, you must specify them when defining the relationship using `withPivot()`


##Retrieving Values

```php
<?php
	$meals = $user->meals;
	foreach($meals as $meal)
	{
		echo $meal->name; //meal table
		echo $meal->pivot->created_at; //pivot table
		echo $meal->pivot->is_allowed; //pivot table

	}
?>
```

In this example, we will get a user's meals and their created_at and is_allowed from the pivot table.

##Chaining

```php
<?php
$user_meals = $user->meals()
					 ->where( DB::raw('DATE(metric_user.created_at)'), '=', date('Y-m-d'))
					 ->where( 'is_allowed', '=', 1)
					 ->get();
?>
```
To apply further eloquent methods on the collection, use the method `->meals()` instead of `->meals`.

In this example we will get the meals that were created today.



