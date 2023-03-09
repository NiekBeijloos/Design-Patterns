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
3. Interface dehancement:
   - The Decorator pattern allows removal of object's functionality. An existing object can be wrapped by a Decorator that replaces the original object's behavior with an empty call.
   - The Visitor pattern unable to remove existing behavior on an object structure.

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