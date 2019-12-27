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
 
 
 ### Singleton
 * For some somponents it only makes sense to have one in the system
   * Database repository
   * Object factory
 * E.g. the contructor call is expensive
   * we only do it once
   * we provide everyone with the same instance
 * Want to prevent anyone creating additional copies
 * Need to take care of lazy instantiation and thread safety
 
 * A singleton is a component which is instantiated only once. 

**prevent serializable**
 ```java
 package designPattern;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

class BasicSingleton implements Serializable{
	private BasicSingleton() {}
	private static final BasicSingleton INSTANCE = new BasicSingleton();
	public static BasicSingleton getInstance() {return INSTANCE;}
	private int value = 0;
	public int getValue() {return value;}
	public void setValue(int value) {this.value = value;}
	// solution for serialization
	protected Object readResolve() {return INSTANCE;}
}
public class SingletonSerialize {
	static void saveToFile(BasicSingleton singleton, String filename) throws Exception{
		try(FileOutputStream fileOut = new FileOutputStream(filename);
			ObjectOutputStream out = new ObjectOutputStream(fileOut)){
			out.writeObject(singleton);
		}
	}
	static BasicSingleton readFromFile(String filename) throws Exception{
		try(FileInputStream fileIn = new FileInputStream(filename);
			ObjectInputStream in = new ObjectInputStream(fileIn)){
			return (BasicSingleton) in.readObject();
		}
	}
	public static void main(String[] args) throws Exception {
		// 1. reflection
		// 2. serialization (when you deserialize an object, JVM doesn't care your constructor is private)
		BasicSingleton singleton = BasicSingleton.getInstance();
		singleton.setValue(111);
		String filename = "singleton.bin";
		saveToFile(singleton,filename);
		singleton.setValue(222);
		BasicSingleton singleton2 = readFromFile(filename);
		
		System.out.println(singleton==singleton2);
		
	}
}
 ```
**thread safe**
```java
package designPattern;

public class SingletonLazy {
	private static SingletonLazy instance;
	private SingletonLazy() {
		System.out.println("initializing a lazy singleton");
	}
	// race condition
//	public static SingletonLazy getInstance() {
//		if {instance=new SingletonLazy();}
//		return instance;
//	}
	// double-checked locking
	public static SingletonLazy getInstance() {
		if(instance==null) {
			synchronized (SingletonLazy.class) {
				if(instance==null) {
					instance = new SingletonLazy();
				}
			}
		}
		return instance;
	}
}

```
