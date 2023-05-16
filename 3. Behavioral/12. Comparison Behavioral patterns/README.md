# Comparison Behavioral patterns

## Strategy and Visitor
The Strategy and Visitor pattern can be both used to apply algorithmic logic to an existing object, without direct modification. Both patterns comply to the Open Closed Principle. Their difference can be identified in their details:
1. Participant relation:
    - The Strategy pattern has an one to many relationship. A single Context is injected with multiple Strategies. Each Strategy has its own class.
    - The Visitor pattern is a many to many relationship. A hierarchy of Elements can have multiple Visitors, however a single Visitor can provide functionality for a complete class hierarchy.
    
    **Strategy pattern case:** <br/>
    Given: a Context that uses a Strategy <br/>
    When: a different kind of Context is introduced, which is in favor of a slightly modified version of that Strategy <br/>
    Then: we end-up introducing a new class to support this Strategy <br/>

    **The Visitor pattern resolves this case on the functional level:** <br/>
    Given: a Visitor, that operates on a Context <br/>
    When: a different kind of Context is introduced, which is in favor of a slightly modified version of that Visitor <br/>
    Then: we add a function to that Visitor and make use of dispatching<br/>
    
    The Visitor pattern might require significant fewer classes to resolve a problem as opposed to applying the Strategy design pattern.

## Strategy and Template Method

The Strategy and Template pattern can be both used to change parts of an existing object, without direct modification. Both patterns comply to the Open Closed Principle. Their differences can be identified in their details:
1. Participant relation:
    - The Strategy pattern uses composition. This compliments the Strategy reusability aspect, because Strategies are loosly coupled to the Context. A Context can reuse a Strategy without being directly coupled to another Context. This compliments the Interface Segregation principle; a Context is not dependent on functionality it does not use (e.g. from another Context). Adhering to the interface seggregation principle provides less recompilation, more 'confident' when changing an interface (less search hits) and constraining clients ('role' interface). Composition is prefered here, because inheritance would glue Strategy and Context together violating ISP in case another Context solely wants to depend on the Strategy.
    - The Template Method uses inheritance. This enables the base Template Method class to access the sub class via a relative encapsulated approach. This relative encapsulated approach can be achieved via 'protected' access modifier in combination with the 'Hollywood' principle. Inheritance is prefered here, because composition would enforce exposure of internals on the public interface. This would violate encapsulation causing unwanted coupling that might impact adaptability of the system later.
    
   **The Strategy pattern should be prefered over the Template Method when:**
   - the Strategies are self contained (e.g. more then just a difference in a 'simple' return).
   - the Strategies must be reused accross different Contexts.

   **The Template Method pattern should be prefered over the Strategy pattern when:**
   - the Strategies (=Concrete Template class) are granular details (e.g. just a difference in a 'simple' return).
   - the Strategies (=Concrete Template class) are used by a single Context (=base Template class), that needs access to the internal structure via an encapsulated approach.

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

The Command and Observer pattern both prescribe decoupling Sender from the Receiver to gain reusability and stability of the code base. Both patterns provide a way to let Senders notify their Receivers when their internal structure changes. Their differences:
1. Participant relation: 
    - The Observer pattern allows an open end of Receivers to be notified by the same Sender during runtime.
    - The Command pattern allows a single Receiver to be notified by the same Sender during runtime.
2. Messaging mechanism:
    - The Command pattern has an 'indirect' messaging mechanism between Sender and Receiver. A message from the Sender, within the Command pattern, is embodied by a Command object. This Command object resolves the dependency constraints between Sender and Receiver. A seperate Command object, to transfer a message from Sender to Receiver, allows messages to be queued, to be undone or redone. 
    - The Observer pattern has a 'direct' messaging mechanism between Sender and Receiver. The message from the Sender to Receiver is direct, because the Receiver resides in the Sender and gets called directly when the Sender changes. 
3. Sender-Receiver decoupling:
   - The Command pattern decouples the Sender and Receiver via a seperate Command object. This allows the Receiver to be unaware of the Sender. This contributes to the Interface Segregation principle. The Receiver is only dependent on the information it requires from the Sender. Adherence to the ISP contributes in this case to less recompilation and stable test code of the Receiver. Additionaly, the reusability of the Receiver 'grows', because it has less dependencies making it more Single Responsible. The gain in the Receiver, could result in a shift of the ISP and SRP violation to the Command object. This can be a design trade off. 
   - The Observer pattern decouples the Sender and Receiver via an abstraction. The Receiver is aware of the Sender, in case the Sender injects itself as an argument. In this way the Receiver can request the required information from the Sender. The Sender becomes less reusable in case the Sender doesn't send itself, but the required information directly. This is because other Receivers might require different information from the Sender. The Interface Segregation principle is violated in case the Receiver depends on functionality from the Sender it does not use. This negativily impacts the test code stability and recompilation of the Receiver. Additionaly, there is less adherence to the Single Responsibility principle, that negativily impacts the Receivers reusability. 

**The Command pattern should be prefered over the Observer pattern when:**
- The message from the Sender toward the Receiver requires queuing, undoing or redoing.
- A Sender will have a single Receiver, which can be defined during compile time.
- Sender and receiver decoupling has high importance.

**The Observer pattern should be prefered over the Command pattern when:**
- A Sender will have an open end of Receivers, which will vary during runtime.

The patterns can of course be combined to gain the applicability of both 'worlds'. A Sender, from the Observer pattern, could use a Command to notify its Receivers.

