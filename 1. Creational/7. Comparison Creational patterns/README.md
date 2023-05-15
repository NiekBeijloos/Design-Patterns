# Comparison Creational patterns

## Simple Factory, Factory Method and Abstract Factory

### Context
'Factory pattern' is one of the most misused concepts within software development. 'Factory pattern' does not exist according to the book Design Patterns written by the 'Gang of four'. When people use this term, it could be that they refer to a Simple factory (which is not a 'real' design pattern), the Factory method or the Abstract Factory. Maybe it is not important to address the exact design solution we are proposing. Maybe it is sufficient to just propose the intent: separating construction logic from our functional logic. I still believe it is valuable to know the difference, so we can be more exact in our communication.

For simplicity's sake, I'll call the Simple Factory a "pattern" in this article.

### Simple Factory

Simple factory helps us to separate construction logic from client code. We do this to improve reusability and readability. Reusability, because there will be a single object creation point. This 'single' point is used by all clients instead of each client having its own, identical dependency creation. This improves maintainability, because there is a single point that we need to adjust in case the object needs to be created differently. Readability, because we separate 'complex' construction logic from client code. This improves readability of client code and compliments to Single Responsibility Principle.

<img src=SimpleFactoryUML.png width=60% height=60%>

### Factory Method

Factory method uses inheritance to delegate dependency creation to the concrete type of the object. The pattern is also called 'Virtual Constructor'. The pattern allows us to be flexible with the dependency(ies) the object works with. This flexibility ensures the Open Closed Principle. We do not have to modify the object in case we need to work with a different kind of dependency. Instead we introduce a new concrete type. 

<img src=FactoryMethodUML.png width=60% height=60%>

### Abstract Factory

<img src=AbstractFactoryUML.png width=60% height=60%>

The main responsibility of an Abstract Factory is to create (families of related) objects. In its most simplistic form, an Abstract Factory is a simple factory behind an interface constructing a product behind an interface. The pattern provides a single point of change, namely the concrete factory instantiation, instead of multiple. This compliments the Open Closed principle by providing stability and, thus, less chance of introducing a breaking change. In addition, the pattern adheres to Single Responsibility, because it delegates complex object creation to another object. This means clients will become more readable and, we can reuse our implementation in other places of our program.

### Abstract comparison: Factory patterns

The intent of Abstract, Simple Factory and Factory Method pattern is to delegate object creation. Factory Method achieves this by using inheritance. Abstract Factory and Simple Factory achieve this by using delegation. The client uses Factory Method when product creation should be part of the client itself. The client uses Abstract Factory or Simple Factory in case object creation should be delegated to another object. 

Factory Method is a start when we want to be flexible in our dependencies, but don't want to introduce a separate factory class. We could decide to delegate object creation in case our object construction code becomes more complex and/or we need the construction logic in other parts of our code. Delegating object creation starts with a Simple Factory. A Simple Factory is not behind an abstraction, resulting in a client depending on the concrete factory type. I.e. a violation of the Open Closed principle. This can be acceptable in certain scenarios, because not using the abstraction, means a simplification in navigating and debugging the code. Abstract Factory is applicable in case the abstraction is required, because the factory implementation will probably change or must vary. 

**Which pattern to choose when?**

Use the Factory method when:
- The construction logic of the product is only applicable to a single client
- A client doesn't need to vary the product(s) during runtime
- The construction logic is likely to change

Use the Abstract Factory when:
- The construction logic of the product is applicable to multiple clients
- A client needs to vary the product(s) during runtime
- The construction logic is likely to change

Use the Simple Factory when:
- The construction logic of the product is applicable to multiple clients
- A client needs to vary the product(s) during runtime
- The construction logic is not likely to change

### Detailed comparison: Simple Factory and Factory method

We can achieve the same goal with both patterns. We could use a Simple factory and dependency injection to make an object flexible in the product it uses. This is the intent of the Factory method. However, there are subtle differences. In the Factory method, product creation is part of the object itself. Using the Simple factory and dependency injection means the product creation is 'outsourced' and thus not part of the object itself. Product creation is more tightly coupled to the client in case we use the Factory method. Another difference is that a simple factory requires us to adapt the existing code in case we want to support a new concrete product. Using the Factory method, we are able to introduce a new concrete type that supports another concrete product.

