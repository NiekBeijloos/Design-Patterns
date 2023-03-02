# Comparison Behavioral patterns

## Strategy and Visitor
The Strategy and Visitor pattern can be both used to apply algorithmic logic to an existing object, without direct modification. Both patterns comply to the Open Closed Principle. Their difference can be identified in their details:
1. Participant relation:
    - The Strategy pattern has an one to many relationship. A single Context is injected with multiple Strategies. Each Strategy has its own class.
    - The Visitor pattern is a many to many relationship. A hierarchy of Elements can have multiple Visitors, however a single Visitor can provide functionality for a complete class hierarchy.

    <br/>
    
    Given a Context, that uses a Strategy. When a different kind of Context is introduced, which is in favor of a slightly modified version of that Strategy, then we end-up introducing a new class to support this Strategy.

    The Visitor pattern resolves this issue on the functional level. Given a Visitor, that operates on a Context. When a different kind of Context is introduced, which is in favor of a slightly modified version of a Visitor, then we can provide this via the same Visitor using dispatching. Not introducing new classes might simplify the code. Especially with a lot of Strategies/Visitors.

## Strategy and Template Method

The Strategy and Template pattern can be both used to change parts of an existing object, without direct modification. Both patterns comply to the Open Closed Principle. Their differences can be identified in their details:
1. Participant relation:
    - The Strategy pattern uses composition. This compliments the Strategy reusability aspect, because Strategies are loosly coupled to the Context. A Context can reuse a Strategy without being directly coupled to another Context. This compliments the Interface Segregation principle; a Context is not dependent on functionality it does not use (e.g. from another Context). Adhering to the interface seggregation principle provides less recompilation, more 'confident' when changing an interface (less search hits) and constraining clients ('role' interface). Composition is prefered here, because inheritance would glue Strategy and Context together violating ISP in case another Context solely wants to depend on the Strategy.
    - The Template Method uses inheritance. This enables the base Template Method class to access the sub class via a relative encapsulated approach. This relative encapsulated approach can be achieved via 'protected' access modifier in combination with the 'Hollywood' principle. Inheritance is prefered here, because composition would enforce exposure of internals on the public interface. This would violate encapsulation causing unwanted coupling that might impact adaptability of the system later.
  
   <br/>
    
   **The Strategy pattern should be prefered over the Template Method when:**
   - the Strategies are self contained (e.g. more then just a difference in a 'simple' return).
   - the Strategies must be reused accross different Contexts.
   
   <br/>

   **The Template Method pattern should be prefered over the Strategy pattern when:**
   - the Strategies (=Concrete Template class) are granular details (e.g. more just a difference in a 'simple' return).
   - the Strategies (=Concrete Template class) are used by a single Context(=base Template class), that needs access to the internal structure via an encapsulated approach.

## Strategy and State

The Strategy and State pattern both enable an object to change behavior during runtime. This is achieved by enbodying this behavior in another object; Strategy or State. Their differences are:
1. Participant interaction:
   - The Strategy pattern enforces the client to switch the Strategy of a given Context. The client injects the Strategy in the Context. This makes the client aware and dependent on the Strategy object making the Strategy pattern less Single Responsible, because of a tight coupling between client and Strategy.

   - The State pattern enables a self-transioning mechanism to change the State of a given Context. The client provides a stimulus that switches the underlying State object used by the Context. This makes the client unaware and indepent of the State object. However, the switching mechanism (provided by the States) makes the States less Open Closed, because when a new State is introduced an existing State needs to change.


   **The Strategy pattern should be prefered over the State pattern when:**
    - The Strategies should be used accross different Context classes that have different transition behavior. Given two Context classes and two Strategies. During runtime each Context uses each Strategy variably. The client switches the Strategy for the respective Context. The Strategies are indepent of each other. In case the Strategies would contain self-transitioning logic, as within the State pattern, then each Context would be forced to use the same transition logic.  
    - The Strategies should be used accross multiple Context objects. The system constructs multiple Context objects in combination with a Strategy. This behavior is dependent on runtime conditions (e.g. a configuration file). This means a context is only dependent on a single Strategy and the Strategies should not have any mutual relationship. 
    
    <br/>
    
    **In short:**
    The Strategies do not have a mutual relationship, making them more reusable. In contrast, the States, in the State pattern, often have a mutual relationship making them less reusable in other parts of the system.

   **The State pattern should be prefered over the Strategy pattern when:**
   - The Client must be released of the 'burden' of changing the State (=Strategy) of the Context. This releases the coupling between Client and State making it less impact full when the State requires change. 

