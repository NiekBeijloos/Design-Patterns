# Comparison Creational, Structural and Behavioral patterns

The Creational, Structural and Behavioral patterns can have similar aspects. It can be hard to distinguish patterns across those categories and know when to apply one over the other. Even though these patterns belong to different design pattern categories, they can still resolve identical problems. In the following paragraphs I will cross compare patterns within these categories to address their differences.

## Decorator and Visitor

The Decorator pattern is a Structural pattern and the Visitor pattern is a Behavioral pattern. Both patterns can be used to 'enhance' an existing interface while preserving it (no modification). This means both patterns adhere to the Open Closed Principle. Their differences can be identified in their details:
1. Interface enhancement: 
    - The Decorator pattern is 'constraint' to only enhance an object based on its current interface. This means the Decorator's functionality must be inline with the existing object's interface.
    - The Visitor pattern is able to enhance an object unrelated to the object's current interface. This means the enhancement is not inline with the object's existing interface.
2. Level of enhancement:
    - The Decorator pattern allows to enhance the interface on the object level. I.e. the Decorator can apply interface enhancement(s) to specific objects of a class.
    - The Visitor pattern provides enhancement of the interface on the class level. I.e. specific (concrete) classes can have different extensions (Visitors), however the objects of those classes will all have identical behavior for each Visitor.

## Prototype and Momento

The Prototype pattern is a Creational pattern and the Momento is a behavioral pattern. Both patterns can be used to copy the internals of an object. Their differences are:

1. Intent:
   - The Prototype pattern enables copying an object without knowing its underlying concrete type. 
   - The Momento pattern enables a state snapshot of a given object to revert to it later.
2. Interface level:
   - Using the Prototype pattern, the copy of an object contains the same interface as the original object.
   - Using the Momento Pattern, the copy of an object contains a narrower interface than the original object.
3. Copying behavior:
   - The Prototype pattern prescribes making a full internal copy of an existing object.
   - The Momento pattern enables making a full or partial copy of an existing object.

## Decorator and Chain Of Responsibility

The Decorator and Chain Of Responsibility can be both used to execute a sequence of operations. Their difference:
1. Intent:
   - The Chain Of Responsibility is intended to handle a request and if it can't handle it pass it on to the next handler in the chain. In case the Handler is able to handle the request the sequence (=Chain) can be interrupted.
   - The Decorator is intended to add additional behavior on top of existing behavior. Interrupting the sequence of calls would violate the intend of the Design pattern, because (existing) behavior would be discarded.

## Bridge and Strategy

The Bridge design pattern is a structural design pattern and the Strategy pattern is a behavioral pattern. The patterns can be both used to dynamically switch the behavior of a given object. Their differences:
1. Intent:
   - The Bridge design pattern is used to seperate Abstraction from Implementation so both can evolve independenly.
   - The Strategy design pattern is used to dynamically switch behavior of a given object. 
2. Interface level:
   - The Bridge design pattern prescribes the use of two abstractions. One abstraction inherited by the 'Implementor' (=Strategy in Strategy pattern) and one inherited by 'Abstraction' (=Context in Strategy pattern). This allows client flexibility for both 'Abstraction' and 'Implementor'.
   - The Strategy design pattern prescribes the use of a single abstraction. The abstraction is inherited by the Strategy (=Implementor in Bridge design pattern). This allows flexibility of change for solely the Strategy. The Context (=Abstraction in Bridge design pattern) will be hard wired to the client, resulting in less flexibility.

Unfortunately, the argumentation is not as black and with as it seems. The Bridge pattern is more expressed in the relationship between classes (composition or aggregation). I.e. the Bridge pattern is often not literally mentioned in the design, but more in communication. The Strategy pattern is more expressed on the class level. I.e. the Strategy pattern is often expressed in the design (e.g. class names). The argumentation, defined in '2. interface level', is correct in terms of the UML structure of the design pattern. However, I wouldn't explicity mention that the design is using only Bridge or Strategy based on this argument. I think both can be applicable simultaneously, only in design I would express Strategy explicitly and Bridge only in communication.

The Bridge pattern can be used to dynamically switch behavior of a given object just like the Strategy pattern. However, if would need to emphasize this runtime flexibility I would prefer to use the Strategy pattern. This is because the intent of the Strategy pattern is more focussed on this aspect.

## Facade and Mediator

The Facade pattern is a structural design pattern and the Mediator pattern is a behavioral pattern. Both patterns can be used to encapsulate and orchistrate objects. Their differences:
1. Intent:
   - The Facade pattern enables a simplified interface **to** an (existing) subystem
   - The Mediator centralizes complex object interaction **within** a subsystem
2. Object interaction:
   - The Facade pattern emphasizes unidirectional object interaction. I.e. the Facade 'calls' the the subsystem classes, but the subsystem classes don't call the Facade.
   - The Mediator pattern emphasizes bidirectional object interaction. I.e. the Mediator 'calls' the Colleagues and the Colleagues call the Mediator.
3. Functional enhancement:
   - The Facade pattern is clustering object calls, but does not enhance the functionality of a system.
   - The Mediator pattern enhances the functionality of a system as a result of object interaction.