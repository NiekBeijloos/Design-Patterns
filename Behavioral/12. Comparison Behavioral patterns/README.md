# Comparison Behavioral patterns

## Strategy and Visitor
The Strategy pattern and Visitor pattern can be both applied to use different algorithms (Visitors or Strategies) on a given object, while perseving the object's interface (no modification). This means both patterns adhere to the Open Closed principle. Their difference can be identified in their details:
1. Participant relations:
    - The Strategy pattern has an one to many relationship. A single Context is injected with multiple Strategies. Each Strategy has its own class.
    - The Visitor pattern is a many to many relationship. A hierarchy of Elements can have multiple Visitors, however a single Visitor can provide functionality for a complete class hierarchy. 
   
    Given a Context, that uses a Strategy. When a different kind of Context is introduced, which is in favor of a slightly modified version of that Strategy, then we end-up introducing a new class to support this Strategy.
        
    The Visitor pattern resolves this issue on the functional level. Given a Visitor, that operates on a Context. When a different kind of Context is introduced, which is in favor of a slightly modified version of that Visitor, then we can provide this via the same Visitor using dispatching. Not introducing new classes might simplify the code. Especially with a lot of Strategies/Visitors.
