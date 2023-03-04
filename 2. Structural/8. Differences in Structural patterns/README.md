# Comparison Structural patterns

It is hard to comprehend the difference between design patterns, because the result, benefits and/or structure are (almost) similar. It is yet important to know their differences, for clarifying intent verbally and non-verbally (=writing software). Developing software is all about intent. Intent is important for understanding. How 'well' we understand code and design impacts our vilocity and the amount of new bugs we introduce.

Adapter, Facade, Bridge, Decorator and Proxy design patterns could be viewed identical in many aspects. However, they have differences in intent. The upcoming paragraphs will first describe the characteristics of each of those patterns. This will be followed-up by a conclusion that concludes the difference(s) between the patterns. 

## Characteristics

The characteristics used to describe the structural patterns Adapter, Facade, Bridge, Decorator, Proxy are: 
- **Participant interaction:** in what way does the pattern incorporate with its environment / other objects. 
- **Participant relation:** how does the pattern relate to its environment.
-  **Nickname:** what is a synonym for the pattern. (This helps in identifying the pattern in interactions with people and code).
- **Problem:** which problem does the pattern resolve.
- **Solution:** how does the pattern resolve the problem.
- **Why:** why is the pattern a 'suitable' solution for the problem.

**Adapter:** 
- **Participant interaction:**
    - *Interface-level*: Provide 'translation' between two or more existing, incompatible interfaces.
    - *Implementation-level*: Adapter 'wraps' Adaptee, performs conversion and propogates requests to it.
- **Participant relation:** composition between Adapter and Adaptee.
- **Nickname:** 'Wrapper'
- **Problem:** Incompatibility between two existing interfaces 
- **Solution:** Convert one interface into the other 
- **Why:** 
    - Stability: overcome the necessity to change 
    - Reusability: overcome code duplication

**Facade:**
- **Participant interaction:** 
    - *Interface-level*: Provide an unified, new interface for one or more existing / new interfaces.
    - *Implementation-level*: Facade 'wraps' Subsystem classes and ensures cooperation between them.
- **Participant relation:** composition between Facade and Subsystem classes. 
- **Nickname:** -
- **Problem:** Complex interface to a sub-system (= class or a group of classes).
- **Solution:** Simplify interface according to client 'need'
- **Why:**  
    - Stability: simplify change in a sub-system (less client coupling)
    - Stability: restrict execution flow 
    - Stability: less re-compilation

**Bridge:** 
- **Participant interaction:**
    - *Interface-level*: Provide seperation between a new Abstraction and new Implementation.
    - *Implementation-level*: Bridge 'seperates' Abstraction from Implementation. A Bridge forms the relation between Abstraction and Implementation.
- **Participant relation:** the Bridge is the aggregation relationship between 'Abstraction' and 'Implementation'.
- **Nickname:** 'Handle-body'
- **Problem:** Intertwined inheritance hierachies or responsibilities
- **Solution:** Seperate higher, more abstract logic from lower level logic
- **Why:**
    - Reusability: simplify combinations between Abstractions and implementations
    - Readability: layering
    - Testability: descrease problem window
    - Stability: less re-compilation (PImpl)
    - Stability: reducue binary incompatability (PImpl)

**Decorator:** 
- **Participant interaction:**
    - *Interface-level*: Provide 'enhancement' on an existing interface.
    - *Implementation-level*: Decorator 'wraps' a Component, 'enhances' its pre and/or post conditions and propegates requests to it.
- **Participant relation:** aggregation between Component and Decorator.
- **Nickname:** 'Wrapper'
- **Problem:** Functionality extension on the (static) class level (inheritance or change) is undesirable
- **Solution:** Functionality extension on the (dynamic) object level
- **Why:**
    - Reusability: simplify combinations of responsibilities
    - Dynamically add or remove responsibilities
    - Readability: seperation of core and additional logic
    - Testability: decrease problem window
    
**Proxy:** 
- **Participant interaction:**
    - *Interface-level*: Provide 'substitution' using an existing interface.
    - *Implementation-level*: Proxy 'wraps' a Real Subject, constraints it and propegates requests to it.
- **Participant relation:** composition between Subject and Proxy.
- **Nickname:** 'surrogate' or 'placeholder'
- **Problem:** clients need a (constraining) placeholder for the 'real' subject, but do not want to or cannot 'own' placeholder logic.
- **Solution:** delegate placeholder logic to a dedicated class.
- **Why:**
    - Testability: decrease of problem window on client-side
    - Readability: extract logic from client side
    - Reusability: share proxy logic amongst multiple clients
    - Stability: overcome the change of existing code

**Composite:**
- **Interface-level:**
    - *Interface-level:* Provide interface 'unification' for individual objects and compositions.
    - *Implementation-level:* Composite 'wraps' multiple Leafs, adds pre and/or post conditions and delegates a request to them.
- **Participant relation:** aggregation between Leaf and Composite.
- **Nickname:** -
- **Problem:** Clients need to treat an individual object the same as a collection of those objects, but it is undesirable or not possible to do collection 'management' on client side. 
- **Solution:** Delegate collection 'management' to a deticated class and situate it behind the same interface as the collection item.
- **Why:**
    - Reusability: share composite logic across multiple clients
    - Readability: extract logic from client side
    - Testability: decrease problem window
    - Stability: overcome the change of existing client code

## Conclusion

The differences and similarities between Adapter, Facade, Bridge, Decorator, Proxy and Composite design pattern can be conducted from previous paragraph: Characteristics. 

The Adapter pattern is focussed on conversion of existing interfaces. The Facade is focussed on providing a new, simplified interface to an existing class or group of classes (=sub system). The Bridge pattern is focussed on seperating abstraction from implementation, so both dimensions can change uniformly. The Decorator is focussed on 'enhancing' individual objects by adding additional pre and/or post conditions on an existing interface. The Proxy is foccused on 'substitution' of the real subject with the goal to constraint, hold-place or optimize. The Composite is foccused on treating collections the same as individual objects.

The Composite and Decorator pattern have simulair structures. However, the Composite pattern is focussed on representation and the Decorator on functional enhancement.

Adapter, Facade, Bridge, Proxy, Decorator and Composite all have a form of indirection to an existing object. However, the Adapter follows an existing interface A and encapsulates existing interface B. The Facade follows new interface C and encapsulates existing interfaces A, B. The Bridge follows new interface C and encapsulates new interface D. The Proxy, Decorator and Composite follow existing interface A and encapsulate existing interface A.

What can we conduct from above alinea? 

The Bridge results only in new interfaces. It is often applied in an early development stage as opposed to its complete opposite: Adapter. The Adapter is often applied when the system is already quite 'mature'. Proxy, Decorator and Composite follow and encapsulate the same interface. However, often the Proxy has a composition relationship with the object it encapsulates and Decorator and Composite an aggregation. The Decorator and Proxy can become quite simulair in their usage despite the differences in intent. E.g. a logging Proxy, which logs requests to the encapsulated object, might as well be a logging Decorator.
