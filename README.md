# Java-Questions

How to prevent cloning of Singleton object: We can throw an exception from the clone method of the Singleton class.

=======
Is Java pass by reference or pass by value: Java is always pass by value, with no exceptions, ever. So how is it that anyone can be at all confused by this, and believe that Java is pass by reference, or think they have an example of Java acting as pass by reference? The key point is that Java never provides direct access to the values of objects themselves, in any circumstances. The only access to objects is through a reference to that object. Because Java objects are always accessed through a reference, rather than directly, it is common to talk about fields and variables and method arguments as being objects, when pedantically they are only references to objects. The confusion stems from this (strictly speaking, incorrect) change in nomenclature. So, when calling a method. For primitive arguments (int, long, etc.), the pass by value is the actual value of the primitive. For objects, the pass by value is the value of the reference to the object.

=======
Java Reflection provides ability to inspect and modify the runtime behavior of application. Reflection in Java is one of the advance topic of core java. Using java reflection we can inspect a class, interface, enum, get their structure, methods and fields information at runtime even though class is not accessible at compile time. We can also use reflection to instantiate an object, invoke it’s methods, change field values. Using reflection, your code can look at the object and find out if it has a method called 'doSomething' and then call it if you want to.
- Reflection in Java is a very powerful concept and it’s of little use in normal programming but it’s the backbone for most of the Java, J2EE frameworks. Some of the frameworks that use java reflection are:
- JUnit – uses reflection to parse @Test annotation to get the test methods and then invoke it.
- Spring – dependency injection, read more at Spring Dependency Injection
- Tomcat web container to forward the request to correct module by parsing their web.xml files and request URI.
- Eclipse auto completion of method names
- Struts
- Hibernate

=======
Why reflection is not preferred?
- Poor Performance – Since java reflection resolve the types dynamically, it involves processing like scanning the classpath to find the class to load, causing slow performance.
- Security Restrictions – Reflection requires runtime permissions that might not be available for system running under security manager. This can cause you application to fail at runtime because of security manager.
- Security Issues – Using reflection we can access part of code that we are not supposed to access, for example we can access private fields of a class and change it’s value. This can be a serious security threat and cause your application to behave abnormally.
- High Maintenance – Reflection code is hard to understand and debug, also any issues with the code can’t be found at compile time because the classes might not be available, making it less flexible and hard to maintain.

=======
How do I call one constructor from another in Java: We use this. To chain to a particular superclass constructor instead of one in the same class, use super instead of this. Note that you can only chain to one constructor, and it has to be the first statement in your constructor body.

=======
Breaking out of nested loops in Java
public class Test {
    public static void main(String[] args) {
        outerloop:
            for (int i=0; i < 5; i++) {
                for (int j=0; j < 5; j++) {
                    if (i * j > 6) {
                        System.out.println("Breaking");
                        break outerloop;
                    }
                System.out.println(i + " " + j);
            }
        }
        System.out.println("Done");
    }
}

=======
What do the expression 1.0 / 0.0 will return? will it throw Exception? any compile time error: It will not throw ArithmeticExcpetion and return Double.INFINITY. Also, note that the comparison x == Double.NaN(Not a number) always evaluates to false, even if x itself is a NaN. To test if x is a NaN, one should use the method call Double.isNaN(x) to check if given number is NaN or not.

=======
Difference between StringBuilder and StringBuffer: StringBuffer methods e.g. length(), capacity() or append() are synchronized while corresponding methods in StringBuilder are not synchronized. StringBuilder is faster because of synchronized methods.

=======
Does java support multiple inheritance: No. Diamond problem.

=======
How would you solve Deadlock: Would look the code if I see nested synchronized block or calling one synchronized method from other or trying to get lock on different object.
public class DeadLockDemo { /* * This method request two locks, first String and then Integer */ 
	public void method1() { 
		synchronized (String.class) { 
			System.out.println("Acquired lock on String.class object"); 
			synchronized (Integer.class) {
				System.out.println("Acquired lock on Integer.class object"); 
			} 
		} 
	} 
/* * This method also requests same two lock but in exactly * Opposite order i.e. first Integer and then String. * This creates potential deadlock, if one thread holds String lock * and other holds Integer lock and they wait for each other, forever. */ 
	public void method2() { 
		synchronized (Integer.class) { 
			System.out.println("Acquired lock on Integer.class object"); 
				synchronized (String.class) { 
					System.out.println("Acquired lock on String.class object"); 
				} 
			} 
		}
	}
}
How to solve this : Just make the method to have the synchronization in one way.