## Observer and Mediator

The Observer and Mediator both prescribe decoupling Sender from Receiver to gain reusability and stability. Their differences:
1. Participant relation:
   - The Mediator pattern requires change when a new Receiver is introduced that needs to be coupled to an existing Sender. The intermediate object (= mediator) would require change to support interaction between the new Receiver and the existing Sender. The relation between Sender and Receiver is determined during compile time, which means it should be statically defined.
   - The Observer pattern does not require change when a new Receiver is introduced that needs to be coupled to an existing Sender. The Observer pattern allows an open-end of Receivers to be coupled to an existing Sender. This may vary during runtime, which means it can be dynamically defined.
2. Messaging mechanism:
   - The Mediator pattern uses 'indirect' communication between Sender and Receiver. The communication between Sender and Receiver is centralized into a seperate object, called the Mediator. This means the Sender notifies an intermediate object (= Mediator) and the intermediate object notifies the Receiver.  
   - The Observer pattern uses 'direct' communication between Sender and Receiver. The Observer pattern distributes Sender and Receiver interaction. The Receiver hooks up with the Sender and the Sender notifies the Receiver when it has changed.
3. Sender-receiver decoupling:
   - The Mediator pattern decouples Sender and Receiver via an intermediate object. This enhances stability and reusability of the Senders and Receivers. The intermediate object can be replaced by a new interaction context, without modification or duplication of Senders and Receivers. The 'gain' in Senders and Receivers is a 'loss' in the intermediate object. Intermediate objects are hard to reuse and can be unstable, because of the amount of dependencies they require. The reusability and stability of the interaction logic is degraded.
   - The Observer pattern decouples Sender and Receiver via an abstraction. As stated in the comparison between 'Command and Observer': > "The Receiver is aware of the Sender, in case the Sender injects itself as an argument". This makes the Receiver less stable and reusable. The changes of the Sender reflect on the Receiver and the Receiver becomes less reusable for other Senders. However, the reusability and stability of interaction logic between Sender and Receiver is increased.

**The Observer pattern should be prefered over the Mediator pattern when:**
- The amount of new Senders and Receivers are unstable and can vary.
- The hook-up of Senders and Receivers needs to vary during runtime.
- The reusability and stability of interaction logic is more important than the reusability and stability of stand-alone Sender and Receiver logic.
- There is 'Simple' interaction logic between Senders and Receivers.

**The Mediator pattern should be prefered over the Observer pattern when:**
- The amount of new Senders and Receivers are stable and don't vary often.
- The hook-up of Senders and Receivers can be predefined during compile time.
- The reusability and stability of stand-alone Sender and Receiver logic is more important than the reusability and stability of interaction logic.
- There is 'Complex' interaction logic between Senders and Receivers. Centralizing this logic can simplify understanding.

The patterns can of course be combined to gain the applicability of both 'worlds'. The intermediate object (=Mediator), of the Mediator pattern, can use the Observer pattern to hook-up Receivers with Senders.

## Command and Mediator

The Command and Mediator both prescribe decoupling Sender from Receiver to gain reusability and stability. Both patterns use an 'intermediate' from to transfer message from Sender to Receiver. The Command pattern uses a Command instance. The Mediator pattern uses a Mediator instance. Their differences:
1. Participant interaction:
   - The Mediator pattern uses an intermediate object (=Mediator) to facilitate bidirectional messaging between multiple objects. This means object 1 can send a message to object 2 via the same Mediator and visa versa.
   - The Command pattern uses an intermediate object (=Command) to faciliate unidirectional messaging between two objects. This means object 1 can send a message to object 2 via a Command object, but not visa versa. A new Command class would be needed to send a message from object 2 to object 1.
2. Applicability:
   - The Command pattern complies to the Single Responsibility principle. Each command contains a single transaction, which has positive affect on the testability, readability and stability of the code. Testability and readability, because the problem window is small. Stability, because the impact of changing a Command is relativily small. The Single Responsibility of a command, realized by a single transaction, contributes to the possibility to queue, undo and redo the transaction easily.
   - The Mediator pattern complies to the Single Responsibility principle. A Mediator contains multiple transactions in a centralized place, that simplifies understanding and extendibility. Understanding, because complex interaction logic between multiple objects can be seen at a glance. extendibility, because interaction logic can be changed in a single place, by inherting from the Mediator base.

**The Command pattern should be prefered over the Mediator pattern when:**
- Queuing, undoing and redoing of transactions is a requirement

**The Mediator pattern should be prefered over the Command pattern when:**
- The transaction logic of multiple objects should be centralized to gain extendibility and understandability

It might be possible to queue or undo/redo interactions using the Mediator pattern, but this would require us to externally keep track of 'Colleagues' & Momentos and use the Mediator to perform these actions. The Command pattern enables us to only keep track of Command objects externally and perform these actions on them directly. As stated before the Command's flexibility and ease of use comes from its adherence to Single Responsibility principle.

## Chain of Responsibility and Observer
The Chain of Resonsibility and the Observer both decouple sender and receiver. This enables runtime flexibility to hook-up an open-end of senders and receivers. Their differences:
1. Participant interaction:
   - The Observer pattern allows an open-end of Receivers to hook-up with the same Sender (during runtime).
   - The Chain of Responsbility allows a single Receiver to hook-up with a single Sender sequently (during runtime). This means, in the middle of the chain Senders are also Receivers and Receivers are also Senders. 
