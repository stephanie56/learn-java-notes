# Chapter 2 Developing web application

Book: Spring in Action

In a Spring application, it’s a controller’s job to fetch and process data. And it’s a view’s job to render that data into HTML that will be displayed in the browser. In a typical Spring MVC application, you will create the following components in support of the taco creation page:

- A domain class that defines the properties of a taco ingredient
- A Spring MVC controller class that fetches ingredient information and passes it along to the view, or write data directly to the body of a response (RESTful)
- A view template that renders a list of ingredients in the user’s browser

## What is domain?

An application’s domain is the subject area that it addresses - the ideas and concepts that influence the understanding of the application. In the Taco Cloud application, the domain includes such objects as taco designs, the ingredients, customers, and taco orders placed by the customers.

For the Taco application, we will write a simple controller that will do the following:

- Handle HTTP GET requests where the request path is /design
- Build a list of ingredients
- Hand the request and the ingredient data off to a view template to be rendered as HTML and sent to the requesting browser
- Q: so a controller is an equivalent to a middleware in NodeJS? It processes data and passes the request to the next middleware.

## How to handle requests

- @RequestMapping is a general-purpose request handler
- @GetMapping is equivalent to `@RequestMapping(method=RequestMethod.GET)`
- @PostMapping: handles HTTP POST requests
- @PutMapping: handles HTTP PUT requests
- @DeleteMapping: handles HTTP DELETE requests
- @PatchMapping: handles HTTP PATCH requests
- Generally, only use @RequestMapping at the class level to specify the base path
- Use the more specific @GetMapping, @PostMapping and so on, on each of the handler methods

## The Model object

- Model is an object that ferries data between a controller and whatever view is charged with rendering that data.
- Data that is placed in Model attributes is copied into the servlet response attributes, where the view can find them
- The controller handler method returns the name of the view that will be used to render the model to the browser

GET Request -> Controller fetch data from the repository -> Controller place data in the model -> pass the request on to the view

## The View

- Spring offers several options for defining views, including JSP, Thymeleaf, FreeMarker, Mustache, and Groovy-based templates
- View libraries are designed to be decoupled from any particular web framework - as such, they are unaware of Spring’s model abstraction and are unable to work with the data that the controller placed in Model
- View templates work directly with servlet request attributes
- Before Spring hands the request over to a view, it copies the model data into request attributes that view templates have access to

## Add method to handle form submissions

- On the view: the form set its method attribute to POST, and it doesn’t declare the action attribute
- This means that when the form is submitted, the browser will gather up all the data in the form and send it to the server in an HTTP POST request to the same path for which a GET request displayed the form - the /design path

```java
@PostMapping
Public String processDesign(Design design) {
	// save the taco design
	log.info(“processing design, ” + design);
	return “redirect:/order/current”;
}
```

- The processDesign() method returns the name of a view that will be shown to the user. It’s prefixed with “redirect:”, indicating that this is a redirect view, the relative path ‘/order/current’

## Form Validation from the Server Side

- The Bean Validation API (known as JSR-303) makes it easy to declare validation rules
- The validation APIs are added to the project automatically as dependencies of Spring Boot’s web starter
- To apply validation in Spring MVC, you need to:

1. Declare validation rules on the class that is to be validated
2. Specify that validation should be performed in the controller methods that require validation
3. Modify the form views to display validation errors

### Gotchas:

1. If the form is validated on frontend, but fails the validation on backend, is form submission is not successful, therefore we will need to check both cases
2. Async validators on the front end

```java
@Data
Public class Taco {
  @NotNull
  @Size(min=5, message=“Name must be at least 5 characters long”)
  private String name;

  @Size(min=1, message=“You must choose at least 1 ingredient”)
  private List<String> ingredients;

  @CreditCardNumber(message=“Not a valid credit card”)
  private String ccNumber;

  @Pattern(regexp=“^(0[1-9] | 1[0-2]) ([\\/]) ([1-9] [1-9])$”, message=“Must be formatted MM/YY”)
  private String ccExpiration

  @Digits(integer=3, fraction=0, message=“Invalid CVV”)
  private String ccCVV;
}
```

- The @CreditCardNumber annotation declares that the property’s value must be a valid credit card number that passes the Luhn algorithm check
- Custom validator can be implemented by using @Pattern annotation, providing it with a regular expression that ensures that the property value adheres to the desired format
- The @Digits annotation is to ensure that the value contains exactly there numeric digits

## Performing validation at form binding

- Specify that validation should be performed when the forms are POSTed to their respective handler methods
  - To validate a submitted Taco, you need to add the JSR-303’s @Valid annotation to the Taco argument of the handler method
  - The @Valid annotation tells Spring MVC to perform validation on the submitted Taco object after it’s bound to the submitted form data and before the POST handler method is called.
  - If there are any validation errors, the details of those errors will be captured in an Error object that is passed into the handler method.
  - The first few lines of the method consult the Errors object, asking its hasErrors() method if there are any validation errors
  - If there are errors, the method concludes without processing the Taco and returns the “design” view name so that the form is redisplayed.

```java
@PostMapping
public String processDesign(@Valid Taco design, Errors errors) {
	if (errors.hasErrors()) { return “design”; }
	return “redirect:/order/current”
}
```

## Working with view controllers

- When a controller is simple enough that it doesn’t populate a model or process input - as is the case with a HomeController - you can declare a view controller instead - a controller that does nothing but forward the request to a view.

```java
@Configuration
public Class WebConfig implements WebMvcConfigurer {
  @Override
  public void addViewControllers(ViewControllerRegistry registry) {
    registry.addViewController(“/”).setViewName(“home”);
    }
}
```

- The WebMvcconfigurer defines several methods to configure Spring MVC.
- Even though it’s an interface, it provides default implementations of all the methods, so you only need to override the methods you need.
- The addViewControllers() method is given a ViewControllerRegistry that you can use to register one or more view controllers
- Any configuration class can implement WebMvcConfigurer and override the addViewController method. For instance, you could have added the same view controller declaration to the bootstrap application class

```java
@SpringBootApplication
public class TacoApplication implements WebMvcConfigurer {
  public static void main(String[] args) {…}

  @Override
  public void addViewControllers(ViewControllerRegistry registry) {
    registry.addViewController(“/”).setViewName(“home”);
    }
}
```