=======
What is immutable object: Immutable classes are Java classes whose objects can not be modified once created. Any modification in Immutable object result in new object. For example is String is immutable in Java. 

=======
Can you create an immutable object that contains a mutable object?
State of immutable object can not be modified after construction, any modification should result in new immutable object. All fields of Immutable class should be final so that values can be assigned only once. Object must be properly constructed i.e. object reference must not leak during construction process. Object should be final in order to restrict sub-class for altering immutability of parent class. Class should be final so that it should not be extended. Do not provide setter methods for variables. Perform cloning of objects in getter method to return a copy rather than returning the actual object reference.
public final class Contacts {
    private final String name;
    private final String mobile;
    private final HashMap<String,String> testMap;
    public Contacts(String name, String mobile) {
        this.name = name;
        this.mobile = mobile;
    }
    public String getName(){
        return name;
    }
    public String getMobile(){
        return mobile;
    }
    //Accessor function for mutable objects
    public HashMap<String, String> getTestMap() {
        //return testMap;
        return (HashMap<String, String>) testMap.clone();
    }
}
This Java class is immutable, because its state can not be changed once created. You can see that all of it’s fields are final. This is one of the most simple way of creating immutable class in Java, where all fields of class also remains immutable like String in above case. Some time you may need to write immutable class which includes mutable classes like java.util.Date, despite storing Date into final field it can be modified internally, if internal date is returned to the client. In order to preserve immutability in such cases, its advised to return copy of original object

=======
What is the difference between creating String as new() and literal: When we create string with new() Operator, it’s created in heap and not added into string pool while String created using literal are created in String pool. String.intern() is used to put new() into String pool explicitly.

=======
What happens if your Serializable class contains a member which is not serializable? How do you fix it: Any attempt to Serialize that class will fail with NotSerializableException, but this can be easily solved by making that variable transient for static in Java.

class DerivedFactors {             // Class that contains derived values computed from entries in a
    private Number efficiency;     // performance record
    private Number costPerItem;
    private Number profitPerItem;
    ...
}

