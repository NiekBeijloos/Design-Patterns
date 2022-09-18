# Simple factory vs Factory method vs Abstract Factory
## Context
'Factory pattern' is one of the most misused concepts within software development. 'Factory pattern' does not exist according to the book Design Patterns written by the 'Gang of four'. When people use this term, it could be that they refer to a Simple factory (which is not a 'real' design pattern), the Factory method or the Abstract Factory. Maybe it is not important to address the exact design solution we are proposing. Maybe it is sufficient to just propose the intent: seperating construction logic from our functional logic. I still believe it is valuable to know the difference, so we can be more exact in our communication.

For simplicity's sake, I'll call the Simple Factory a "pattern" in this article.

### Simple Factory

Simple factory helps us to seperate construction logic from client code. We do this to improve reusability and readability. Reusability, because there will be a single object creation point. This 'single' point is used by all clients instead of each client having it's own, identical dependency creation. This improves maintainability, because there is a single point that we need to adjust incase the object needs to be created differently. Readability, because we seperate 'complex' construction logic from client code. This improves readability of client code and compliments to Single Responsibility Principle.

<img src=SimpleFactoryUML.png width=60% height=60%>

### Factory Method

Factory method uses inheritance to delegate dependency creation to the concrete type of the object. The pattern is also called 'Virtual Constructor'. The pattern allows us to be flexible with the dependency(ies) the object works with. This 'flexibility' ensures the Open Closed Principle. We do not have to modify the object in case we need to work with a different kind of dependency. Instead we introduce a new concrete type. 

<img src=FactoryMethodUML.png width=60% height=60%>

### Abstract Factory

<img src=AbstractFactoryUML.png width=60% height=60%>

The main responsibility of an Abstract Factory is to create (families of related) objects. In its most simplistic form,  an Abstract Factory is a simple factory behind an interface constructing a product behind an interface. The pattern provides a single point of change, namely the concrete factory instantiation, instead of multiple. This compliments the Open Closed principle by providing stability and thus less chance of introducing a breaking change. In addition, the pattern adheres to Single Responibility, because 'complex' object creation is delegated to another object. This means clients will become more readable and we can reuse our implementation in other places of our program.

## Abstract Factory VS Simple Factory VS Factory Method

The intent of Abstract, Simple Factory and Factory Method pattern is to delegate object creation. Factory Method achieves this by using inheritance. Abstract Factory and Simple Factory achieve this by using delegation. The client uses Factory Method when product creation should be part of the client itself. The client uses Abstract Factory or Simple Factory incase object creation should be delegated to an another object. 

Factory Method is a start when we want to be flexible in our dependencies, but don't want to introduce a seperate factory class. We could descide to delegate object creation incase our object construction code becomes more complex and/or we need the construction logic in other parts of our code. Delegating object creation starts with a Simple Factory. A Simple Factory is not behind an abstraction, resulting in a client being dependent on the concrete factory type. I.e. a violation of the Open Closed principle. This can be acceptable in certain scenarios, because not using the abstraction, means a simplification in navigating and debugging the code. Abstract Factory is applicable incase the abstraction is required, because the factory implementation will likely change or must vary. 

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

## Example: Simple Factory VS Factory method

We can achieve the same goal with both patterns. We could use a Simple factory and dependency injection to make an object flexible in the product it uses. This is the intent of the Factory method. However, there are subtle differences. In the Factory method, product creation is part of the object it self. Using the Simple factory and dependency injection means the product creation is 'outsourced' and thus not part of the object itself. Product creation is more tightly coupled to the client in case we use the Factory method. Another difference is that a simple factory requires us to adapt existing code in case we want to support a new kind of concrete product. Using the Factory method, we can just introduce a new concrete type that supports another concrete product. 

**When would we use one over the other?**

It depends on the flexibility and complexity of the object creation. A simple factory, as the name states, is simple and does not require to introduce an extra layer of abstraction. Abstractions create complexity, because they result in indirection. This indirection makes it harder to navigate, debug and comprehend the code. However, abstraction provides us a way to extend the system without modifying it. Not modifying code means there is less chance that we introduce a bug or that we have to modify test code. In case we use the simple factory, it could be that we have to adjust the factory method providing us the corresponding product, but how hard is it to oversee that change? And does that really impact our test code? Maybe we can even introduce a second function that returns a different concrete product, this will give us Open Closed on the functional level. 

**So are there any motives left to use the Factory method?**

Yes! The main point lies in the Single Responsibility Principle. In case we separate creation logic from our client, our intent is to seperate responsiblities to improve readability and reusability (and thus reduce complexity). If reusability is not our intent, then readability remains. The question then is: does it really improve the readability if I seperate this construction logic from the client using it. If the answer is no, then we are over doing SRP. Overdoing SRP will result in a scattered code base which is harder to comprehend. It will be harder to connect the individual 'pieces' and try to see the whole. So the questions we need to ask our selfs are:
1. Do we need this creation logic somewhere else in our code?
2. Does it improve readability if I seperate this logic from the client using it?

That last question is really difficult to answer, because there is not a strict guideline or regulation that actually tells us what 'good' readability means. If the answer to those questions is 'no' then we should try to group the code together again. Grouping it together means we comply to single responsiblity again. In case we need flexibility in the product creation we could descide to use the Factory Method pattern. This will still clarify our intent: the creation logic belongs to this class, but as an addition we need flexibility in the products it works with.

I think the book Design Patterns provides a good example when to use the Factory Method pattern:

<img src=FactoryMethodExample(1).png width=50% height=60%>
<img src=FactoryMethodExample(2).png width=60% height=60%>

In case we want to accomplish the same using the Simple factory and Dependency injection, we will end up with the following code:

<img src=SimpleFactory.png width=50% height=60%>

Above example is clearly not desirable. We need to check the size of the arrays to not refer outside the array boundaries. Boundary checking means we add extra complexity to our code. On top of that, the 'intermitted' client code will have knowledge about rooms, walls and doors. This means a violation of the Interface Seggregation Principle. 'The 'intermitted' client is dependent on entities it does not use'. We need to recompile and/or modify this 'intermitted' client in case there is an interface change. 