**When should we use one over the other?**

It depends on the flexibility and complexity of the object creation. A simple factory, as the name states, is simple and does not require to introduce an extra layer of abstraction. Abstractions create complexity, because they result in indirection. This indirection makes it harder to navigate, debug and comprehend the code. However, abstraction provides us a way to extend the system without changing it. This means there is less chance that we introduce a bug or that we have to modify test code. In case we use the simple factory, it could be that we have to adjust the factory method providing us the corresponding product, but how hard is it to oversee that change? And does that really impact our test code? Maybe we can even introduce a second function that returns a different concrete product, this will give us Open Closed on the functional level. 

**So are there any motives left to use the Factory method?**

Yes! The main point lies in the Single Responsibility Principle. In case we separate creation logic from our client, our intent is to seperate responsibilities to improve readability and reusability (and thus reduce complexity). If reusability is not our intent, then readability remains. The question then is: does it really improve the readability if I separate this construction logic from the client using it. If the answer is no, then we are over doing SRP. Overdoing SRP will result in a scattered code base which is harder to comprehend. It will be harder to connect the individual 'pieces' and try to see the whole. So the questions we need to ask our selves are:
1. Do we need this creation logic somewhere else in our code?
2. Does it improve readability if I separate this logic from the client using it?

That last question is really difficult to answer, because there is not a strict guideline or regulation that actually tells us what 'good' readability means. If the answer to those questions is 'no' then we should try to group the code together again. Grouping it together means we comply to single responsibility again. In case we need flexibility in the product creation, we could decide to use the Factory Method pattern. This will still clarify our intent: the creation logic belongs to this class, but as an addition we need flexibility in the products it works with.

I think the book Design Patterns provides a good example when to use the Factory Method pattern:

<img src=FactoryMethodExample(1).png width=50% height=60%>
<img src=FactoryMethodExample(2).png width=60% height=60%>

In case we want to accomplish the same using the Simple factory and Dependency injection, we will end up with the following code:

<img src=SimpleFactory.png width=50% height=60%>

The above example is clearly not desirable. We need to check the size of the arrays to not refer outside the array boundaries. Boundary checking means we add extra complexity to our code. On top of that, the 'intermitted' client code will have knowledge about rooms, walls and doors. This means a violation of the Interface Seggregation Principle. 'The 'intermitted' client is dependent on entities it does not use'. We need to recompile and/or modify this 'intermitted' client in case there is an interface change.

## Factory method and Prototype
### Context
In previous example: 'Factory method vs Simple factory', we have seen the applicability of both patterns. Prototype pattern might have crossed your mind as an alternative solution when we discussed the 'inconvenience' of dependency injection in combination with a simple factory in this example: 

<img src=SimpleFactory.png width=50% height=60%>

Prototype pattern can be a good alternative in this example. This article is about comparing Factory Method and Prototype, to view there pros and cons based on design intent.

### Factory method

Factory method delegates dependency creation to the concrete type of the object. The pattern is also called 'Virtual Constructor'. The pattern allows us to be flexible with the dependency(ies) the object works with. This 'flexibility' ensures the Open Closed Principle. We do not have to modify the object in case we need to work with a different kind of dependency. Instead we introduce a new concrete type. 

<img src=FactoryMethodUML.png width=60% height=60%>

### Prototype

Prototype pattern provides copy 'behavior' on the abstract level. The pattern is also called 'virtual copy-constructor'. The pattern allows us to be unfamilair with the concrete type, but still being able to make copies of that concrete type. This pattern compliments the Open Closed Principle. It is possible to introduce new concrete types without having to change the client.

<img src=PrototypeUML.png width=40% height=40%> 

### Detailed comparison: Factory Method and Prototype

