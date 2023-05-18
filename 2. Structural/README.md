# Structural Design Patterns

The Structural Design Patterns emphasize usage of Composition over inheritance to enable reusability and extendability. I would like to summarize the argumentation of this preference. The details of this argumentation are embedded within the Structural Design Patterns. 

Composition is favored over inheritance because:
- code navigability might enhance. Navigating through an inheritance tree can be difficult, especially when 'deep rooted' (inheritance-on-inheritance). Composition can simplify code navigation.
- testing in isolation might enhance. Inheritance 'shapes' a tight coupling between classes, which results in testing the inherited classes simultansiouly with the class under test. Composition might enhance isolated testing, because the relationship is 'weaker' as opposed to inheritance.
- flexibility might enhance. Inheritance is applied on the class level, which means it is statically defined during compile time. Composition is applied on the object level, which means it can be used to dynamically enhance or reuse particular instances (and not the entire class e.g. Decorator pattern). 
- reusability might enhance. Inheritance binds a particular implementation to a specific inheritance tree. Re-using this implementation can result in the client being dependent on the full inheritance tree, while it only requires the particular implementation. Composition can prevent that, ensuring reusability 'ease'.
- readability might enhance. Inheritance can cause a class explosion when two (or more) dimensions are intertwinned. This negativily influences the readability of the code. Composition can avoid this class explosion by seperating the inheritance hierarchy into two independent hierarchies (=Bridge Pattern) or to introduce a seperate branch within the same inheritance hierarchy (=Decorator).
- stability might enhance. Inheritance tightly couples the base and concrete classes. In case the base class changes it might reflect fatally in the (deeper) layers of the inheritance tree. This change might be hard to oversee in an extensive inheritance hierarchy. Composition might simplify change, by overseeing the impact, resulting in a higher level of stability.

As can be conducted from above argumentation, composition is often favored over inheritance. However, there are cases that inheritance is favored over composition. It is worth mentioning them to gain a comprehensive understanding. 

Inheritance is favored over composition because:
- stability might enhance. Composition does not enable subtyping, which would result in the complete system to dependent on concrete types. This would require modification in both product and test code when change is required. Inheritance enables subtyping, which allows extension of the system with minimal modification.
- flexibility might enhance. Composition does not enable subtyping, which would disallow the runtime flexibility to treat different concrete objects uniformly. Inheritance enables subtyping, which allows this flexibility.
- testability migth enhance. Composition does not allow subtyping, which would disallow mocking possibility. Inheritance enables subtyping, which allows mockability. This enhances testing in isolation.
- understandability might enhance. Composition might imply wrong intent, because it is a 'has-a' relationship. Inheritance is an 'is-a' relationship, which might be more descriptive in some cases to clarify code intent.