class WrongPerformanceRecord implements Serializable {
    private String unitId;
    private Number dailyThroughput;
    private Number dailyCost;
    private DerivedFactors factors;  // BAD: 'DerivedFactors' is not serializable
                                     // but is in a serializable class. This
                                     // causes a 'java.io.NotSerializableException'
                                     // when 'WrongPerformanceRecord' is serialized.

class PerformanceRecord implements Serializable {
    private String unitId;
    private Number dailyThroughput;
    private Number dailyCost;
    transient private DerivedFactors factors;  // GOOD: 'DerivedFactors' is declared
                                               // 'transient' so it does not contribute to the
                                               // serializable state of 'PerformanceRecord'.
    ...
}

=======
Why character array is better than String for Storing password: Since Strings are immutable in Java if you store password as plain text it will be available in memory until Garbage collector clears it and since String are used in String pool for re-usability there is pretty high chance that it will be remain in memory for long duration, which pose a security threat. Storing password in character array clearly mitigates security risk of stealing password. With String there is always a risk of printing plain text in log file or console but if use Array you won't print contents of array instead its memory location get printed.

=======
Why is 0.1 * 3 != 0.3: There reason is that only numbers that are powers of 2 can be represented precisely with a simple binary representation. The individual digits of a binary number are different powers of 2 and any number is thus a sum of different powers of 2. 0.1 does not correspond to a power of 2 and can not be summed up from different powers of 2. Thus it can be represented only with a certain error.

=======
What is the right data type to represent currency: BigDecimal. It helps in ability to specify a scale, which represents the number of digits after the decimal place, and Ability to specify a rounding method

=======
Can we use multiple main methods: Yes. While starting the application, we mention the class name to be run. The JVM will look for the main method only in the class whose name you have mentioned.

=======
Is an empty .java file name a valid source file name: Yes. You save your java file by .java only, compile it by javac .java and run by java your class name.

=======
Does overriding the hashCode() method have any performance implication: A poor hash code function will result in the frequent collision in HashMap which eventually increases the time for adding an object into Hash Map.

=======
Do all properties of an Immutable Object need to be final: Not necessarily, as stated above you can achieve same functionality by making the member non-final but private and not modifying them except in a constructor. Don’t provide setter methods for them and if it is a mutable object, then don’t ever leak any reference for that member. Remember making a reference variable final, only ensures that it will not be reassigned to a different value, but you can still change individual properties of object, pointed by that reference variable.

=======
Why String class is immutable: String pool requires string to be immutable otherwise shared reference can be changed from anywhere. Security because string is shared on different area like file system, networking connection, database connection, having immutable string allows you to be secure and safe because no one can change reference of string once it gets created. If string had been mutable anyone can surpass the security be logging in someone else name and then later modifying file belongs to other.

=======
In Java, can we store a double value in a long variable without explicit casting: The following 19 specific conversions on primitive types are called the widening primitive conversions:
byte to short, int, long, float, or double
short to int, long, float, or double
char to int, long, float, or double
int to long, float, or double
long to float or double
float to double
Widening primitive conversions do not lose information about the overall magnitude of a numeric value.

=======
Is the pre-increment operator thread-safe: ++ operator is not atomic. For atomic operations you can use AtomicInteger. AtomicInteger atomicInteger = new java.util.concurrent.atomic.AtomicInteger(0) and every time you want to increment you can call atomicInteger.incrementAndGet() method which returns a primitive int. 0 is the default initial value for the atomic integer.

=======
Cloning and copying: The java.lang.Object class contains a clone() method that returns a bitwise copy of the current object. protected native Object clone() throws CloneNotSupportedException. Not all objects are Cloneable. It particular only instances of classes that implement the Cloneable interface can be cloned. Trying to clone an object that does not implement the Cloneable interface throws a CloneNotSupportedException. For example, to make the Car class Cloneable, you simply declare that it implements the Cloneable interface. Since this is only a marker interface, you do not need to add any methods to the class.
public class Car extends MotorVehicle implements Cloneable {
  // ...
}
For example
Car c1 = new Car("New York A12 345", 150.0);
Car c2 = (Car) c1.clone();
Most classes in the class library do not implement Cloneable so their instances are not Cloneable. Most of the time, clones are shallow copies. In other words if the object being cloned contains a reference to another object A, then the clone contains a reference to the same object A, not to a clone of A. If this isn't the behavior you want, you can override clone() yourself. You may also override clone() if you want to make a subclass notCloneable, when one of its superClass does implement Cloneable. In this case simply use a clone() method that throws a CloneNotSupportedException. For example,
public Object clone() throws CloneNotSupportedException {
    throw new CloneNotSupportedException("Can't clone a SlowCar");
}
You may also want to override clone() to make it public instead of protected. In this case, you can simply fall back on the superclass implementation. For example,
public Object clone() throws CloneNotSupportedException {
    return super.clone();
}

=======
Is it necessary to throw CloneNotSupportedException for Singleton classes: If you're going to implement a singleton one of the "wrong" ways, no, there's absolutely no reason whatsoever to implement Cloneable and override Object#clone() just to throw CloneNotSupportedException. Object#clone() already does this when the Cloneable interface is absent. The above is only necessary if a superclass of a singleton class implements a public clone() method.

=======
Dry run: A dry run (not only in Java language) simply means to execute your code manually by going through each line, iteration, operations, functions, etc. in order to test it. In a dry run, you give your program a sample input (which is generally a small and easy number/string/etc. with respect to the purpose of your program) and check for an output whether it comes correctly or not with the proper logic implementation in your code.

=======
If you declare a method to take ArrayList<Animal> it can take only an ArrayList<Animal>, not ArrayList<Dog> or ArrayList<Cat>. Array types are checked again at runtime, but collection type checks happen only when you compile.
To solve the issue use like this
public void takeAnimals(ArrayList<? extends Animal> animals){
    for(Animal a: animals){
        a.eat();
    }
}
Here extends mean extends or implements.

==========
REFRENCES
==========
Weak References: Weak Reference objects are not the default type/class of Reference Object and they should be explicitly specified while using them. This type of reference is used in WeakHashMap to reference the entry objects. If JVM detects an object with only weak references (i.e. no strong or soft references linked to any object), this object will be marked for garbage collection. To create such references java.lang.ref.WeakReference class is used. These references are used in real time applications while establishing a DBConnection which might be cleaned up by Garbage Collector when the application using the database gets closed. If an object has no strong reference but has a weak reference then GC reclaims this object’s memory in next run even though there is enough memory. 
// Strong Reference
Gfg g = new Gfg();   
g.x();
// Creating Weak Reference to Gfg-type object to which 'g' is also pointing.
WeakReference<Gfg> weakref = new WeakReference<Gfg>(g);
//Now, Gfg-type object to which 'g' was pointing earlier is available for garbage collection.
//But, it will be garbage collected only when JVM needs memory.
g = null; 
// You can retrieve back the object which has been weakly referenced. It successfully calls the method.
g = weakref.get(); 
g.x();

=======
Soft References: Even if the object is free for garbage collection then also its not garbage collected, until JVM is in need of memory badly. The objects gets cleared from the memory when JVM runs out of memory. To create such references java.lang.ref.SoftReference class is used. If the object is not GCed, it returns the object, otherwise , it returns null.
// Creating Soft Reference to Gfg-type object to which 'g' is also pointing.
SoftReference<Gfg> softref = new SoftReference<Gfg>(g);
// You can retrieve back the object which has been weakly referenced. It successfully calls the method.
g = softref.get(); 
 
=======
Phantom References: The objects which are being referenced by phantom references are eligible for garbage collection. But, before removing them from the memory, JVM puts them in a queue called ‘reference queue’ . They are put in a reference queue after calling finalize() method on them. Phantom references can’t be accessed directly. When using a get() method it will always return null. It is used where sometimes using finalize() is not sensible. This is a special reference which says that the object was already finalized, and the garbage collector is ready to reclaim its memory. Object is useful only to know exactly when an object has been effectively removed from memory: normally they are used to fix weird finalize() revival/resurrection behavior, since they actually do not return the object itself but only help in keeping track of their memory presence.
//Creating reference queue
ReferenceQueue<Gfg> refQueue = new ReferenceQueue<Gfg>();
//Creating Phantom Reference to Gfg-type object to which 'g' is also pointing.
PhantomReference<Gfg> phantomRef = new PhantomReference<Gfg>(g,refQueue);
//Now, Gfg-type object to which 'g' was pointing earlier is available for garbage collection.
//But, this object is kept in 'refQueue' before removing it from the memory.
//It always returns null. 
g = phantomRef.get(); 
//It shows NullPointerException.
g.x();

=======
Weak Reference Objects are ideal to implement cache modules. In fact, a sort of automatic eviction can be implemented by allowing the GC to clean up memory areas whenever objects/values are no longer reachable by strong references chain. An example is the WeakHashMap retaining weak keys.

=======
Differences Between Multitasking and Multithreading in OS
- The basic difference between multitasking and multithreading is that in multitasking, the system allows executing multiple programs and tasks at the same time, whereas, in multithreading, the system executes multiple threads of the same or different processes at the same time.
- In multitasking CPU has to switch between multiple programs so that it appears that multiple programs are running simultaneously. On other hands, in multithreading CPU has to switch between multiple threads to make it appear that all threads are running simultaneously.
- Multitasking allocates separate memory and resources for each process/program whereas, in multithreading threads belonging to the same process shares the same memory and resources as that of the process.

=======
public class MethodOverloadingExample {
    public void methodTest(Object object){
        System.out.println("Calling object method");
    }
    public void methodTest(String object){
        System.out.println("Calling String method");
    }
    public static void main(String args[]){
        MethodOverloadingExample moe = new MethodOverloadingExample();
        moe.methodTest(null);
    }
}
Output: "Calling String method"
When we have two overloaded version of same method, JVM will always call most specific method.

=======
public class PumpkinDemo {
    public static void main(String[] args) {
        testMethod(1); 
    }
    public static void testMethod(long number){
        System.out.println("long");
    }
    public static void testMethod(int number){
        System.out.println("int");
    }
    public static void testMethod(Integer number){
        System.out.println("Integer");
    }
    public static void testMethod(double number){
        System.out.println("double");
    }
    public static void testMethod(Number number){
        System.out.println("Number");
    }
}
It will print int. Here is preference hierarchy. int->long->double->Integer->Number

=======
public class PumpkinDemo {
    public static void main(String[] args) {
        testMethod(1); 
    }
    public static void testMethod(byte number){
        System.out.println("byte");
    }
    public static void testMethod(short number){
        System.out.println("short");
    }
}
It will give compile time error as jvm can't downcast from int to byte by itself.

=======
public class PumpkinDemo {
    public static void main(String[] args) {
        testMethod(null); 
    }
    public static void testMethod(Long number){
        System.out.println("Long");
    }
    public static void testMethod(Integer number){
        System.out.println("Integer");
    }
}
It will give compile time error. null creates ambiguity here as Long and Integer both can be null and JVM can not decide the method to be called.

=======
public static void main(String[] args) {
    testMethod(null); 
}
public static void testMethod(Number number){
    System.out.println("Number");
}
public static void testMethod(Integer number){
    System.out.println("Integer");
}
Number is a parent class and Integer is the child class, so it will not create ambiguity. Priority is given to the more specific child class i.e. Integer in our case. Answer is "Integer".

=======
public class PumpkinDemo {
    public static void main(String[] args) {
        Shape s = new Circle();
        s.draw();
    }
}
class Shape{
    protected void draw(){
        System.out.println("Shape");
    }
}
class Circle extends Shape{
    public void draw(){
        System.out.println("Circle");
    }
}
Access Modifier must not be more restrictive. Can be less restrictive. It will print "Circle". What will happen if you change protected to private in Shape class. It will give compile time error as s is a type shape but cannot get draw method.

=======
public class PumpkinDemo { 
    public static void main(String[] args) {
        Shape s = null;
        s.draw();
    }
}
class Shape{
    public static void draw() {
        System.out.println("Shape");
    }
}
It won't give null point exception but it will print Shape as it is a static function does not need object.

=======
public class PumpkinDemo {
    public static void main(String[] args) {
        Shape s = new Circle();
        s.draw();
    }
}
class Shape{
    public static void draw()   {
        System.out.println("Shape");
    }
}
class Circle extends Shape{
    public static void draw()   {
        System.out.println("Circle");
    }
}
It will print "Shape" output. Static methods are attached to class and compiler converts reference variable to class name. What will happen if we remove static from draw function in shape. It will give compile time error "THis instance method cannot override the static method from Shape"

=======
public class PumpkinDemo {
    public static void main(String[] args) throws IOException{
        Shape s = new Circle();
        s.draw();
    }
}
class Shape{
    public void draw() throws IOException{
        System.out.println("Shape");
    }
}
class Circle extends Shape{
    public void draw() throws FileNotFoundException {
        System.out.println("Circle");
    }
}
While overriding a method, you can compress the scope of checked exception but you cannot widen it. Also you cannot throw any other checked exception which is not being thrown in the parent class method. Here FileNotFoundException is a child class of IOException. So, above code will compile successfully and it will give Circle as output. IF we change FileNotFoundException to Exception then this code will not compile and will say "Exception is not compatible throws clause in Shape.draw()"

========
public class PumpkinDemo {
    public static void main(String[] args){
        Shape s = new Circle();
        s.draw();
    }
}
class Shape{
    public void draw() throws ArithmeticException{
        System.out.println("Shape");
    }
}
class Circle extends Shape{
    public void draw() throws RuntimeException {
        System.out.println("Circle");
    }
}
This will compile. Overridden methods can throw any RuntimeException irrespective of its scope unlike checked exception.

========
public class PumpkinDemo { 
    public static void main(String[] args){
        Parent p = new Child();
        p.testMethod();
    }
}
class Parent{
    public Number testMethod(){
        System.out.println("Parent");
        return 0;
    }
}
class Child extends Parent{
    public Integer testMethod() {
        System.out.println("Child");
        return 0;
    }
}
Compiles successfully. Child output. Number is a parent of Integer wrapper class. They are called covariant return types. Covariant return type allows us to change the return type of the overriding method in the subclass, however this return type in subclass method must be subtype of the super class method return type.
There are two ways in which you can get compile time error:
1) Parent class return type: Integer, Child class return type: Number or Long
2) Parent class return type: String, Child class return type: Number or Long

