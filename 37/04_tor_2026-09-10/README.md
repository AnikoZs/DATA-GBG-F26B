# Thymeleaf & HTML forms, Turistguide 2

## Underviser: Signe

## Beskrivelse


Vi skal arbejde med HTML forms, og I vil lære hvordan man får data fra en HTML form til en Spring Boot applikation ved brug af Thymeleaf.

I kommer til at starte på 2. del af Turistguideprojektet, som skal afleveres onsdag 25. februar.



## Forberedelse

Se afnit 9 om forms på LinkedIn Learning: [HTML Essential Training](https://www.linkedin.com/learning/html-essential-training-4/html-form-basics?resume=false&u=36836804) (13 min)

Se disse videoer om Thymeleaf og forms i [Thymeleaf Tutorial](https://www.youtube.com/watch?v=510O21xeelY&list=PLGRDMO4rOGcNhzNRdqhmrJ_RaLOtpwZiS&index=17) (30 min) på youtube:


#17 Create Handler Method to Return Register Page

#18 Design User Registration Form

#19 Submit Form and Display User Registration Form Data



## Læringsmål

- at kunne anvende HTML forms med Thymeleaf - check
- at forstå annotationen `@ModelAttribute` - 
- at kunne håndtere response/redirect i controlleren



## Overblik

- Peer instruction
- Jeg starter med at kode lille projekt
- Opgaver



## Why are we even talking about forms and post?

Sending data to a server is essential for interacting with a website user. Create a new user, booking online flight tickets, ordering a product online.



## HTML Forms

HTML forms is used for sending data to the server. it comes from physical forms like these:

![img](https://behu.gitbook.io/kea/~gitbook/image?url=https%3A%2F%2F3537223523-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MTL1nD8q978tREYfKpA%252Fuploads%252Fgit-blob-2eac696b2127d474d3c9c937fe2aa4649514f603%252Fphysical-forms.png%3Falt%3Dmedia&width=768&dpr=3&quality=100&sign=9761843c&sv=2)

## GET request vs POST request

There are quite a lot of different request types. We will focus on `GET` and `POST`:

* **GET request** - Getting information. Fx get all the information we have on the user with id 1. Or simply get the html at the `/about` url
* **POST request** - Creating new information. Fx creating a new user, making a new order, creating new facebook post.



### Creating a form

Here is an example of a form

```html
<form action="/sign-up" method="POST">
    <input type="text" name="name"/>
    <input type="tel" name="mobile" />
    <input type="checkbox" name="formal-name"/>

    <button type="submit">Submit</button>
</form>
```

There are a few things going on. Lets disect it:

`action="https://telmore.dk"` - The `action` attribute decides what url the form data should be send to.

`method="GET"` - The `method` attribute decides what kind of request to make. When posting we will mostly be using a `POST` request because we are creating a new user.

`<label for="mobile">Write your mobile</label>` This is a label that is connected to some field. It helps the user figuring out what to put into the connected field. The connection between `label` and field happens with the `for` attribute and the `id` on the field.

`type="text"` - `input` fields can have a type. There are quite a lot of [types](https://www.w3schools.com/html/html_form_input_types.asp). it can help the user and also do a bit of validation on the frontend. So fx if you specify `type="number"` then the number keyboard will come up on the users mobile.

`name="mobile"` - When we send the data to a server, then name decides the key of that field. See below. Here is the `POST` request

![img](https://behu.gitbook.io/kea/~gitbook/image?url=https%3A%2F%2F3537223523-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MTL1nD8q978tREYfKpA%252Fuploads%252Fgit-blob-d7d100e612ff522ce4aa54c228952b08659eaa8b%252Fpost-form.png%3Falt%3Dmedia&width=768&dpr=3&quality=100&sign=8c0f1b5f&sv=2)

`button type="submit"` - When the button is clicked submit the form.



#### Exercise

Consider using <https://codepen.io/> for making the html

Create an html page with a form that can submit a new social media post. It should have these fields:

* Title
* Content (the text of the social media post)
* Date
* Public/private

Answer these two questions:

* If i wanted a `label` for my field how could i do that?
* What if i wanted a `placeholder` for my input?



**Testing that your form works!**

* Go to that website that visualizes your request: <https://webhook.site>
* Where it says **Your unique URL** copy the url and put that url into the `action` attribute in the `form` you have created.
* Now when you submit the form, you should be able to see the request coming in on the <https://webhook.site>.
* In the bottom where it says `Raw Content` you should be able to see the data you sent (You should see title, content, date and public/private)

![img](https://behu.gitbook.io/kea/~gitbook/image?url=https%3A%2F%2F3537223523-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MTL1nD8q978tREYfKpA%252Fuploads%252Fgit-blob-cb21378482eb647acf40f71dd5a972bcce03ede9%252Fpost-test.png%3Falt%3Dmedia&width=768&dpr=3&quality=100&sign=6d09382a&sv=2)



## PostMapping

![img](https://behu.gitbook.io/kea/~gitbook/image?url=https%3A%2F%2F3537223523-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MTL1nD8q978tREYfKpA%252Fuploads%252Fgit-blob-377eca0953f1c4a53054ac76699275bb8930c826%252Fclient-server.png%3Falt%3Dmedia&width=768&dpr=3&quality=100&sign=26e30c82&sv=2)

Now we have figured out how to send the `POST` request (with data) to the server using forms. Now we need to figure out how to get that data in our `@controller`

```java
@PostMapping(value = "/sign-up")
@ResponseBody
public String createNewUser(@RequestParam("name") String name, @RequestParam("mobile") int age) {
    return "User created with name: " + name + " and mobile: " + mobile;
}
```

Using the `@PostMapping` notation we can use it just like the `@GetMapping` specifying a `value` that will be the endpoint.

To get data out of the `POST` request use `@RequestParam("name") String name`. `@RequestParam` specifies the key you are looking for. Remember that the `name` attribute on the field decided the key!



## ModelAttribute

Another way of getting data from a form is to use `@ModelAttribute`.

This is useful when the form data should become an object.

For example, imagine we have a `Student` class:

```java
public class Student {
    private String name;
    private String email;
    private int age;

    public Student() {
    }

    public Student(String name, String email, int age) {
        this.name = name;
        this.email = email;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

Then we can create a form like this:

```html
<form action="/create-student" method="POST">
    <input type="text" name="name"/>
    <input type="email" name="email"/>
    <input type="number" name="age"/>

    <button type="submit">Submit</button>
</form>
```

And in our controller we can receive the form data like this:

```java
@PostMapping("/create-student")
@ResponseBody
public String createStudent(
        @ModelAttribute("student") Student student) {

    return "Student created with name: " + student.getName()
            + ", email: " + student.getEmail()
            + " and age: " + student.getAge();
}
```

Here Spring looks at the form data and creates a `Student` object for us.

The important thing is that the `name` attributes in the HTML form must match the fields in the Java class.

```html
<input type="text" name="name"/>
<input type="email" name="email"/>
<input type="number" name="age"/>
```

These match the fields in the `Student` class:

```java
private String name;
private String email;
private int age;
```

So instead of writing this:

```java
@PostMapping("/create-student")
@ResponseBody
public String createStudent(
        @RequestParam("name") String name,
        @RequestParam("email") String email,
        @RequestParam("age") int age) {

    return "Student created with name: " + name;
}
```

We can write this:

```java
@PostMapping("/create-student")
@ResponseBody
public String createStudent(
        @ModelAttribute("student") Student student) {

    return "Student created with name: " + student.getName();
}
```

So the difference is:

`@RequestParam` is good when you only need a few single values from the form.

`@ModelAttribute` is good when the form represents an object, for example a `Student`, `User`, `Product` or `Post`.



## Redirect

Some times we are interested in making the user go to another website than the one he put in the url or was directed to. For this we use forwards and redirects

Using the redirect prefix we can redirect to another page: `redirect:/URL_TO_REDIRECT_TO`

```java
// Redirect with prefix redirect
@GetMapping("redirect-prefix-test-simple")
public String redirectViewPrefixSimple() {
    // adding query parameters to the redirected page
    return new String("redirect:/sign-up");
}
```

Using query parameters

```java
// Redirect with prefix redirect
@GetMapping("redirect-prefix-test")
public ModelAndView redirectViewprefix(ModelMap model) {
    // adding query parameters to the redirected page
    model.addAttribute("name", "Louise");
    return new ModelAndView("redirect:/sign-up", model);
}
```



#### Redirect behind the scenes

Below is how the redirect will work behind the scenes. What does the 302 mean?

![img](https://behu.gitbook.io/kea/~gitbook/image?url=https%3A%2F%2F3537223523-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-MTL1nD8q978tREYfKpA%252Fuploads%252Fgit-blob-ca028536c67f7ee60d28a8703d8f22700c391898%252Fredirect.png%3Falt%3Dmedia&width=768&dpr=3&quality=100&sign=ca0eae56&sv=2)

So the redirect says : "Hey browser i have actually moved this url by sending the `302` response code".

Now the browser asks: "Sound good server, but where have you moved the url to???".

The server responds: "Just look at the `response header` called `Location`. Thats where the url has been moved to!".

The browser now loads the new url found under the `Location` header!



## Post, redirect, get pattern

Good youtube video: [https://www.youtube.com/watch?v=DCC7ufuFD2w](https://www.youtube.com/watch?v=DCC7ufuFD2w)

Imagine a user submits a form and reloads the page. Now that form request will be sent twice. Resulting in two database instances.

With this new pattern a server receives a request, saves the data (`createProduct`) and then redirects the user to a confirmation page using `GET` not `POST` (`createProductPageSuccess`)

```java
@Controller
public class PostRedirectGet {
    @GetMapping("create-product")
    public String createProductPage() {
        return "create-new-product";
    }
  
    @PostMapping("create-product")
    public String createProduct(@RequestParam("title") String title, @RequestParam("price") int price, RedirectAttributes attributes) {
        attributes.addAttribute("title", title);
        attributes.addAttribute("price", price);

        return "redirect:/create-product-success";
    }

    @GetMapping("create-product-success")
    @ResponseBody
    public String createProductPageSuccess(@RequestParam String title, @RequestParam int price) {
        return "Created product: " + title + " " + price;
    }
}
```

Notice how the `POST` parameters are sent to the `create-product-success` using `RedirectAttributes`.



## Exercise time 🎉 in groups of 2-3

With AI create the following site: 

We would like to create a new social media site!

Therefore create a website where users can create a new social media post and see a list of all posts that were created. 

The site should have these url's:

| Url            | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `/submit`      | Is where a user can create a new social media post using a `form`. Use the `form` you created in the exercise earlier. In the starter there is an example of how to return an html template for a specific route. |
| `/dashboard`   | Return the json for the `titles` of the public social media posts (Thursday we will render these posts using html templates). In the starter example there is an example of how to return json from a list. |
| `/submit-post` | Where the `@PostMapping` exists. Remember to `redirect` to `/dashboard` |

This is what a post should include

* Title
* Content
* Date
* Public/private
* Something that you come up with!



### Understand the code

Look through the code and make a list of 3 improvements. Implement those improvements



### Extra feature

Now **without** AI you need to add a new feature! 

Every post should have the possibility to add comments. Create a plan for how you can implement this and implement it.



### Extra features

To give this new social media a bit of edge, add something to the social media post.

Maybe it's a site for dog lovers, so you add Dog name to the post

I would love to see a bit of creativity here :)





