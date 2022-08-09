# Simple factory vs Factory method
## Context
'Factory pattern' is one of the most misused concepts within software development. 'Factory pattern' does not exist according to the book Design Patterns written by the 'Gang of four'. When people use this term, it could be that they refer to a Simple factory (which is not a 'real' design pattern) or the Factory method. Maybe it is not important to address the exact design solution we are proposing. Maybe it is sufficient to just propose the intent: seperating construction logic from our functional logic. I still believe it is valuable to know the difference, so we can be more exact in our communication.

For simplicity's sake, I'll call both the simple factory and factory method "patterns" in this article.

### Simple Factory

Simple factory helps us to seperate construction logic from client code. We do this to improve reusability and readability. Reusability, because there will be a single object creation point. This 'single' point is used by all clients instead of each client having it's own, identical dependency creation. This improves maintainability, because there is a single point that we need to adjust incase the object needs to be created differently. Readability, because we seperate 'complex' construction logic from client code. This improves readability of client code and compliments to Single Responsibility Principle.

<img src=SimpleFactoryUML.png width=60% height=60%>

### Factory method

Factory method delegates dependency creation to the concrete type of the object. The pattern is also called 'Virtual Constructor'. The pattern allows us to be flexible with the dependency(ies) the object works with. This 'flexibility' ensures the Open Closed Principle. We do not have to modify the object in case we need to work with a different kind of dependency. Instead we introduce a new concrete type. 

<img src=FactoryMethodUML.png width=60% height=60%>

## Simple Factory VS Factory method

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

## Conclusion

The key takeways of this article are simplicity, Single Responsibility and Open Closed. First observe if seperating the construction logic makes the code more readable and maintenance friendly. If not, keep the code as is. If is does, try to seperate the logic. We have now applied Simple factory. In case more logic is added to the Simple Factory, we might consider to make it more flexible to change and make it reusable. We do this by delegating the product creation to the sub-classes. We have now applied Factory method.   