=======
public class PumpkinDemo {
    public static void main(String[] args){
        Parent p = new Child();
        p.testMethod(0);
    }
}
class Parent{
    public void testMethod(Number n){
        System.out.println("Parent");
    }
}
class Child extends Parent{
    public void testMethod(Integer n){
        System.out.println("Child");
    }
}
Does java allow covariant method parameters? Is it method overriding?? Compiler will consider both the functions as different methods and it is not method overriding. It will give priority to Parent class testMethod() and prints Parent as output.

=======
To make things easier to remember, heap is used for dynamic memory allocation, while stack is for static allocations.

=======
When does the allocation happen? On the stack, memory allocation happens when the program is compiled. Meanwhile, on the heap, it begins when the program is run.

=======
TreeSet internally uses TreeMap and HashSet internally uses HashMap.

=======
Initial capacity of LinkedList is 10

=======
public class MainClass {
    public static void main(String[] args) {
        Parent p = new Child();
        System.out.println(p.getValue());
    }
}
class Parent{
    int value =0;
    Parent(){
        addValue();
    }
    void addValue(){
        value +=10;
    }
    int getValue(){
        return value;
    }
}
class Child extends Parent{
    Child(){
        addValue();
    }
    void addValue(){
        value += 20;
    }
}
Output: 40

