# Chat Memory Project

Welcome to the Chat Memory project! This Spring Boot application demonstrates how to create a simple chatbot with memory capabilities using Spring AI and OpenAI. By leveraging in-memory chat storage, our bot can maintain context across multiple interactions, providing a more engaging and coherent conversation experience.

## Project Overview

The Chat Memory project showcases the integration of Spring Boot with Spring AI to create an intelligent chatbot. It uses OpenAI's powerful language models to generate responses while maintaining conversation history in memory.

## Project Requirements

- Java 23
- Maven 3.6+
- Spring Boot 3.3.4
- Spring AI 1.0.0-M3
- An OpenAI API key

## Dependencies

This project relies on the following main dependencies:

- `spring-boot-starter-web`: For creating RESTful web services
- `spring-ai-openai-spring-boot-starter`: To integrate Spring AI with OpenAI
- `spring-boot-starter-test`: For testing purposes

## Getting Started

To get started with the Chat Memory project, follow these steps:

1. Ensure you have Java 23 and Maven installed on your system.
2. Set up your OpenAI API key as an environment variable:
   ```
   export SPRING_AI_OPENAI_API_KEY=your_api_key_here
   ```
3. Build the project using Maven:
   ```
   mvn clean install
   ```

## How to Run the Application

To run the application, use the following command in the project root directory:

```
mvn spring-boot:run
```

Once the application is running, you can interact with the chatbot by sending GET requests to the root endpoint (`/`) with a `message` parameter.

Example using curl:

```
curl "http://localhost:8080/?message=Hello, who are you?"
```

## Relevant Code Examples

Let's dive into some key parts of the code to understand how the Chat Memory project works.

### Main Application Class

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

This is the entry point of our Spring Boot application. It uses the `@SpringBootApplication` annotation to enable auto-configuration and component scanning.

### Chat Controller

The `ChatController` class is where the magic happens:

```java
@RestController
public class ChatController {
    private final ChatClient chatClient;

    public ChatController(ChatClient.Builder builder) {
        this.chatClient = builder
                .defaultAdvisors(new MessageChatMemoryAdvisor(new InMemoryChatMemory()))
                .build();
    }

    @GetMapping("/")
    public String home(@RequestParam String message) {
        return chatClient.prompt()
                .user(message)
                .call()
                .content();
    }
}
```

Let's break down this controller:

1. We use constructor injection to get a `ChatClient.Builder`.
2. In the constructor, we build a `ChatClient` with a `MessageChatMemoryAdvisor` that uses `InMemoryChatMemory`. This setup allows our chatbot to remember previous interactions.
3. The `home` method handles GET requests to the root endpoint. It takes a `message` parameter and uses the `ChatClient` to generate a response.

The use of `InMemoryChatMemory` is crucial here. It allows the chatbot to maintain context across multiple interactions within the same session.

## Conclusion

The Chat Memory project demonstrates a powerful yet straightforward way to create a chatbot with memory using Spring Boot and Spring AI. By leveraging OpenAI's language models and implementing in-memory chat storage, we've created a bot that can maintain context and provide more engaging conversations.

This project serves as an excellent starting point for developers looking to build more complex conversational AI applications. Feel free to explore, modify, and expand upon this code to create your own unique chatbot experiences!

Happy coding, and may your chatbots always remember the important stuff! 🤖💬🧠