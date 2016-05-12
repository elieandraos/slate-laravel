# Events And Listeners

Laravel allows out of the box to subscribe and listen for events in your application. Event classes are typically stored in the `app/Events` directory, while their listeners are stored in `app/Listeners`.

##Event
```php
<?php

namespace App\Events;

use App\Events\Event;
use Illuminate\Queue\SerializesModels;
use Illuminate\Contracts\Broadcasting\ShouldBroadcast;

class UserHadMeal extends Event
{
    use SerializesModels;
    
    public $meal, $user;

    /**
     * Create a new event instance.
     *
     * @return void
     */
    public function __construct($user, $meal)
    {
        $this->user = $user;
        $this->meal = $meal;
    }

    /**
     * Get the channels the event should be broadcast on.
     *
     * @return array
     */
    public function broadcastOn()
    {
        return [];
    }
}

```

We will continue with the user/meal models. We will generate an event after the user have the meal, to send him and email or do something else on the models.

`php artisan make:event UserHadMeal`


All we need to do is pass the parameters to the `__construct()` method. 

##Listener
```php
<?php

namespace App\Listeners;

use App\Events\UserHadMeal; 
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Contracts\Queue\ShouldQueue;

class SendEmailMeal
{
    /**
     * Create the event listener.
     *
     * @return void
     */
    public function __construct()
    {
        //
    }

    /**
     * Handle the event.
     *
     * @param  UserHadMeal  $event
     * @return void
     */
    public function handle(UserHadMeal $event)
    {
        $user = $event->user;
        $meal = $event->meal;

        // Mail( $user had meal $meal->name); or anything else you need to do
    }
}
```

The listener is what will the event do once it is fired. Generate the listener with artisan as follows.

`php artisan make:listener SendEmailMeal --event=UserHadMeal`

Everything you need to do is inside the `handle()` function of the listener class.


##Event Service Provider 
```php
<?php
/**
 * The event listener mappings for the application.
 *
 * @var array
 */
protected $listen = [
    'App\Events\UserHadMeal' => [
        'App\Listeners\SendEmailMeal',
    ],
];
?>
```
To Register the event and map its listener(s), you add it to the `EventServiceProvider` included with your Laravel application.


##Firing the event
Fire the event inside your controller method like so: 

`Event::fire(new UserHadMeal($user, $meal));`
