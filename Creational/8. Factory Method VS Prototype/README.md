# Factory method vs Prototype
## Context
In previous example: 'Factory method vs Simple factory', we have seen the applicability of both patterns. Prototype pattern might have crossed your mind as an alternative solution when we discussed the 'inconvenience' of dependency injection in combination with a simple factory in this example: 

<img src=SimpleFactory.png width=50% height=60%>

Prototype pattern can be a good alternative in this example. This article is about comparing Factory Method and Prototype, to view there pros and cons based on design intent.

### Factory method

Factory method delegates dependency creation to the concrete type of the object. The pattern is also called 'Virtual Constructor'. The pattern allows us to be flexible with the dependency(ies) the object works with. This 'flexibility' ensures the Open Closed Principle. We do not have to modify the object in case we need to work with a different kind of dependency. Instead we introduce a new concrete type. 

<img src=FactoryMethodUML.png width=60% height=60%>

### Prototype

Prototype pattern provides copy 'behavior' on the abstract level. The pattern is also called 'virtual copy-constructor'. The pattern allows us to be unfamilair with the concrete type, but still being able to make copies of that concrete type. This pattern compliments the Open Closed Principle. It is possible to introduce new concrete types without having to change the client.

<img src=PrototypeUML.png width=40% height=40%> 

## Factory method VS Prototype

The key differences between the Prototype pattern and Factory Method pattern are:
-  Prototype makes a copy of an object and Factory Method provides a 'new' object. This could be a major factor in design intent. 'New-ing' an object could be expensive, so cloning might be necessary to gain performance in our program.
-  Object creation (copy logic), using Prototype pattern, is part of the product itself. Object creation, using Factory Method pattern, is part of the client (factory). This can be a design choice. In case we use Factory method we are forced to know 'how' we need to construct the product. This can be difficult in case the product requires certain dependencies. However, this can also be complex incase we use the prototype pattern, because we might need to provide copy behavior for each of its dependencies.
- Clients of a Factory Method class can be unfamilair with the product(s) type(s). In case of the Prototype pattern, the client is familair with the product classes.
- Prototype pattern requires us to think about shallow and deep-copy. Implementing this behavior can be labour intensive and difficult. On the other hand, Factory Method requires us to implement a sub-class for each newly introduced product. 

Let's reconsider the example we used in the article 'Simple Factory VS Factory Method'. This is how we implemented the 'Maze' construction using the Factory Method:

<img src=FactoryMethodExample.png width=50% height=60%>

The concrete type of the MazeGameCreator creates the sub components that determine the construction of the Maze object. Let's implement this using the prototype pattern:

<img src=PrototypeExample.png width=55% height=60%>

The construction of the products is delegated to the product itself. The prototype is being cloned and used in the Maze construction. 

As seen both solutions could fit this example. 

**But which one is preferable in this example?**

I would say Factory Method. Constructing 'new' objects via de Factory Method pattern does not really impact our performance, so using Prototype for performance is redundant. The 'intermediate' client, injecting the products, is now familair with the type of these products. This makes the client more tightly coupled with the products. The 'intermediate' client being dependent on these products, while not using them directly, means a violation of Interface Seggregation principle resulting in a violation of Open Closed Principle. Our 'intermediate' client will need to change in 3 places in case the concrete products change. On top of that, if the product interfaces changes then our 'intermediate' client & PrototypeMazeGameCreater need to recompile. While using the Factory Method pattern, the client only needs to change in one place, namely where the Factory is initialized. Re-compilation will only be needed in the Factory. Both violate Open Closed in that sense, however it is 'easier' to oversee a single point of change then multiple. We could overcome the knowledge of the concrete type, by the 'intermediate' client, if we introduce a 'Prototype Registry'. However, this way we introduce an extra class that needs to be maintained and created.

## Conclusion
Prototype pattern and Factory Method could be both applicable in certain problem scenario's. However, I think there is always a perference for one over the other. First, we need to have a problem. Talking about the Factory Method or Prototype pattern, our problem is often related to a client that needs to be flexible in the product(s) it works. To concluded the preferable option, we need to ask ourselves the following questions:
1. How 'valuable' is copying an object over creating a new one? Do we exclude complexity or increase required performance?
2. Is our or will our Factory hierachy, parallel to our product hierachy, pollute or add extra complexity to the overall code?
3. How does using the Factory Method or Prototype Pattern impact the rest of our system? (E.g. as stated in prior example, our 'intermediate' client is familair with the product type(s).)
4. How much effort does it require to implement & test Copy behavior or new construction logic?

In prior example we could have combined both Factory Method and Prototype pattern to gain the performance and encapsulation. However, we should not forget that we have to implement copy behavior. Introducing 'new' behavior means we need to test and maintain it. We should avoid introducing code without 'real' intent.

## Is there a use-case to combine the two patterns?

The Factory Method pattern arises when a class requires flexibility in the products it needs to create a composite object or to provide functionality. The Prototype pattern can be seen as the product, in context of the Factory Method pattern. A product with a 'Clone' functionality. We can see the difference in encapsulation if we view these patterns in this context. In case of the Factory Method pattern, the product is encapsulated inside the class and does not have exposure to other clients. The Prototype pattern does not provide product encapsulation. The Prototype pattern is part of the product itself. Keep in mind the requirement: 'the client needs flexibility in its dependencies'. Injecting the product prototype inside a client, always requires two clients to be familair with the product. One injecting the other consuming. Incase one of the two clients should not be familair with the product, because it does not use it (violation of ISP!), we should consider using the Factory Method pattern. However, it could be that we also need the performance of copying over new-ing, because of a slow or complex object construction. This is the point where we would combine the two. Performance from Prototype pattern and encapsulation from the Factory Method Pattern. 