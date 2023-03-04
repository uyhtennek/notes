[Test Framework | 1.3.3](https://docs.unity3d.com/Packages/com.unity.test-framework@1.3/manual/course/welcome.html)
At Unity, our main way of testing content is using the Unity Test Framework.
___

# Setup and Write Tests

* make sure to add the assembly definition reference
* remembe to include the namespace of the script to test if there is any
___

# AAA
= Arrange, Act, Assert

this is an industry standard in unit testing
* in the first part of the code, we arrange all the elements needed for the test
* in the middle part, we act on the object that is under test
* in the final part, we assert on the result of the act part

```c#
[Test]
public void StringWriterTest()
{
	// Arrange
	var stringWriterUnderTest = new StringWriter();
	stringWriterUnderTest.NewLine = "\\n";
	var testStringA = "I am testing";
	var testStringB = "with new line";
	
	// Act
	stringWriterUnderTest.WriteLine(testStringA);
	stringWriterUnderTest.WriteLine(testStringB);
	
	// Assert
	Assert.AreEqual("I am testing\\nwith new line\\n", stringWriterUnderTest.ToString());
}
```

`stringWriterUnderTest`
* it's good practice to use `XUnderTest` as a variable name of the class or object that is being tested
	* helps to keep the focus of the test clean

if a test can't fit in to a AAA structure,
it's probably a sign that the test needs to be splited into multiple tests
___

# Semantic test assertion

the NUnit test framework and the Unity Test Framework have a series of classes for asserting objects in a way that is closer to natural language

eg.
```c#
Assert.That(myValue, Is.GreaterThan(20));
Assert.That(number, Is.EqualTo(11));
Assert.That(number, Is.GreaterThan(19.33f).And.LessThan(19.34f));
Assert.That(str, Does.Contain("string").And.Contain("asserted"));
```
___

# Custom comparison

Unity Test Framework has extended the assertion capabilities of NUnit with some custom comparisons for Unity-specific objects
* [Custom equality comparers | Test Framework | 1.1.33](https://docs.unity3d.com/Packages/com.unity.test-framework@1.1/manual/reference-custom-equality-comparers.html)

eg. compare two `Vector3` objects
```c#
actual = new Vector3(0.01f, 0.01f, 0f);
expected = new Vector3(0.01f, 0.01f, 0f);

Assert.That(actual, Is.EqualTo(expected).Using(Vector3EqualityComparer.Instance));
```
* it allows us to verify that the two vectors are identical within a given tolerence
	* by default the tolerance is `0.0001f`

* we can change the tolerance by providing a new `Vector3EqualityComparer` instead of using the default `.Instance` one
```c#
// up the tolerance to 0.01f
Assert.That(actual, Is.EqualTo(expected).Using(new Vector3EqualityComparer(0.01f)));
```

If the comparison fails, the comparers give a message about the actual and expected value, just like a normal assertion
* However, because `ToString` rounds value off before displaying it, the two values in the string message might look equal but in fact they might be not
___
