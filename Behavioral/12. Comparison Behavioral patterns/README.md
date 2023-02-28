# Comparison Behavioral patterns

## Strategy and Visitor
The Strategy and Visitor pattern can be both used to apply algorithmic logic to an existing object, without direct modification. Both patterns comply to the Open Closed Principle. Their difference can be identified in their details:
1. Participant relation:
    - The Strategy pattern has an one to many relationship. A single Context is injected with multiple Strategies. Each Strategy has its own class.
    - The Visitor pattern is a many to many relationship. A hierarchy of Elements can have multiple Visitors, however a single Visitor can provide functionality for a complete class hierarchy. 
    
    Given a Context, that uses a Strategy. When a different kind of Context is introduced, which is in favor of a slightly modified version of that Strategy, then we end-up introducing a new class to support this Strategy.

    The Visitor pattern resolves this issue on the functional level. Given a Visitor, that operates on a Context. When a different kind of Context is introduced, which is in favor of a slightly modified version of that Visitor, then we can provide this via the same Visitor using dispatching. Not introducing new classes might simplify the code. Especially with a lot of Strategies/Visitors.

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