=======
public class MainClass {
    public static void main(String[] args) {
        Parent p = new Child();
        p.printValue();
        System.out.println(p.i);
    }
}
class Parent{
    int i =10;
    void printValue(){
        System.out.println("Value-A");
    }
}
class Child extends Parent{
    int i = 12;
    void printValue(){
        System.out.println("Value-B");
    }
}
Value-B
10

=======
In java, you can either do widening, boxing or boxing-then-widening.

=======
In Java 6, Collections.Sort() uses the modified merge sort.

=======
public class MainClass {
    public static void main(String[] args) {
        Thread r = new Thread(new Runnable());
        r.start();
    }
    static class Runnable extends Thread{
        public void run() {
            System.out.println("Run");
        }
        public synchronized void start() {
            System.out.println("Start");
        }
    }
}
Output: Run

=======
public class MainClass {
    public static void main(String[] args) {
        Runnable r = new Runnable();
        r.start();
    }
    static class Runnable extends Thread{
        public void run() {
            System.out.println("Run");
        }
        public synchronized void start() {
            System.out.println("Start");
        }
    }
}
Output: Start

=======
public static void main(String[] args) {
    System.out.println(foo());
}
private static int foo(){
    try{
        return 1;
    }finally {
        System.out.println("Finally");
        return 2;
    }
}
Output: 
Finally
2

=======================
Round Robin Scheduling
=======================
Round Robin is a CPU scheduling algorithm where each process is assigned a fixed time slot in a cyclic way.
- It is simple, easy to implement, and starvation-free as all processes get fair share of CPU.
- One of the most commonly used technique in CPU scheduling as a core.
- It is preemptive as processes are assigned CPU only for a fixed slice of time at most.
- The disadvantage of it is more overhead of context switching.

https://www.geeksforgeeks.org/gate-notes-operating-system-process-scheduling/

https://www.geeksforgeeks.org/cache-memory/
