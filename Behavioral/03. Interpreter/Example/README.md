# Interpreter example

## Context
The Interpreter pattern is not often applied by developers, but a lot of fundamental software concepts are build on top of this pattern. For example Regular expressions, mathmatical syntax, query languages and static code analysis. Understanding this pattern provides insight in how problems in these concepts can be resolved by applying the Interpreter pattern. The Interpreter pattern enables expressing a problem in a relative 'simple' language. This 'simple' language is evaluated by an interpreter. Evaluation results in an action or a different representation. An example is required to express 'why' and 'how' this pattern can be used to resolve problems in concepts as query languages and Regular expressions. In the upcoming paragraph the Interpreter pattern will be explained by building a simple Regular Expression Matcher. In this example I will eloborate both on the 'why' and 'how' of the pattern.

## Simple Regular Expression Matcher

The goal of the example is to build a regular expression matcher that can interpret a Asterisks expression and string expression. The definition of the Asterisk symbol is: 'match zero or more of the preceding character'. Let's see some use cases to clarify the requirements of the Regular Expression Matcher:

Scenario 1:
- Given: regular expression: 'a*'
- When: string literal: 'aa' is evaluated
- Then: the Regular Expression Engine should return: TRUE

Scenario 2:
- Given: regular expression: 'a*'
- When: string literal: 'ab' is evaluated
- Then: the Regular Expression Matcher should return: FALSE

Scenario 3:
- Given: regular expression: 'a*b'
- When: string literal: 'ab' is evaluated
- Then: the Regular Expression Matcher should return: TRUE

Scenario 4:
- Given: regular expression: 'a*b'
- When: string literal: 'abb' is evaluated
- Then: the Regular Expression Matcher should return: FALSE

Scenario 5:
- Given: regular expression: 'a*b\*'
- When: string literal: 'abbb' is evaluated
- Then: the Regular Expression Matcher should return: TRUE

Let's draw an UML class diagram to express how the problem can be resolved using the Interpreter Pattern:

<img src=RegularExpressionEngineUML.png width=40% height=50%>

This can be reflected on the 'templated' class diagram of the Interpreter Pattern:

<img src=InterpreterUML.png width=40% height=50%>

The Asterisk expression is a Non Terminal Expression. A non Terminal Expression 'connects' both terminal and non terminal expressions. As the name indicates it cannot 'terminate' expression evaluation. A Literal expression is a Terminal Expression. A Terminal Expression defines the base of a language. As the name indicates it can 'terminate' expression interpretation. The context will contain the string-under-interpretation. This context will be interpreted based on the defined regular expression. The regular expression will be composed of AsteriskExpression object(s) and LiteralExpression object(s). The pattern prescribes that each symbol in a grammar should have its own class for reusability, ease of change and testability.

Let's define the context:

<img src=Context.png width=20% height=20%>

The Context will consist of an input string and the character that is currently under interpretation. The Context will be used by the grammar.

Let's create the LiteralExpression:

<img src=Expression.png width=30% height=30%>
<img src=LiteralExpression.png width=50% height=50%>

The LiteralExpression will store a token of type char. The interpret function evaluates the context and removes the first char of the string-under-interpretation in case the char-under-match is equal to the token. After removal the interpretation will return true if there is a match between context and token. E.g.: input "a" will match 'true' against LiteralExpression 'a'.

Now that the LiteralExpression is defined, let's compose the AsteriskExpression:

<img src=AsteriskExpression.png width=50% height=50%>

The AsteriskExpression will 'consume' two expressions. One expression before (=left leaf) the Asterisk and one expression after(=right leaf) the Asterisk symbol. E.g. reglar expression: a*b, has an 'a' before the Asterisk and a 'b' after the Asterisk. The interpret function will evaluate the context until all symbols are evaluated. The first character of the input is interpreted during each evaulation step. In case the expression before the Asterisk does not evaluate to 'true' then the expression after the Asterisk symbol is evaluated. If the expression after the Asterisk symbol is interpreted to 'true', and the context input is completly evaluated, then there is a match. The check if the input size is 0 means that the complete input is evaluated and overcomes that the combination of regular expression: "a\*b" and string input "aabb" is interpreted as 'true'.

