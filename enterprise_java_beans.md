# Enterprise Java Beans

- Outline
  - Why EJB?
  - 3 tier architecture
  - Enterprise javabeans
  - POJO vs java beans vs ejb
  - Types of ejb
  - Session EJBs lifecycle
- From last week using the MVC methodology
  - Model - managed beans
    - However don't want to store data inside this
  - View - UI
  - Controller - FacesServlet
- 2 main containers - Web container (has the faces servlet, JSF), and the EJB container
- The `@Named` annotation makes a class into a managed bean
- The standard way to have methods on models is using an external class which implements an interface
  - The class can have the `@Stateless` annotation

## Tier Architectures

- Presentation
  - Application client and optional javabeans components
  - Web browser, web pages, applets, and optional javabeans components
  - E.g Main interface, admin interface
- Business logic
  - Javabeans components (optional)
  - Web pages servlets
  - Java persistence entities, session beans, message-driven beans
  - E.g config bean, request bean
- Data tier
  - Database and legacy systems

## POJO VS Java Beans VS EJB

- Java bean is a java class which should have
  - A public no-argument constructor
  - Private attributes
  - Public getter/setters for accessing and setting the private attributes
- JSF beans are POJO, classes which used to read the UI component value of JSF
- All java beans are POJOs but vice-versa is not true
- Difference between POJO and java beans
  - POJO can have other than private attributes whereas java beans can only have private fields
  - POJO may or may not have a constructor whereas java beans should have a no argument constructor
- Examples

POJO

```java
class StudentClass {
  public StudentClass() {}
  ...
}
```

JSF Bean

```java
@Named
@RequestScoped
class StudentJSFBean {
  public StudentJSFBean() {}
  ...
}
```

EJB Bean

```java
@Stateless
class StudentEJBBean {
  public StudentEJBBean() {}
  ...
}
```

## Enterprise JavaBeans (EJB)

- Enterprise javabeans is a platform for building potable, reusable and scalable business applications using the java programming language
- EJB bean is a server-side component that encapsulates the business logic of an application
- The business logic is the code that achieves the purpose of the application

### EJB Container

- In the EJB architecture, EJB beans are managed by the EJB container
  - It provides a runtime environment for EJB
  - It provides persistence management
  - It is responsible for the lifecycle management of EJBs
  - It is in charge of ensuring that all EJBs are secured
- You do not need to implement all the things that go with a server, like threading, polling, security, etc.

### Types Of EJB

- Message-driven
- Session
  - Stateless
    - Calls are handled by multiple bean instances, each bean being a session with a client
    - These beans are all a part of a single "pool"
    - Difference between this and the singleton bean, is that this is meant for scalability, as new beans will be created when needed
  - Stateful
    - Used to remember the client's current session (e.g for a shopping cart)
  - Singleton
    - Only one instance of a bean ever, where all requests go through the one bean's methods
    - Can only process one thing at a time

#### Stateless VS Stateful

| Stateless                                                                                                                                                         | Stateful                                                                                                                                        |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| The implementation class is marked @Stateless                                                                                                                     | The implementation class is marked @Stateful                                                                                                    |
| SsSB do not maintain state associated with any client                                                                                                             | SfSB maintain the state associated with a client                                                                                                |
| Enterprise bean container manages a pool of these beans (they all are equivalent)                                                                                 | The EJB container instantiates a stateful session bean once for each client that calls it                                                       |
| Access to a single bean instance is to only one client at a time (no concurrent access)                                                                           | Access to a single bean instance is to only one client at a time (no concurrent access)                                                         |
| If a particular web component calls a SsSB multiple times, then different instances of the enterprise bean implementation may handle these calls                  | If a particular web component calls a SfSB multiple times, then the same instance of the enterprise bean implementation will handle these calls |
| If different web components call the same SsSB, then any available instance bean can be used to service                                                           | If different web components call the same SfSB, then different instances of the bean's implementation class will handle these calls             |
| The state is retained for the duration of the invocation                                                                                                          | The state is retained for the duration of the client/bean session. If the client removes the bean, the session ends and the state disappears    |
| Clients may change the state of the instance variables in pooled stateless beans, and this state is held over to the next invocation of the pooled stateless bean |
| Example: Adding an item to the database, sending messages, and performing mathematical calculations                                                               | Example: Shopping cart                                                                                                                          |
| No scaling problem<br/>A stateless session bean can implement a web service                                                                                       | Limitation: scaling problem<br/>A stateful session bean cannot implement a web service                                                          |
