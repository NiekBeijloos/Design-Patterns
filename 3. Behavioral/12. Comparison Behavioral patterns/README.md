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
   - The Strategy pattern enforces the client to switch the Strategy of a given Context. The client injects the Strategy in the Context. This makes the client aware and dependent on the Strategy object which makes the Strategy pattern less Single Responsible, because of a tight coupling between client and Strategy.

   - The State pattern enables a self-transioning mechanism to change the State of a given Context. The client provides a stimulus that switches the underlying State object used by the Context. This makes the client unaware and indepent of the State object. However, the switching mechanism (provided by the States) makes the States less Open Closed, because when a new State is introduced an existing State needs to change.


   **The Strategy pattern should be prefered over the State pattern when:**
    - The Strategies should be used accross different Context classes that have different transition behavior. Given two Context classes and two Strategies. During runtime each Context uses each Strategy variably. The client switches the Strategy for the respective Context. The Strategies are indepent of each other. In case the Strategies would contain self-transitioning logic, as within the State pattern, then each Context would be forced to use the same transition logic.  
    - The Strategies should be used accross multiple Context objects. The system constructs multiple Context objects in combination with a Strategy. This behavior is dependent on runtime conditions (e.g. a configuration file). This means a context is only dependent on a single Strategy and the Strategies should not have any mutual relationship. 
    
    <br/>
    
    **In short:**
    The Strategies do not have a mutual relationship, making them more reusable. In contrast, the States, in the State pattern, often have a mutual relationship making them less reusable in other parts of the system.

   **The State pattern should be prefered over the Strategy pattern when:**
   - The Client must be released of the 'burden' of changing the State (=Strategy) of the Context. This releases the coupling between Client and State making it less impactful when the State requires change. 

## Command and Observer

The Command and Observer pattern can be used to decouple a Sender from the Receiver. Both patterns provide a way to let Senders notify their Receivers when their internal structure changes. Their differences:
1. Participant relation: 
    - The Observer pattern allows an open end of Receivers to subscribe to the same Sender during runtime.
    - The Command pattern allows a single Receiver to subscribe to the same Sender during runtime.
2. Messaging mechanism:
    - The Command pattern has an 'indirect' messaging mechanism between Sender and Receiver. A message from the Sender, within the Command pattern, is embodied by a Command object. This Command object resolves the dependency constraints between Sender and Receiver. A seperate Command object, to transfer a message from Sender to Receiver, allows messages to be queued, to be undone or redone. 
    - The Observer pattern has a 'direct' messaging mechanism between Sender And Receiver. The message from the Sender to Receiver is direct, because the Receiver resides in the Sender and gets called directly when the Sender changes. 
3. Sender-Receiver decoupling:
   - The Command pattern decouples the Sender and Receiver via a seperate Command object. This allows the Receiver to be unaware of the Sender. This contributes to the Interface Segregation principle. The Receiver is only dependent on the information it requires from the Sender. Adherence to the ISP contributes in this case to less recompilation and stable test code of the Receiver. Additionaly, the reusability of the Receiver 'grows', because it has less dependencies making it more Single Responsible. The gain in the Receiver, could result in a shift of the ISP and SRP violation to the Command object. This can be a design trade off. 
   - The Observer pattern decouples the Sender and Receiver via an abstraction. The Receiver is aware of the Sender, in case the Sender injects itself as an argument. In this way the Receiver can request the required information from the Sender. The Sender becomes less reusable in case the Sender doesn't send itself, but the required information directly. This is because other Receivers might require different information from the Sender. The Interface Segregation principle is violated in case the Receiver depends on functionality from the Sender it does not use. The negativily impacts the test code stability and recompilation of the Receiver. Additionaly, there is less adherence to the Single Responsibility principle, that negativily impacts the Receivers reusability. 

**The Command pattern should be prefered over the Observer pattern when:**
- The message from the Sender toward the Receiver requires queuing, undoing or redoing.
- A Sender will have a single Receiver.
- Sender and receiver decoupling has high importance.

**The Observer pattern should be prefered over the Command pattern when:**
- A Sender will have an open end of Receivers. 

The patterns can of course be combined to gain the applicability of both 'worlds'. A Sender, from the Observer pattern, could use a Command to notify its Receivers.