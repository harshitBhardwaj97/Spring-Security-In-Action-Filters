# A web app’s security begins with filters

## Overview

This document provides a summary of key concepts related to customizing filters in Spring Security, as discussed in Chapter 5, "A web app’s security begins with filters," of the book *Spring Security in Action*. The chapter covers how to extend and modify the HTTP filter chain to meet specific security requirements, including adding extra authentication steps, implementing custom authentication strategies, and auditing authentication events for tracing and security monitoring. It also focuses on `OncePerRequestFilter`, its characteristics, and how filters are 
used to customize overall security behavior.

The project includes examples of custom filters and how they can be integrated into a Spring Security-based web application.

## Summary

### A Few Observations About the `OncePerRequestFilter` Class

- **HTTP Requests Only**: The `OncePerRequestFilter` supports only HTTP requests, which is ideal for web applications.
  The advantage is that it automatically casts the types, and you receive the requests as `HttpServletRequest` and 
  `HttpServletResponse`. This simplifies the filter's implementation compared to the `Filter` interface, where you had 
  to manually cast the request and response.
  
- **Conditional Filtering**: You can implement logic to decide whether the filter should apply to a given request. Even 
  if you add the filter to the chain, you can choose to skip it for certain requests by overriding the 
  `shouldNotFilter(HttpServletRequest)` method. By default, the filter applies to all requests.

- **Asynchronous & Error Dispatch Requests**: By default, a `OncePerRequestFilter` doesn’t apply to asynchronous 
  requests or error dispatch requests. You can change this behavior by overriding the methods 
  `shouldNotFilterAsyncDispatch()` and `shouldNotFilterErrorDispatch()`.

If you find any of these characteristics of the `OncePerRequestFilter` useful in your implementation, we recommend 
using this class to define your filters.

### Customizing the Filter Chain

- **Filter Chain Basics**: The first layer of the web application architecture, which intercepts HTTP requests, is a filter chain. 
  You can customize the filter chain by adding new filters before, after, or at the position of an existing filter.

- **Multiple Filters at the Same Position**: You can have multiple filters at the same position in the chain. In this case, 
  the order in which the filters are executed is not defined unless explicitly specified.

- **Customizing Authentication & Authorization**: By changing the filter chain, you can customize authentication and 
  authorization to match the specific requirements of your application.

### Spring Security Filters in the Project

This repository contains several filters used to enhance the security of a Spring Boot application. These filters are used for:
- Validating custom headers (like `Request-Id`).
- Logging authentication events.
- Checking static authorization key (defined in `application.properties` of [`Spring-Security-Static-Key-Auth-Filter`](https://github.com/harshitBhardwaj97/Spring-Security-Static-Key-Auth-Filter.git))
- Ensuring security by adding custom filters at specific positions in the Spring Security filter chain.

### Using Submodules

This repository leverages Git submodules to manage the integration of other related projects. In particular:
- The [`Spring-Security-Request-Validation-Filter`](https://github.com/harshitBhardwaj97/Spring-Security-Request-Validation-Filter.git) repository is added as a submodule. This submodule provides a separate implementation for request validation filters used in Spring Security.

- The [`Spring-Security-Static-Key-Auth-Filter`](https://github.com/harshitBhardwaj97/Spring-Security-Static-Key-Auth-Filter.git) repository is added as a submodule. This submodule provides an implementation for static key-based authentication filters used in Spring Security.

## Conclusion

This project introduces key security concepts related to HTTP filters and how they can be used to customize Spring Security's behavior. By using the `OncePerRequestFilter`, you can easily manage the security of your web application in a clean and extensible manner.

For further details and examples, refer to the full text
in [Spring Security in Action](https://www.manning.com/books/spring-security-in-action-second-edition), by Laurentiu
Spilca.
