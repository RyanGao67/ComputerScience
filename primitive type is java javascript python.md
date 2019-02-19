This blog will introduce some basic concepts in Java, Js, Python regarding primitive types. As we know, most modern programming languages applied "pass by value" approach when interacting with functions. However, the underlying meaning of values here are different with regard to the type of values. In most cases, the value refers to the actual value stored in the variable if the variable is of a primitive type and refers to the memory location of the actual value if the variable is of a reference type. 
* When a parameter is passed by reference, the caller and the callee use the same variable for the parameter. If the callee modifies the parameter variable, the effect is visible to the caller's variable.
* When a parameter is passed by value, the caller and callee have two independent variables with the same value. If the callee modifies the parameter variable, the effect is not visible to the caller.

##### In Java
There are eight primitive data types: 
1. boolean
2. char:16-bit
3. byte:8-bit
4. short:16-bit
5. int:32-bit
6. long:     64-bit
7. float:    32-bit
8. double:   64-bit

##### In Javascript
Boolean, Null, Undefined, Number, String, Symbol

##### In python
Premitive type is more often refered to as built-in immutable type. 
bool, int, float, tuple, str, frozenset.(basically string, number, tuple)
```
x = 'hi'
y = 'hi'
id(x)==id(y)   # this returns true
```