The key differences between the Prototype pattern and Factory Method pattern are:
-  Prototype makes a copy of an object and Factory Method provides a 'new' object. This could be a major factor in design intent. 'New-ing' an object could be expensive, so cloning might be necessary to gain performance in our program.
-  Object creation (copy logic), using Prototype pattern, is part of the product itself. Object creation, using Factory Method pattern, is part of the client (factory). This can be a design choice. In case we use Factory method we are forced to know 'how' we need to construct the product. This can be difficult in case the product requires certain dependencies. However, this can also be complex in case we use the prototype pattern, because we might need to provide copy behavior for each of its dependencies.
- Clients of a Factory Method class can be unfamilair with the product(s) type(s). In case of the Prototype pattern, the client is familair with the product classes.
- Prototype pattern requires us to think about shallow and deep-copy. Implementing this behavior can be labour intensive and difficult. On the other hand, Factory Method requires us to implement a sub-class for each newly introduced product. 

Let's reconsider the example we used in the article 'Simple Factory VS Factory Method'. This is how we implemented the 'Maze' construction using the Factory Method:

<img src=FactoryMethodExample.png width=50% height=60%>

The concrete type of the MazeGameCreator creates the sub components that determine the construction of the Maze object. Let's implement this using the prototype pattern:

<img src=PrototypeExample.png width=55% height=60%>

The construction of the products is delegated to the product itself. The prototype is being cloned and used in the Maze construction. 

As seen both solutions could fit this example. 

**But which one is preferable in this example?**

I would say Factory Method. Constructing 'new' objects via de Factory Method pattern does not really impact our performance, so using Prototype for performance is redundant. The 'intermediate' client, injecting the products, is now familair with the type of these products. This makes the client more tightly coupled with the products. The 'intermediate' client being dependent on these products, while not using them directly, means a violation of Interface Seggregation principle resulting in a violation of Open Closed Principle. Our 'intermediate' client will need to change in 3 places in case the concrete products change. On top of that, if the product interfaces changes then our 'intermediate' client & PrototypeMazeGameCreater need to recompile. While using the Factory Method pattern, the client only needs to change in one place, namely where the Factory is initialized. In case the product interface changes only re-compilation in the Factory will be needed. Both violate Open Closed in that sense, however it is 'easier' to oversee a single point of change then multiple. We could overcome the knowledge of the concrete type, by the 'intermediate' client, if we introduce a 'Prototype Registry'. However, this way we introduce an extra class that needs to be maintained and created.

### Conclusion
Prototype pattern and Factory Method could be both applicable in certain problem scenario's. However, I think there is always a perference for one over the other. First, we need to have a problem. Talking about the Factory Method or Prototype pattern, our problem is often related to a client that needs to be flexible in the product(s) it works. To concluded the preferable option, we need to ask ourselves the following questions:
1. How 'valuable' is copying an object over creating a new one? Do we exclude complexity or increase required performance?
2. Is our or will our Factory hierachy, parallel to our product hierachy, pollute or add extra complexity to the overall code?
3. How does using the Factory Method or Prototype Pattern impact the rest of our system? (E.g. as stated in prior example, our 'intermediate' client is familair with the product type(s).)
4. How much effort does it require to implement & test Copy behavior or new construction logic?

In prior example we could have combined both Factory Method and Prototype pattern to gain the performance and encapsulation. However, we should not forget that we have to implement copy behavior. Introducing 'new' behavior means we need to test and maintain it. We should avoid introducing code without 'real' intent.

### Is there a use-case to combine the two patterns?

The Factory Method pattern arises when a class requires flexibility in the products it needs to create a composite object or to provide functionality. The Prototype pattern can be seen as the product, in context of the Factory Method pattern. A product with a 'Clone' functionality. We can see the difference in encapsulation if we view these patterns in this context. In case of the Factory Method pattern, the product is encapsulated inside the class and does not have exposure to other clients. The Prototype pattern does not provide product encapsulation. The Prototype pattern is part of the product itself. Keep in mind the requirement: 'the client needs flexibility in its dependencies'. Injecting the product prototype inside a client, always requires two clients to be familair with the product. One injecting the other consuming. In case one of the two clients should not be familair with the product, because it does not use it (violation of ISP!), we should consider using the Factory Method pattern. However, it could be that we also need the performance of copying over new-ing, because of a slow or complex object construction. This is the point where we would combine the two. Performance from Prototype pattern and encapsulation from the Factory Method Pattern. 
