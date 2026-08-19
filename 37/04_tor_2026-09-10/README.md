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

- Kunne anvende HTML forms med Thymeleaf
- Binde modelobjekter til forms med `th:object` og `th:field`
- Modtage formdata som objekter med `@ModelAttribute`
- Behandle forskellige inputtyper
  - tekstfelter
  - options
  - checkbokse
  - knapper
- Håndtere redirect efter en `POST` request



## Overblik

- Peer instruction
- Jeg starter med at kode lille projekt ELLER GØR JEG?
  - Forklar redirect pattern
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

Here is an example of a standard HTML form

```html
<form action="/sign-up" method="POST">
    <input type="text" name="name"/>
    <input type="tel" name="mobile" />
    <input type="checkbox" name="formal-name"/>

    <button type="submit">Submit</button>
</form>
```

There are a few things going on. Let's dissect it:

`action="/sign-up"` - The `action` attribute decides what URL the form data should be sent to.

`method="POST"` - The `method` attribute decides what kind of request to make. When a form changes or creates data, we will normally use a `POST` request.

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
@PostMapping("/sign-up")
@ResponseBody
public String createNewUser(
        @RequestParam("name") String name,
        @RequestParam("mobile") String mobile) {

    return "User created with name: " + name + " and mobile: " + mobile;
}
```

Using the `@PostMapping` notation we can use it just like the `@GetMapping` specifying a `value` that will be the endpoint.

To get data out of the `POST` request use `@RequestParam("name") String name`. `@RequestParam` specifies the key you are looking for. Remember that the `name` attribute on the field decided the key!



## Binding a model object to a form

When a form represents an object, Thymeleaf can bind the form directly to a Java object.

We will use a `Student` as an example:

```java
public class Student {
    private String name;
    private String email;
    private int age;
    private String programme;
    private boolean fullTime;

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

    public String getProgramme() {
        return programme;
    }

    public void setProgramme(String programme) {
        this.programme = programme;
    }

    public boolean isFullTime() {
        return fullTime;
    }

    public void setFullTime(boolean fullTime) {
        this.fullTime = fullTime;
    }
}
```



### 1. Put the object in the model

Before Thymeleaf can bind the form, the controller needs to put a `Student` object in the model:

```java
@GetMapping("/create-student")
public String showCreateStudentForm(Model model) {
    model.addAttribute("student", new Student());
    return "create-student";
}
```

The name `"student"` is the name we use to refer to this object from the Thymeleaf template.



### 2. Bind the form to the object

In the HTML template we use `th:object` to bind the whole form to the `Student` object:

```html
<form th:action="@{/create-student}"
      th:object="${student}"
      method="post">

    <input type="text" th:field="*{name}" />
    <input type="email" th:field="*{email}" />
    <input type="number" th:field="*{age}" />

    <button type="submit">Submit</button>
</form>
```

`th:action="@{/create-student}"` is Thymeleaf's way of generating the URL for the form action

`th:object="${student}"` means: **this form represents the `student` object**.

`th:field="*{name}"` means: **this field is bound to the `name` property on that object**.

Thymeleaf generates the corresponding `name`, `id` and `value` attributes for us. For example:

```html
<input type="text" th:field="*{name}" />
```

will result in HTML similar to:

```html
<input type="text" id="name" name="name" value="" />
```

Notice that `th:field="*{name}"` must match a property on the `Student` class. The model attribute name `student` does **not** need to have the same name as the fields.

### Checkboxes and options

`th:field` also works with other form controls. For example, the `Student` class above has these two properties:

```java
private String programme;
private boolean fullTime;
```

We can bind a `<select>` with several `<option>` elements to `programme`:

```html
<label for="programme">Programme</label>
<select th:field="*{programme}">
    <option value="">Choose programme</option>
    <option value="Datamatiker">Datamatiker</option>
    <option value="IT-arkitektur">IT-arkitektur</option>
    <option value="Multimediedesigner">Multimediedesigner</option>
</select>
```

When the user selects an option, its `value` becomes the value of `student.programme`.

A checkbox can be bound directly to a `boolean` property:

```html
<label>
    <input type="checkbox" th:field="*{fullTime}" />
    Full-time student
