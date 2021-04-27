##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 19 - Spring and Sockets

#### ______________________________________________________

# Using WebSocket to build an interactive web application:

**If you use Gradle, you need to add the following dependencies:**
```
implementation 'org.webjars:webjars-locator-core'
implementation 'org.webjars:sockjs-client:1.0.2'
implementation 'org.webjars:stomp-websocket:2.3.3'
implementation 'org.webjars:bootstrap:3.3.7'
implementation 'org.webjars:jquery:3.1.1-1'
```

* Create a Message-handling Controller
In Spring’s approach to working with STOMP messaging, STOMP messages can be routed to `@Controller` classes. For example, the GreetingController..

```
package com.example.messagingstompwebsocket;

import org.springframework.messaging.handler.annotation.MessageMapping;
import org.springframework.messaging.handler.annotation.SendTo;
import org.springframework.stereotype.Controller;
import org.springframework.web.util.HtmlUtils;

@Controller
public class GreetingController {


  @MessageMapping("/hello")
  @SendTo("/topic/greetings")
  public Greeting greeting(HelloMessage message) throws Exception {
    Thread.sleep(1000); // simulated delay
    return new Greeting("Hello, " + HtmlUtils.htmlEscape(message.getName()) + "!");
  }

}
```

* This controller is concise and simple, but plenty is going on. We break it down step by step.

The `@MessageMapping` annotation ensures that, if a message is sent to the `/hello` destination, the `greeting()` method is called.

The payload of the message is bound to a HelloMessage object, which is passed into `greeting()`.

Internally, the implementation of the method simulates a processing delay by causing the thread to sleep for one second. This is to demonstrate that, after the client sends a message, the server can take as long as it needs to asynchronously process the message. The client can continue with whatever work it needs to do without waiting for the response.

After the one-second delay, the `greeting()` method creates a `Greeting` object and returns it. The return value is broadcast to all subscribers of` /topic/greetings`, as specified in the` @SendTo` annotation. Note that the name from the input message is sanitized, since, in this case, it will be echoed back and re-rendered in the browser DOM on the client side.


### Configure Spring for STOMP messaging

Now that the essential components of the service are created, you can configure Spring to enable WebSocket and STOMP messaging.

Create a Java class named WebSocketConfig that resembles the following:

```
package com.example.messagingstompwebsocket;

import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

  @Override
  public void configureMessageBroker(MessageBrokerRegistry config) {
    config.enableSimpleBroker("/topic");
    config.setApplicationDestinationPrefixes("/app");
  }

  @Override
  public void registerStompEndpoints(StompEndpointRegistry registry) {
    registry.addEndpoint("/gs-guide-websocket").withSockJS();
  }

}
```

`WebSocketConfig` is annotated with `@Configuration` to indicate that it is a Spring configuration class. It is also annotated with `@EnableWebSocketMessageBroker`. As its name suggests, `@EnableWebSocketMessageBroker` enables WebSocket message handling, backed by a message broker.

The `configureMessageBroker()` method implements the default method in `WebSocketMessageBrokerConfigurer` to configure the message broker. It starts by calling `enableSimpleBroker()` to enable a simple memory-based message broker to carry the greeting messages back to the client on destinations prefixed with `/topic`. It also designates the `/app` prefix for messages that are bound for methods annotated with `@MessageMapping`. This prefix will be used to define all the message mappings. For example, `/app/hello` is the endpoint that the `GreetingController`.`greeting()` method is mapped to handle.

The registerStompEndpoints() method registers the /gs-guide-websocket endpoint, enabling SockJS fallback options so that alternate transports can be used if WebSocket is not available. The SockJS client will attempt to connect to /gs-guide-websocket and use the best available transport (websocket, xhr-streaming, xhr-polling, and so on).



#### ______________________________________________________

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.

###### [resource1](https://spring.io/guides/gs/messaging-stomp-websocket)
###### [resource2]()
