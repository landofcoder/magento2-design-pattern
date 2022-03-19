# Magento 2 Design Patterns

Magento 2 introduces a couple of interesting design patterns and solutions that make the code easier to read, better optimized and easier to work with development. We know design patterns are the reusable solution of some commonly occurring problem during our development and recommended way to write our code.

Design pattern is perfectly fine, because they are to help with solving common problems as seen fit, instead of deploying verbatim implementations.


>> References: https://symphisys.com/blog-patterns


## There are lots of design patterns available in Magento.

### Part 1: MVC

Magento utilizes a unique MVC pattern, utilizing a DOM based configuration layer. It leverages xml to drive the configuration and actions of the application on top of the regular Model-View-Controller architecture.

### Part 2: Front Controller

Magento uses the Front Controller pattern to implement workflows for it’s application. It has a single entry point (index.php) for all of it’s requests.

### Part 3: Factory

The Factory Method is used to instantiate classes in Magento. You instantiate a class in Magento by calling an appropriate method passing an abstract name representing a class group followed by a class name. Class groups and their appropriate abstractions are declared in your configuration XML files in your module’s /etc/ folder.

### Part 4: Singleton

Much like factory class abstraction and class groups in Magento, the Singleton pattern is instantiated for Blocks and Classes just the same.

### Part 5: Registry

The registry pattern is basically a pattern that allows any object or data to be available in a public global scope for any resource to use.

### Part 6: Prototype

The Prototype pattern in Magento is used as an extension of the Abstract Factory pattern. It ensures that an appropriate subclass is instantiated via appropriate types that are assigned to an object. What does this mean? Basically, it means that whenever you need to get a specific class that is defined via its parent type, the prototype pattern ensures you get the right class that can handle what you need.

### Part 7: Object Pool

The Object Pool Pattern keeps objects ready for use over and over again instead of re-instantiating them and destroying them once finished. It is a great way to save on memory consumption and compute cycles.

### Part 8: Iterator

The Iterator Pattern is a design pattern that allows an object to traverse through the elements of another class. This allows you to specify an iterator and allow for multiple different sets of data to be passed without changing the underlying structure that allows the iteration.

### Part 9: Lazy Loading

Lazy Loading is a design pattern that delays the loading of an object until the time that the object is called upon. With Magento, they don’t utilize this with objects, but data.

### Part 10: Service Locator

The service locator is a design pattern that allows a user to get a service by encapsulating the process inside an abstraction layer. This allows the user to retrieve the appropriate or best service without knowing what that service is at runtime.

### Part 11: Module

The Module Design Pattern is a form of modular programming that emphasizes the grouping of the functionality of a program into independent, interchangeable modules.

### Part 12: Observer

The observer pattern is where an event listener is set at a certain point during an application’s execution. Other components of the application can "hook" into this event listener and execute their code during this point.

### Part 13: Active record

Objects are a representation of a row in the database table. These objects should have properties that reflect the columns representing the structure of the table, and methods to allow modifications of these properties in the database.

The use of the pattern by Magento

The classes that inherit after \Magento\Framework\Model\AbstractModel class have access to load(), save() and delete() methods that allow loading, modification, creating or deleting records in a table that the class is connected with. Additionally, \Magento\Framework\Model\AbstractModel class inherits from \Magento\Framework\DataObject, which gives us access to truly magical methods setData() and getData() that are responsible for automatic mapping of columns in a database table with the properties of a given object.

### Part 14: Service Contract Design Pattern

Magento is an extension based or modular system, which allows a third-party developer to customize and overwrite core parts of its framework. These customizations may lead to several issues, for example, it will become for developers to keep track of customization done by external extensions. Thus to overcome this Magento comes up with a service contract pattern. A service contract is a set of interfaces that act as a layer between an end-user and business layer. Thus rather than directly exposing business logic for customization to end-user, a layer called service contract comes in between.

Service contracts enhance the modularity of Magento. Helps merchants for easy upgrade of Magento Ensure well-defined and durable API that other external and Magento module implements. Provide an easy way to expose business logic via REST or SOAP interfaces.

Read More: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/service-contracts/design-patterns.html

### Part 15: Object Manager

It itself consists of various pattern such as - Dependency injection, Singleton, Factory, Abstract Factory, Composite, strategy, CQRS, Decorator and many more. We will discuss some most used patterns among these. Object manager has a very big role to play, Magento prohibits the direct use of it. The object manager is responsible for implementing factory, singleton and proxy patterns. It automatically instantiates parameters in class constructors. Before moving future lets understand injectable and non-injectable objects:-

