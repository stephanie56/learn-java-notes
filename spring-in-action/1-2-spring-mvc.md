# Chapter 1-2: Spring MVC

## The Core of Spring MVC

- At the centre of Spring MVC is the concept of a controller, a class that handles requests and responds with information of some sort.
- In the case of a browser-facing application, a controller responds by optionally:
- a. populating model data and,
- b. passing the request on to a view to product HTML that is returned to the browser.

## How a controller looks like:

```
@Controller
public class HomeController {
@GetMapping(“/”)
String home() {return “home”;}
}

```

1. @Controller: all the controller classes are decorated with the @Controller annotation, which primarily identifies this class as a component for component scanning. With this annotation, Spring’s component scanning automatically discovers it and creates an instance of HomeController as a bean in the Spring application context.
2. Other annotations, such as @Component, @Service and @Repository serve the same purpose as @Controller. However, the choice of @Controller is more descriptive of this component’s role in the application.
3. @GetMapping: indicate that if an HTTP GET request is received for the root path “/”, then this method should handle that request.

## Testing the controller

- For the purpose of the home page, you will write a test that’s comparable in complexity to the homepage itself. Your test will perform an HTTP GET request for the root path “/” and expect a successful result where the view name is home and the resulting content contains the phrase “Welcome to…”

```
@RunWith(SpringRunner.class)
@WebMvcTest
public class HomeControllerTest {
@Autowired private MockMvc mockMvc;

@Test
public void testHomePage() throws Exception {
mockmvc.perform(get(“/”))
.andExpect(status().isOk())
.andExpect(view().name(“home”))
.andExpect(content().string(containString(“Welcome to….”)));
}
}
```

- Instead of the @SpringBootTest, HomeControllerTest is annotated with @WebMvcTest. This is a special test annotation provided by Spring Boot that arranges for the test to run in the context of a Spring MVC application. In this case, it arranges for HomeController to be registered in Spring MVC so that you can throw requests against it.
- @WebMvcTest could be made to start a server, we still need to mock the mechanics of Spring MVC. The test class is injected with a MockMvc object for the test to drive the mockup.
