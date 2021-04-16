##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 11 - Spring

#### ______________________________________________________

# Create a Web Controller

* In Spring’s approach to building web sites, HTTP requests are handled by a controller. You can easily identify the controller by the `@Controller` annotation. 



```
@GetMapping("/greeting")
	public String greeting(...) {..}

```

* The `@GetMapping` annotation ensures that HTTP GET requests to `/greeting` are mapped to the `greeting()` method.

```
public String greeting(@RequestParam(name="name", required=false, defaultValue="World") String name, Model model) {..}
```

* `@RequestParam` binds the value of the query string parameter name into the name parameter of the `greeting()` method. Thi(s query string parameter is not required. If it is absent in the request, the defaultValue of World is used. The value of the name parameter is added to a Model object, ultimately making it accessible to the view template.


* The implementation of the method body relies on a view technology (in this case, `Thymeleaf`) to perform server-side rendering of the HTML. `Thymeleaf` parses the `greeting.html` template and evaluates the `th:text` expression to render the value of the `${name}` parameter that was set in the controller.
The following listing (from `src/main/resources/templates/greeting.html`) shows the `greeting.html` template:

```
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head> 
    <title>Getting Started: Serving Web Content</title> 
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
    <p th:text="'Hello, ' + ${name} + '!'" />
</body>
</html>

```

##### _____________________

## Test the Application

* Now that the web site is running, visit `http://localhost:8080/greeting`, where you should see “Hello, World!”

Provide a name query string parameter by visiting `http://localhost:8080/greeting?name=User`. Notice how the message changes from “Hello, World!” to “Hello, User!”:

This change demonstrates that the `@RequestParam` arrangement in GreetingController is working as expected. The name parameter has been given a default value of World, but it can be explicitly overridden through the query string.

##### _____________________

# Spring MVC and Thymeleaf: how to access data from templates

## Spring model attributes:

* Spring MVC calls the pieces of data that can be accessed during the execution of views `model attributes`. The equivalent term in Thymeleaf language is `context variables`.

* There are several ways of adding model attributes to a view in Spring MVC. 
Below you will find some common cases:

> Add attribute to Model via its addAttribute method:
```
    @RequestMapping(value = "message", method = RequestMethod.GET)
        public String messages(Model model) {
            model.addAttribute("messages", messageRepository.findAll());
            return "message/list";
        }
```

> Return ModelAndView with model attributes included:
```
    @RequestMapping(value = "message", method = RequestMethod.GET)
        public ModelAndView messages() {
            ModelAndView mav = new ModelAndView("message/list");
            mav.addObject("messages", messageRepository.findAll());
            return mav;
        }
```

> Expose common attributes via methods annotated with @ModelAttribute:
```
    @ModelAttribute("messages")
        public List<Message> messages() {
            return messageRepository.findAll();
        }
```

***You can access model attributes in views with Thymeleaf as follows:***
```
    <tr th:each="message : ${messages}">
            <td th:text="${message.id}">1</td>
            <td><a href="#" th:text="${message.title}">Title ...</a></td>
            <td th:text="${message.text}">Text ...</td>
        </tr>
```

##### _____________________

## Request parameters

**Let’s assume we have a @Controller that sends a redirect with a request parameter:**

```
    @Controller
        public class SomeController {
            @RequestMapping("/")
            public String redirect() {
                return "redirect:/query?q=Thymeleaf+Is+Great!";
            }
        }
```

> In order to access the q parameter you can use the param. prefix:

   ` <p th:text="${param.q}">Test</p>`

* Since parameters can be multivalued (e.g. `https://example.com/query?q=Thymeleaf%20Is%20Great!&q=Really%3F) you may access them using brackets syntax:

   ` <p th:text="${param.q[0] + ' ' + param.q[1]}" th:unless="${param.q == null}">Test</p>`


* Another way to access request parameters is by using the special `#request` object that gives you direct access to the *javax.servlet.http.HttpServletRequest* object:

    `<p th:text="${#request.getParameter('q')}" th:unless="${#request.getParameter('q') == null}">Test</p>`


##### _____________________

## Session attributes

* In the below example we add mySessionAttribute to session:

```
    @RequestMapping({"/"})
        String index(HttpSession session) {
            session.setAttribute("mySessionAttribute", "someValue");
            return "index";
        }
```

* Similarly to the request parameters, session attributes can be accessed by using the session. prefix:

   `<p th:text="${session.mySessionAttribute}" th:unless="${session == null}">[...]</p>`

Or by using #session, that gives you direct access to the javax.servlet.http.HttpSession object: `${#session.getAttribute('mySessionAttribute')}`


##### _____________________

## ServletContext attributes

The ServletContext attributes are shared between requests and sessions. 
In order to access ServletContext attributes in Thymeleaf you can use the `#servletContext.` prefix:

```
        <table>
                <tr>
                    <td>My context attribute</td>
                    <!-- Retrieves the ServletContext attribute 'myContextAttribute' -->
                    <td th:text="${#servletContext.getAttribute('myContextAttribute')}">42</td>
                </tr>
                <tr th:each="attr : ${#servletContext.getAttributeNames()}">
                    <td th:text="${attr}">javax.servlet.context.tempdir</td>
                    <td th:text="${#servletContext.getAttribute(attr)}">/tmp</td>
                </tr>
            </table>
```

##### _____________________

## Spring beans

* Thymeleaf allows accessing beans registered at the Spring Application Context with the @beanName syntax, for example:

  `  <div th:text="${@urlService.getApplicationUrl()}">...</div> `

* In the above example, `@urlService` refers to a Spring Bean registered at your context, e.g.

```
    @Configuration
        public class MyConfiguration {
            @Bean(name = "urlService")
            public UrlService urlService() {
                return () -> "domain.com/myapp";
            }
        }

        public interface UrlService {
            String getApplicationUrl();
        }
```


#### ______________________________________________________

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.