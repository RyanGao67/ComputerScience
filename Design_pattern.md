### SRP 
Single responsible principle
* The class should have a single reason to change
* no god object

### OCP (open close principle)+spcification
* It is okay for you to implement interface or extentds things 
* It is close for modification

### LSP(liskov substitution principle)
* You should be able to substitute the subclass for a baseclass

### ISP(Interface segregation principle)
* How to seperate a interface into smaller interfaces
* You shouldn't put into your interface more than the client is expected to implement 

### DIP (Dependency inversion principle)
* Abstraction: abstract class or interface(we get a signature which perform a operation, instead of working with concrete implementation, you work with abstraction )
* A. High-level modules should not depend on low-level modules(both should depend on abstractions)
* B. Abstractions should not depend on details. (Details should depend on abstractions) 

### Gamma categorization
* Creational patterns
* Structural patterns
* Behavioral Patterns

### Builder pattern
* when piecewise object construction is complicated, provide an API for doing it succinctly
```java
package designPattern;

public class RecursiveGenericBuilder {
	public static void main(String[] args) {
		EmployeeBuilder employeeBuilder = new EmployeeBuilder()
				.withName("Dmitri")
				.workAs("Quantitative analyst");
		System.out.println(employeeBuilder.build());
	}
}

class People{
	String name;
	String position;
	@Override
	public String toString() {
		return "Person{"+"name='"+name+'\''+", position='"+position+'\''+'}';
	}
}

class PeopleBuilder<SELF extends PeopleBuilder<SELF>>{
	protected People people = new People();
	public SELF withName(String name) {
		people.name = name;
		return (SELF)this;
	}
	public People build() {
		return people;
	}
}

class EmployeeBuilder extends PeopleBuilder<EmployeeBuilder>{
	public EmployeeBuilder workAs(String position) {
		people.position = position;
		return this;
	}
	
}

```

### Factory 
* Motivation
  * Object creation logic becomes too convoluted
  * Constructor is not descriptive
    * Name mandated by name of containing type
    * Cannot overload with same sets of arguments with different names
    * can turn into overloading hell
  * Wholesale object creation(non-piecewise, unlike builder) can be outsourced to
    * A separate functin(factory method)
    * That may exist in a separate class(Factory)
    * Can create hierarchy of factories with abstract factory
    
  * A factory method is a static method that creates objects
  * a factory can take care of object creation
  * A factory can be external or reside inside the object as an inner class
  * Hierarchies of factories can be used to create related objects
 ```java
 package designPattern;
class Point{
	private double x, y;
	private Point(double x, double y) {	this.x = x;	this.y = y;	}
	public static class Factory{
		public static Point newCartesianPoint(double x, double y) {
			return new Point(x, y);
		}
		public static Point newPolarPoint(double rho, double theta) {
			return new Point(rho*Math.cos(theta), rho*Math.sin(theta));
		}
	}
	@Override
	public String toString() {return "x="+x+";y="+y;}
}
class FactoryPattern{
  public static void main(String[] args){
    Point point1 = Point.Factory.newCartesianPoint(1, 2);
    Point point2 = Point.Factory.newPolarPoint(10, 3.14);
    System.out.println(point1);
    System.out.println(point2);
  }
}
 
 
 ```
