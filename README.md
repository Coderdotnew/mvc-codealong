# your_first_web_app    
It's time. It's finally time to put a dynamic application on the web. Quick side note: we are building a **web application**, not just a website. So what's the difference? Think of a website as a *static* page on the web that's sole purpose is to provide *content*. Using HTML and maybe even some CSS styling, a website can present information, text, images, and maybe even some videos. Your school webpage is likely a static website. This means you *cannot* interact with it. I know you can click the links and read the text, but you can't *truly interact with it*.  

###### Let's focus, this is a long one  
![1](http://i.imgur.com/6JVvBxr.gif)

Think about Airbnb. This is an example of a *web application*. You want to take a vacation to Paris so you head over to [Airbnb](www.airbnb.com) and you login with your username and password. Already, you have interacted with this application in a dynamic manner as you have passed in some attributes (username and password). This web app already has your name, address, password, and maybe even some saved search preferences stored.  

You are constantly interacting with the application by inputting information and receiving new data based in these inputs. 
- For example, when you search for Paris and you receive listings that are specific to Paris. You may further specify a maximum price and specific location within the city. A web application is defined by its *interaction with the user*.  
![2](http://i.imgur.com/p2HVonX.gif)  

# lets_build
Open your `sinatra_template` folder. Here you will find a number of folders and files that we began working on a week ago. Enter the command `cd sinatra_template` to go inside of this directory to run your server. 


# shotgun
Enter the command: `shotgun -p 8080 -o 0.0.0.0.`. Your server is now running! You will see a similar output in your terminal:  
```bash
== Shotgun/WEBrick on http://0.0.0.0.:8080/
[2016-05-22 00:39:25] INFO  WEBrick 1.3.1
[2016-05-22 00:39:25] INFO  ruby 2.3.0 (2015-12-25) [x86_64-linux]
[2016-05-22 00:39:25] INFO  WEBrick::HTTPServer#start: pid=1876 port=8080
```
- You'll notice a few things your recognize
  - port and ip numbers
  - language (Ruby) and version
  - date and time the server has started up  

You'll also notice a link that conveniently pops up in your terminal. This is our web app! If you click this link it will take you to the location of your app! You're probably thinking, but we haven't coded anything?  

Luckily, since Sinatra is built with Ruby, it also provides some great error messages! Let's check out our first error message.  
![4](http://i.imgur.com/ayW2zdq.png?1)  

This tells us a few things:
- We need to add code to our `application_controller.rb`
- There needs to be a class called `ApplicationController`
- There needs to be some type of method or function inside the class  

# views
Let's open our `index.erb` file. **ERB**, or Embedded Ruby, is a simply an HTML file that can have Ruby logic embedded in. Seeing as this is a *template* you can use over and over again, you'll notice much of the *meta data* is already filled in to save you time. There is already a link to our stylesheet, a `title` attribute, and a container `div` inside the `body` attribute. 
```html
<!DOCTYPE html>
<html>
<head>
  <title>Title</title>
  <!-- links to CSS stylesheet in the public directory -->
  <link rel="stylesheet" href="index.css" >
</head>
<body> 

  <div class="home-page">

  </div>

</body>
</html>
```  
###### Hopefully this HTML looks familiar...   

Remember, we already configured our views to be in communication with our controller, now we just need to add content! Let's move the `"Hello World"` to our `index.erb` view and out of our application controller.  
```html
<!DOCTYPE html>
<html>
<head>
  <title>Title</title>
  <!-- links to CSS stylesheet in the public directory -->
  <link rel="stylesheet" href="index.css" >
</head>
<body> 

  <div class="home-page">
    <h1>Hello World!</h1>
  </div>

</body>
</html>
```  
Our controller is responsible for handling requests to get data from our views so now we need to explicitly tell our controller how to *get* this data. We are going to replace the `"Hello World"` in our *get* method to a link for our `index.erb` file.
```ruby
require './config/environment'
require './app/models/status'

class ApplicationController < Sinatra::Base

  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
  end

  get '/' do 
    erb :index
  end

end
```
- And just like that, our controller is making a get request and *getting* any data from the `index.erb` file. 
- Notice we are using a **symbol** for our file name. Recall symbols use less memory than strings and are immunatable and unique, meaning we can't use the same file name more than once!
- Go ahead and run your server again (`shotgun -p 8080 -o 0.0.0.0.`)
- Now you'll notice we have our data displayed from the view, rather than directly from our controller!

# erb
Right now, our application is rendering HTML. Let's embed some Ruby. In order to embed Ruby, there is special syntax. When we want to display Ruby (data, objects, etc) we use `<%= "ruby string" %>`. These special tags inform the HTML that Ruby is being embedded and will be interpreted as Ruby code. Let's render a Ruby string, rather than HTML markup (while we'are at it, let's call a Ruby string method on our string)
```html
<!DOCTYPE html>
<html>
<head>
  <title>Title</title>
  <!-- links to CSS stylesheet in the public directory -->
  <link rel="stylesheet" href="index.css" >
</head>
<body> 

  <div class="home-page">
    <h1><%= "hello world".upcase %></h1>
  </div>

</body>
</html>
```  
- Our string is in all caps now! 
- The most important part of this code is the `=` sign. When we want to render our Ruby, we must include the `=` after the `%`
- However, if we are using Ruby logic that we do not want to be rendered for the user to view (who wants to view logic when you are browsing a webpage?) we *do not* include the `=`. 
- Let's use an example that iterates through an array of strings and see when it is appropriate to include the `=` within our erb.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Title</title>
  <!-- links to CSS stylesheet in the public directory -->
  <link rel="stylesheet" href="index.css" >
</head>
<body> 

  <div class="home-page">
    <% names = ["Jon", "Daenerys", "Gregor", "Arya", "Tyrion"] %> 
    <% names.each do |name| %>
      <h1><%= "Hello, #{name}!" %></h1>
    <% end %>
  </div>

</body>
</html>
```
- Once you break this down and learn to read between the erb sytnax, you'll realize this is nothing new!
- A few things to notice:
  - We only use `<%= %>` when we want to render information for the user. It wouldn't make sense to render any other lines for the user to view, which also makes sense to *not* include an HTML tag for these lines of embedded Ruby
  - Notice that rather than `puts`, we now have the ability to use any HTML tag to render our code, such as `<h1>`
  - It's critical to include the `<% end %>` keyword, as any numeration requires an `end` keyword
  - Lastly, you've probably already recognized it can be rather difficult to embed a lot of Ruby with this special syntax
  - This is where the **model** of the **MVC** comes into play, allowing us to code our Ruby in a `.rb` file and call this code inside our views!
 
# model
It's best to refactor most Ruby logic into our models. Remember, you can have as many models as your application requires (this is so no one model gets overly crowded), but for now, we'll start with one model, aka one class
- Let's create a model that does the following:
  - The `model.rb` file should instantiate a class, `Status`
  - Two attributes, `username` and `status` should be defined upon initialization
  - The user should be able to access these attributes with an `attr_accessor`, in other words, a `reader` and `writer` method  

Let's open our `model.rb` file (yours is blank). Feel free to add the following code into yours (don't forget to save!)
```ruby
class Status
  attr_accessor :username, :status

  def initialize(username, status)
    @username = username
    @status = status
  end

end
```

- This model is essentially a template to create a new Status in our application.
- This model requires a username and status to be input by the user before they can post a status. 
- The `attr_accessor` line allows you to access the username and status anywhere in the application.
- The `@` symbol allows those variables to travel anywhere in your application.

# forms
We need a way for users to input data, this is where HTML **forms** come into play.  

Think about anytime you sign up or login to a web application -- you type information into a textbox or some type of input field, and then you click a button which submits this data to be processed by the controller and model. We need to add a form for our users to input their `username` and `status`. Enter the following code inside the `<div class="homepage">` inside the `index.erb` file.
```html
<!DOCTYPE html>
<html>
<head>
  <title>Title</title>
  <!-- links to CSS stylesheet in the public directory -->
  <link rel="stylesheet" href="index.css" >
</head>
<body> 

<div class="home-page">

  <h1>Post a status!</h1>
  <form method="POST" action="/results">
    <p>username: <input type="text" name="username"></p>
    <p>status: <input type="text" name="status"></p>
    <input type="submit">
  </form>

</div>

</body>
</html>
```
The user now has a text field to input data that we can use to instantiate a new `Status` class!  
Some very important components of our form:  
  1. The `method` is our HTTP method and will *POST* this data for the viewer to see
  2. The `action` determines *where* this data will be *posted*
  3. The `name` for each `<input>` field represents the **parameter** or argument. This is critically important! Just like when we define our parameters in our intialize method, these must be uniquely indentified (for the same reason we cannot have 2 arguments with the same name)
  4. Lastly, we *MUST* include the `submit` `<input type>` to actually *send* this data to our controller to be instantiated  

Time to instantiate this object! Back to the controller we go!

# MVC_object_instantiation
Our controller is constantly *getting* and *posting* data for our users, but now we have an object we've defined and want to utilize. So far, our controller only has a `get` HTTP method to retrieve our `index.erb` view file.   

- However, we need to *post* information to a new view, this information being the `username` and the `status` the user enters in the form on the `index.erb` file.  
- The `post` method will need to be added to our controller, because we want to post data and because we defined a `"POST"` method in our HTML form. 
- The `post` method looks very similar to the `get` method with a few differences:
  - The name of the method being used
  - The action of this method, meaning where this data will be posted (`results.erb`)
  - Object instantiation using the parameters input by the user in the form
  - A different `erb` file to load  
 
```ruby
require './config/environment'
require './app/models/model'

class ApplicationController < Sinatra::Base

  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
  end

  get '/' do 
    erb :index
  end

  post '/results' do 
    @status1 = Status.new(params[:username], params[:status])
    erb :results
  end

end
```
- Takeaways:
  - The *action* of the HTTP method must point to the *same exact action* from our HTML form (`'/results'`). Note, this will also be the **URL** for our website, or the **slug** (URL slugs are the exact address of a specific website, so now rather than simply `/`, we will have `/status` at the end of our application URL)
  - We need an *instance variable* to store the newly instantiated object, otherwise the object would just be floating around. Remember, we will be calling our attribute methods on the instance variable!
  - Our parameters have new syntax when being instantiated.
    - `params` is a keyword that signifies a parameter is being input from the `name` field in the HTML form and the `[:symbol]` is the specific name represented as a unique symbol, again, defined in the `name` field in the HTML form  

For instance, if we instantied this in our Ruby file it might look like this:
```ruby
@status1 = Status.new("coderdotnew", "This is my first status!")
```
- However, because the user cannot instantiate an object like this with their terminal command line, they input their parameters with the forms, which are defined in the `name` input field  

# calling_objects
The new object is now stored in an instance variable. This `@` symbol allows us to pass this instance variable around our application without worrying too much about scope. We want to now call this object and it's attributes in the `results.erb` view file.  

So far, our `results.erb` file is simply a template that we need to fill in.
```html
<!DOCTYPE html>
<html>
<head>
  <title>Results</title>
  <!-- links to CSS stylesheet in the public directory -->
  <link rel="stylesheet" href="index.css" >
</head>
<body> 

  <div class="results-page">
    
  </div>

</body>
</html>
```
Our first job is to access our object in this view. This means we need the embedded Ruby syntax
```html
<!DOCTYPE html>
<html>
<head>
  <title>Results</title>
  <!-- links to CSS stylesheet in the public directory -->
  <link rel="stylesheet" href="index.css" >
</head>
<body> 

  <div class="results-page"> 
    <h1><%= @status1 %></h1>
  </div>

</body>
</html>
```
- You did it! You've officially created an object, defined by the user, instantiated by the controller, and passed to the view using a post HTTP method! This is the primary functionality of all MVC web applications!
- The thing is... all we can see is a `#`. This represents the object id. Let's use a built-in function to double check we have a unique object stored in the `@status1` instance variable.
- Note: The `.object_id` method is a built-in Ruby class method
```html
<!DOCTYPE html>
<html>
<head>
  <title>Results</title>
  <!-- links to CSS stylesheet in the public directory -->
  <link rel="stylesheet" href="index.css" >
</head>
<body> 

  <div class="results-page"> 
    <h1><%= @status1.object_id %></h1>
  </div>

</body>
</html>
```
- Progress! We have confirmed we have a uniquely stored object! If you refresh your browser, you'll notice the object id changes as it is a unique instance 
- Don't forget -- we have 2 attributes we can call on our object: `username` and `status`
- Let's add those now...
```html
<!DOCTYPE html>
<html>
<head>
  <title>Results</title>
  <!-- links to CSS stylesheet in the public directory -->
  <link rel="stylesheet" href="index.css" >
</head>
<body> 

  <div class="results-page"> 
    <h1><%= @status1.username %>: <%= @status1.status %></h1>
  </div>

</body>
</html>
```
- Our application has officially posted our username and status that was defined by the user in the homepage! The possibilities are endless now that you can pass around data this easily!
- Let's break that line of code down:
  - We used a `<h1>` tag to render this data
  - An `=` sign was used in our erb syntax because we wanted to *view* the data on the browser screen
  - The `:` is outside of the erb syntax as this is being rendered as plain HTML

# CSS
Let's add a little CSS to make our application look nicer. Open the `index.css` file inside the `public` folder on the left side of your screen where all the files and folders are located. 
- Feel free to add your own style options that are different than mine (colors, images, etc.)

```css
body {
  margin:0;
  padding:0;
  height:100%;
}
```
- This is safe code to add to all stylesheets as it gets rid of unwanted white space around the edges of your website. 
- Next let's add a background image and make sure it is full screen 

```css
body {
  margin:0;
  padding:0;
  height:100%;
}

.home-page {
  background: url("http://coolwallpaper.website/wp-content/uploads/2016/11/Nice-New-York-City-Skyline-Wallpaper-of-awesome-full-screen-HD-wallpapers-to-download-for-free.-You-can-also-upload-and-share-your-favorite-full-screen-HD-wallpapers-quality-sofas.jpg");
}
```
- I chose a New York City backgorund however it is a bit too big. Let's add a little code to make sure it fits your screen. And while we're at it, let's center align our text and form.

```css
body {
  margin:0;
  padding:0;
  height:100%;
}

.home-page {
  background: url("http://coolwallpaper.website/wp-content/uploads/2016/11/Nice-New-York-City-Skyline-Wallpaper-of-awesome-full-screen-HD-wallpapers-to-download-for-free.-You-can-also-upload-and-share-your-favorite-full-screen-HD-wallpapers-quality-sofas.jpg") no-repeat center center fixed;
  height: 100%;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  text-align:center;
}
```
- Next let's change the color and size of our text. I'll choose white. Also, I'm going to push the text down the screen a little bit so it isn't smushed up against the top.
```css
html, body {
  margin:0;
  padding:0;
  height:100%;
}

.home-page {
  background: url("http://coolwallpaper.website/wp-content/uploads/2016/11/Nice-New-York-City-Skyline-Wallpaper-of-awesome-full-screen-HD-wallpapers-to-download-for-free.-You-can-also-upload-and-share-your-favorite-full-screen-HD-wallpapers-quality-sofas.jpg") no-repeat center center fixed;
  height: 100%;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  text-align:center;
}

h1 {
  color: white;
  font-size: 4vmax;
  font-family: Arial;
  padding-top: 12%;
  margin:0;
}
p {
  color: white;
  font-size: 2vmax;
  font-family; Arial;
  margin:0;
}
```
- Lastly, let's style the submit button. I'm going to give it a transparent background, border, and update the font style. Let's check out the final HTML and CSS code:

# Final CSS
```css
html, body {
  margin:0;
  padding:0;
  height:100%;
}

.home-page {
  background: url("http://coolwallpaper.website/wp-content/uploads/2016/11/Nice-New-York-City-Skyline-Wallpaper-of-awesome-full-screen-HD-wallpapers-to-download-for-free.-You-can-also-upload-and-share-your-favorite-full-screen-HD-wallpapers-quality-sofas.jpg") no-repeat center center fixed;
  height: 100%;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  text-align:center;
}

h1 {
  color: white;
  font-size: 4vmax;
  font-family: Arial;
  padding-top: 12%;
  margin:0;
}

p {
  color: white;
  font-size: 2vmax;
  font-family: Arial;
  margin:0;
}

input[type="submit"] {
  border: 3px solid white;
  color: white;
  font-family: Arial;
  font-size: 1vmax;
  width:25%;
  height:10%;
  background:transparent;
}

```

# Final HTML (homepage)
```html
<!DOCTYPE html>
<html>
<head>
  <title>Title</title>
  <!-- links to CSS stylesheet in the public directory -->
  <link rel="stylesheet" href="index.css" >
</head>
<body> 

<div class="home-page">

  <h1>Post a status!</h1>
  <form method="POST" action="/results">
    <p>Username: <input type="text" name="username"></p>
    <p>Status: <input type="text" name="status"></p>
    <input type="submit">
  </form>

</div>

</body>
</html>
```
- Feel free to change any values you like!


#### You now have the knowledge and foundation to build any number of applications! Though this is a much, much simpler version, this structure is similar to how any social app might post a status!  
![6](http://i.imgur.com/Zo0CCLP.gif)  

## Navigation  
##### Next lesson: [Convert to MVC](https://github.com/Coderdotnew/intro_web_apps_goodyear/tree/master/09_class/04_convert_to_mvc)  
##### Previous lesson: [Sinatra File Structure](https://github.com/Coderdotnew/intro_web_apps_goodyear/tree/master/09_class/02_sinatra_file_structure) 
---  
[Course home](https://github.com/Coderdotnew/intro_web_apps_goodyear)