</label>
```

If the checkbox is checked, `student.fullTime` becomes `true`. If it is not checked, it becomes `false`.

One advantage of using `th:field` is that Thymeleaf handles details such as `name`, `value`, `checked` and `selected` for the bound field.

The form could therefore contain different kinds of fields that all bind to the same object:

```html
<form th:action="@{/create-student}"
      th:object="${student}"
      method="post">

    <input type="text" th:field="*{name}" />
    <input type="email" th:field="*{email}" />
    <input type="number" th:field="*{age}" />

    <select th:field="*{programme}">
        <option value="">Choose programme</option>
        <option value="Datamatiker">Datamatiker</option>
        <option value="IT-arkitektur">IT-arkitektur</option>
        <option value="Multimediedesigner">Multimediedesigner</option>
    </select>

    <label>
        <input type="checkbox" th:field="*{fullTime}" />
        Full-time student
    </label>

    <button type="submit">Submit</button>
</form>
```



### 3. Receive the object with `@ModelAttribute`

When the form is submitted, Spring can bind the submitted values back into a `Student` object:

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



### `@RequestParam` vs `@ModelAttribute`

If we did not bind the data to an object, we could receive each value individually:

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

But when the form represents one object, this is usually simpler:

```java
@PostMapping("/create-student")
@ResponseBody
public String createStudent(
        @ModelAttribute("student") Student student) {

    return "Student created with name: " + student.getName();
}
```

`@RequestParam` is useful when you only need a few individual values from the request.

`@ModelAttribute` is useful when the form represents an object, for example a `Student`, `User`, `Product` or `Post`.




## Redirect

Sometimes, after handling a request, we want the browser to make a new request to another URL. In Spring MVC we can do this with a redirect.

Using the `redirect:` prefix we can redirect to another URL: `redirect:/URL_TO_REDIRECT_TO`

```java
// Redirect with prefix redirect
@GetMapping("redirect-prefix-test-simple")
public String redirectViewPrefixSimple() {
    // adding query parameters to the redirected page
    return "redirect:/sign-up";
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

Imagine a user submits a form and then refreshes the result of that `POST` request. The browser may submit the form again, which can create duplicate data.

With the Post/Redirect/Get pattern, the server receives the `POST`, saves the data, and then redirects the browser. The browser follows the redirect with a new `GET` request.

```java
@Controller
public class PostRedirectGet {

    @GetMapping("/create-product")
    public String createProductPage(Model model) {
        model.addAttribute("product", new Product());
        return "create-new-product";
    }

    @PostMapping("/create-product")
    public String createProduct(
            @ModelAttribute("product") Product product) {

        // Save the product here

        return "redirect:/dashboard";
    }
}
```

The important part is that the `POST` handler does its work and then returns a redirect. The browser then makes a new `GET` request to `/dashboard`.

This avoids the browser resubmitting the same `POST` request if the user refreshes the page.



## Exercise time 🎉 in groups of 2-3

With AI create the following site: 

We would like to create a new social media site!

Therefore create a website where users can create a new social media post and see a list of all posts that were created. 

The site should have these url's:

| Url            | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `/submit`      | Show a form for creating a social media post. Put a new `Post` object in the model and bind the form using `th:object` and `th:field`. |
| `/dashboard`   | Return the json for the `titles` of the public social media posts (Thursday we will render these posts using html templates). In the starter example there is an example of how to return json from a list. |
| `/submit-post` | Where the `@PostMapping` exists. Receive the submitted post using `@ModelAttribute` and remember to `redirect` to `/dashboard`. |


Your solution should therefore contain:

* A `Post` class with fields matching the form data
* `model.addAttribute("post", new Post())` in the `GET /submit` handler
* `th:object="${post}"` on the form
* `th:field="*{...}"` on the form fields
* A checkbox bound to the public/private property
* A `<select>` with at least three `<option>` elements for one of your other properties, for example a category
* `@ModelAttribute("post") Post post` in the `POST /submit-post` handler
* A redirect to `/dashboard` after the post has been created

This is what a post should include

* Title
* Content
* Date
* Public/private
* Something that you come up with!



### Understand the code

Look through the code and make a list of 3 improvements. Implement those improvements. **No AI!**



### Extra feature

Now **without** AI you need to add a new feature! 

Every post should have the possibility to add comments. Create a plan for how you can implement this and implement it.



### Extra features

To give this new social media a bit of edge, add something to the social media post.

Maybe it's a site for dog lovers, so you add Dog name to the post

I would love to see a bit of creativity here :)
