# laravelWebsocket
# laravelWebsocket
Laravel websocket broadcasting with Clinet Lib Vue Js, Socket server Pusher and Web Server

# Download or git clone. 
Run composer install at your project directory to download PHP dependencies
Run npm install to download JS dependencies
if .env is present then change according to your database  or Run cp .env.example .env to create your projects' .env file then Run php artisan key:generate to create a new encryption key
Back in the terminal, run php artisan migrate to migrate your database
Run php artisan db:seed to seed your database
	#	Create Virtualhost At Window
	xammp: E:\xampp\apache\conf\extra\httpd-vhosts.conf

	<VirtualHost *:80>
    
    		DocumentRoot "E:/xampp/htdocs/dirname/public"
    		ServerName websocketblog.test
   
	</VirtualHost>
	# Virtual Host At Vagrant open your homestead.yam file
		folders:
    		- map: D:/Projects
      	to: /home/vagrant/Code

   
		sites:
   		 - map: blog.test
     		 to: /home/vagrant/Code/blog/public

Comments Save:

 We will create some structure to save our comments.

1. php artisan make:controller CommentController
2. php artisan make:model Comment

We will make 2 API endpoints for our comments. The first is to display all comments for a specific post. The second endpoint will allow us to save a new comment via the API.

Then its time to use this API by setting up our frontend. We will create two Vue.js methods to handle this. The first is getComments() which gets all the comments for the current post, and the second is postComments() which will post a new comment for the current post.

For Broadcasting With pusher server

After Api completion then we will trigger the event after it saves, and pass the new comment's details to the socket server.

Creat the account at: Pusher.com
On the server side we can just use the basic Laravel event/listener system, except that we won't bother ourselves with listeners. Listeners can still be used to run scripts server-side when an event is triggered, but our primary concern is to trigger events and then broadcast those events, which will be sent to our socket server on Pusher.com.

When account is created:  make your first app then you will get these credentials: 
app_id = "250292"
key = "23b00356d01e20f3336401"
secret = "e59e21e040baf60a70eca"
cluster = "a2p"

composer require pusher/pusher-php-server
npm install --save pusher-js
npm install --save laravel-echo 
or 
npm install --save laravel-echo pusher-js
Server side settings : 


update your .env file
BROADCAST_DRIVER=pusher
CACHE_DRIVER=file
SESSION_DRIVER=file
SESSION_LIFETIME=120
QUEUE_DRIVER=sync

PUSHER_APP_ID=507529
PUSHER_APP_KEY=3b00356d01e0f3336401
PUSHER_APP_SECRET=e59e1e040baf60a70eca
PUSHER_APP_CLUSTER=ap2


Acitvate laravel broadcasting:
config/app.php
uncomment this line: App\Providers\BroadcastServiceProvider::class,
For Pusher socket server:
update at config/broadcasting.php
'pusher' => [
            'driver' => 'pusher',
            'key' => env('PUSHER_APP_KEY'),
            'secret' => env('PUSHER_APP_SECRET'),
            'app_id' => env('PUSHER_APP_ID'),
            'options' => [
                        'cluster' => env('PUSHER_APP_CLUSTER'),
                        'encrypted' => true,
                //
            ],


On the client-side we will use Laravel Echo to subscribe to channels when the page loads. This will prepare our clients to accept any new events that come down the tube while we are subscribed. Then we will tell Laravel Echo what to do when we hear the NewComment event, and use Vue.js to update the newest comment into our comments array, so that it is automatically displayed in the comments feed.

Client side scripts update:
resources/assets/js/app.js updated it
/*
Vue.component('example-component', require('./components/ExampleComponent.vue'));

const app = new Vue({
    el: '#app'
});
*/
resources/assets/js/bootsrap.js updated it

window.Echo = new Echo({
    broadcaster: 'pusher',
    key: '3b00356d01e0f3336401',
    cluster:'ap2',
    encrypted:true
});

In the end we will have a powerful live commenting realtime engine. It will be a great example of what can be done with websockets and Laravel echo!