Read More: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/object-manager.html

### Part 16: Injectable Objects

They do not have their own identity such as EventManager, CustomerAccountManagementService.

### Part 17: Non-Injectable Objects

Such as customer, product etc. These entities usually have their identities and state, since they have their identities it is important to know on which exact instance of entity we have to work.

### Part 18: Dependency Injection

It is an alternative to Mage in Magento 1. It is a concept of injecting the dependent object through the external environment rather than creating them internally. Thus we will be asking for resources when our object is being created instead of creating resources when needed. This helps in future modification and testing becomes very easy by mocking required objects.

Read More: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/depend-inj.html

### Part 19: Factory Pattern Or Factory Classes:

In Magento 2 Factory classes create a layer between the object manager and business code. Factory classes need not define explicitly as they are auto-generated. We should create factory classes for non-injectable objects.

Read More: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/factories.html

### Part 20: Proxy Pattern

Proxy classes are used to work in place of another class and in Magento 2 they are sometimes used in place of resource hungry classes. To understand what proxy classes do let’s see the reason which leads to the occurrence of proxy classes. As we know Magento uses constructor injection for object creation and when we instantiate an object all the classes in its constructor will also instantiate thus leading to a chain of instantiation via a constructor, this can really slow down the process and impact the performance of an application, so to stop chain instantiation Magento uses proxy classes.

Read More: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/proxies.html

Let's see the following code:-
```
Magento\Catalog\Model\Product\Attribute\Source\Status\Proxy

Magento\Catalog\Model\Product\Link\Proxy
```

So in above code, we are using proxy classes for catalogProductStatus and productLink. When we run
```
 php bin/magento setup:di:compile 
```

Magento creates proxy classes on the fly using di.xml with some fixed conventions, thus replacing the original object with a proxy class object. Now let us look at our proxy class to understand how it is working

``Some common convention Magento follow while creation of proxy:-``

- Namespace of proxy class will be same as original (``Magento\Catalog\Model\Product\Attribute\Source\Status``)
- Proxy class only extends one object i.e, object manager
- Has magic functions such as __sleep, __wake which are invoked only on certain action and function such as __clone will make an object of original class and will provide the object only when it is needed (making use of lazy loading design pattern), thus improving the performance of application https://devdocs.magento.com/guides/v2.0/extension-dev-guide/proxies.html

### Plugins (Interceptors)

#### Overview

A plugin, or interceptor, is a class that modifies the behavior of public class functions by intercepting a function call and running code before, after, or around that function call. This allows you to substitute or extend the behaviour of original, public methods for any class or interface.

Extensions that wish to intercept and change the behavior of a public method can create a Plugin class which are referred to as plugins.

This interception approach reduces conflicts among extensions that change the behavior of the same class or method. Your Plugin class implementation changes the behavior of a class function, but it does not change the class itself. Because they can be called sequentially according to a configured sort order, these interceptors do not conflict with one another.

#### Limitations

Plugins cannot be used with any of the following:
- Final methods
- Final classes
- Non-public methods
- Static methods
- __construct
- Virtual types
- Objects that are instantiated before Magento\Framework\Interception is bootstrapped
- Objects that are not instantiated by the ObjectManager (e.g. by using new directly). https://devdocs.magento.com/guides/v2.0/extension-dev-guide/plugins.html

### ObjectManager

#### Overview

Large applications, such as the Magento application, use an object manager to avoid boilerplate code when composing objects during instantiation.

In the Magento framework, the implementation of the ObjectManagerInterface performs the duties of an object manager.

#### Responsibilities

The object manager has the following responsibilities:

Object creation in factories and proxies. Implementing the singleton pattern by returning the same shared instance of a class when requested. Dependency management by instantiating the preferred class when a constructor requests its interface. Automatically instantiating parameters in class constructors. https://devdocs.magento.com/guides/v2.0/extension-dev-guide/object-manager.html


### Routing

#### Overview

In web applications, such as Magento, routing is the act of providing data from a URL request to the appropriate class for processing. Magento routing uses the following flow:

![Magento 2 Routing](https://devdocs.magento.com/guides/v2.4/extension-dev-guide/images/magento2-request-processing.png)

Read More: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/routing.html


>> References: https://magento.stackexchange.com/questions/234961/how-many-design-patterns-does-magento-have

Thanks you for reading!