Now that we have all the 'assests' of our grammar we can start verifying our Regular Expression Matcher. Let's start with scenario 3:
- Given: regular expression: 'a*b'
- When: string literal: 'ab' is evaluated
- Then: the Regular Expression Matcher should return: TRUE

Construction of the regular expression will not be done by a Parser. A Parser enables composing regular expressions dynamically. In this example the regular expressions will be statically defined. Let's define scenario 3 in a test:

<img src=RegularExpression_A_ASTERISK_B.png width=50% height=50%>

The regular expression 'a*b' is constructed using two LiteralExpression objects and an AsteriskExpression object. The AsteriskExpression is a composite. The context is defined as 'aab'. Interpreting this context evaluates evaluates to 'true'. Verification:

<img src=RegularExpression_A_ASTERISK_B_TestResult.png width=50% height=50%>

The Regular Expression Engine works! 

Let's see a more 'advanced' scenario, scenario 5: 
- Given: regular expression: 'a*b\*'
- When: string literal: 'abbb' is evaluated
- Then: the Regular Expression Engine should return: TRUE

Scenario 5 in a test case:

<img src=RegularExpression_A_ASTERISK_B_ASTERISK.png width=50% height=50%>

Regular expression 'a*b\*' is expressed using two LiteralExpressions and two AsteriskExpressions. This structure can be defined in an abstract syntax tree (=AST):

<img src=AST.png width=40% height=40%>

Interpretation of this AST based on the context "aabb" will result in 'true'. Verification:

<img src=RegularExpression_A_ASTERISK_B_ASTERISK_TestResult.png width=30% height=30%>

The simple Regular Expression Matcher has been developed and verified! The Regular Expression matcher is capable of interpreting a string literal, based on a Regular expression, into a boolean form. 

Now that we understand the 'how' of the Interpreter pattern its time to elobrate on the 'why'. 

*Why would we use the Interpreter pattern for resolving 'simple' language interpretation?*

We use the Interpreter pattern to express a problem in a user-friendly and relative simple language. For example a programming language. A developer can read and write a programming language after knowing its Grammar rules. A programming language enables us to express a problem in a relative simple language. We don't have to know assembly or binary code. While assembly is still human readable, it is harder and more time consuming for developers to resolve problems in such a language. Binary code is even worse, for a developer it is almost impossible to resolve problems using only '0' or '1'. While this translation process is normally done by Compiler, which works a bit differently and is more sophisticated, the fundamentals of the Interpreter pattern still apply.

The Interpreter pattern compliments the Single Responsibility and Open Closed Principle by defining each Grammar rule behind an abstraction and in a separate class. This 'opens' serveral advantages namely: **maintainability**, **reusability**, **stability** and **testability**.

 **Maintainability**, because Grammar rules are seperated from the Parser. In case something is wrong in the final AST, the problem lies in the Parser. If something is wrong in the interpretation, the problem lies in the Grammar. Even within the Grammar we are able to quickly indicate in which rule the problem lies. The Interpreter pattern 'shrinks' the problem window, which enables easy maintainable code. 

**Reusability**, because multiple different parsers can re-use the same grammar rules and new rules can derive from 'old' rules.

 **Stability**, the Interpreter pattern compliments the Open Closed principle. The Open Closed Principle is 'supported' by dependency inversion principle. Each Grammar rule is dependent on an abstraction and not on the concrete implementation (=details). This enables grammar rules to change behavior without modification, but also flexibility to make grammar rule combinations (=reusability). E.g. the Asterisk Expression can work with both a Literal expression and an Asterisk Expression without changing the Asterisk Expression. 

**Testability**, because each grammar rule specifies its 'own' class the problem window is shrinked. This results in finding bugs quicker and easier during unit tests. In contrast, writing every grammar rule in the same class, would make it harder to find where the issue relies.




