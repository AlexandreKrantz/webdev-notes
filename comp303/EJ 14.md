#### Consider implementing Comparable
CompareTo is the sole method in Comparable interface.
Similar to object's equals method.
==By implementing Comparable, a class indicates that its instances have a natural ordering.==
Strings implement Comparable, the following program prints an alphabetized list of its command-line arguments with duplicates eliminated:
```java
public class WordList {
	public static void main(String[] args) {
		Set<String> s = new TreeSet<>();
		Collections.addAll(s,args);
		System.out.println(s);
	}
}
```
All the value classes in the Java platform libraries, as well as all enum types, implement Comparable.
Comparable interface:
``` java
public interface Comparable<T> {
	int compareTo(T t);
}
```
CompareTo, compares this object with the specfied object for order. Returns a negatve, zero , or a positive integer as this object is less than, equal to, or grater than the specifed object.
Throws `ClassCastException` if the specified object's type prevents it from being compared to this object.
In the following description, the notation sgn(expression) designates tha mathematical signum function, which is defiend to return -1,0,1 according to wether the value of expression is negative, zero, or positive.
- The implementor must ensure that sgn(x.compareTp(y)) = - sgn(y.compareTo(x)) -> it must throw an exception only if the other one is throwing an exception.
- The implementor must make sure that the relation is transitive: (x.compareTo(y) > 0 && (y.compareTo(z) >0) implies x.compareTo(z) >0.
- finally if x.compareTo(y) == 0 implies that sgn(x.compareTo(z)) == sgn(y.compareTo(z)), for all z.
- it is strongly recommended but not required that (x.compareTo(y) == 0) == (x.equals(y))
if the last provision is obeyed, the ordering imposerd by compareTo method is consistent with equals. otherwise it is inconsistent.

compareTo does not need to work across objects of different types. -> `ClassCastException`
Classes that depend on camparison include the sorted collections TreeSet and Treemap and the utility classes Collections and Arrays

The first provision says that if you reverse the direction of a comparison the expected things happen.
The second provision says that if an object is greater than a second one, and the second one is greater than a third. Then the first must be greater than the third.
The last provision says that all objects that compare as equal must yield the same results when compared to any other object.

Because the Comparable interface is parameterized, the compareTo method is statistically typed, so you don't need to type check cast its arguments.

Statically typed -> ==variable types are known at compile time==

In a compareTo method, fields are compared for order rather than equality. 
To compare object reference fields, invoke the compareTo method recursively.
if a field does not implement Comparable or you need a nonstandard ordering, use a Comparator instead. you can write your own comparator or use an existing one.
```java
//Single-field Comparable with object reference field
public final class CaseInsensitiveString implements Comparable<CaseInsensitiveString> {
	public int compareTo(CaseInsensitiveString cis){
		return String.CASE_INSENSITIVE_ORDER.compare(s, cis.s)
	}
	... //Remainder omitted
}
```
If a class a has multiple significant fields, the order in which you compare them is critical, Start with the most significant field and work your way down. 
If a comparison result is anything other than zero (which represents equality), you are done; just return the result.
compareTo method for the PhoneNumber class in Item 11:
```java
public int compareTo(PhoneNumber pn) {
	int result = Short.compare(areaCode, pn.areaCode);
	if (result == 0 ){
		result = Short.compare(prefix, pn.prefix);
		if (result == 0) {
			result = Short.compare(lineNum, pn.lineNum);
		}
	}
	return result;
}
```