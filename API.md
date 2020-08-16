# async request

AJAX Requests: https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX/Getting_Started

![image-20200816005436719](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816005436719.png)

## client DOM

![image-20200816010004465](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816010004465.png)

![image-20200816005838928](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816005838928.png)

## client js

https://developers.google.com/web/updates/2015/03/introduction-to-fetch

![image-20200816005608622](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816005608622.png)

![image-20200816010546011](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816010546011.png)

update DOM after success

![image-20200816010740479](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816010740479.png)

![image-20200816010911747](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816010911747.png)

form data

![image-20200816142626869](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816142626869.png)

## server handler

![image-20200816010150820](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816010150820.png)

![image-20200816010314002](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816010314002.png)

# REST

https://restfulapi.net/

REST (REpresentational **S**tate **T**ransfer) is basically an architectural style of development having some principles

 It is architectural style for **distributed hypermedia systems** 

Guiding Principles of REST

1. **Client–server** – By separating the user interface concerns from the data storage concerns, we improve the portability of the user interface across multiple platforms and improve scalability by simplifying the server components.
2. **Stateless** – Each request from client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server. Session state is therefore kept entirely on the client.
3. **Cacheable** – Cache constraints require that the data within a response to a request be implicitly or explicitly labeled as cacheable or non-cacheable. If a response is cacheable, then a client cache is given the right to reuse that response data for later, equivalent requests.
4. **Uniform interface** – By applying the software engineering principle of generality to the component interface, the overall system architecture is simplified and the visibility of interactions is improved. In order to obtain a uniform interface, multiple architectural constraints are needed to guide the behavior of components. REST is defined by four interface constraints: identification of resources; manipulation of resources through representations; self-descriptive messages; and, hypermedia as the engine of application state.
5. **Layered system** – The layered system style allows an architecture to be composed of hierarchical layers by constraining component behavior such that each component cannot “see” beyond the immediate layer with which they are interacting.
6. **Code on demand (optional)** – REST allows client functionality to be extended by downloading and executing code in the form of applets or scripts. This simplifies clients by reducing the number of features required to be pre-implemented.

REST based services follow some of the above principles and not all

RESTful services means it follows all the above principles.

# REST API

https://academind.com/learn/node-js/building-a-restful-api-with/

![image-20200816102706013](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816102706013.png)

![image-20200816102833454](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816102833454.png)

![image-20200816103056654](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816103056654.png)

![image-20200816103454963](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816103454963.png)

![image-20200816104252736](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816104252736.png)

![image-20200816104317923](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816104317923.png)

![image-20200816105617289](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816105617289.png)

![image-20200816115145941](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816115145941.png)

![image-20200816115220566](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816115220566.png)

![image-20200816115428744](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816115428744.png)

![image-20200816115443006](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816115443006.png)

## vs MVC

![image-20200816131601271](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816131601271.png)

![image-20200816150506546](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816150506546.png)

## auth

![image-20200816144511058](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816144511058.png)

token can not be fake by the client

![image-20200816144558704](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816144558704.png)

![image-20200816150526987](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816150526987.png)

### JWT

![image-20200816145015819](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816145015819.png)

![image-20200816145137141](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816145137141.png)

'Bearer' for JWT

![image-20200816145809250](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816145809250.png)

![image-20200816145835887](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816145835887.png)



# CORS

![image-20200816114558105](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816114558105.png)

![image-20200816115025937](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816115025937.png)

![image-20200816115040781](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816115040781.png)

![image-20200816114751488](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816114751488.png)

# Rest API limit

![image-20200816180803990](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816180803990.png)

![image-20200817005749778](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200817005749778.png)

# GraphQL

https://graphql.org/learn/

![image-20200816180637655](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816180637655.png)

![image-20200816180934540](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816180934540.png)

![image-20200816180954828](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816180954828.png)

![image-20200816182045532](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816182045532.png)

![image-20200816182146230](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816182146230.png)

![image-20200816184601796](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200816184601796.png)

![image-20200817005723436](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200817005723436.png)

