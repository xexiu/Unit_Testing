# Unit_Testing
How and why SHOULD make tests for our code/methods??!! Mock vs spy in mockito. JUnit. Examples

# 1. Why doing tests?
 - Developers use unit tests as an internal control on the functionality and compatibility of their applications when changes to features, code or the environment happen.
Imagine the scenario:
- We have our app on version 2.1 and its working perfectly.
- Meanwhile, our team is working to release the new version 2.2
- After some months of working, fixes, and bug solving, the new version is released and uploaded to production.
- Some days after this new release, the team wants to add new features but seems that what they want to do, completely fails and causes more bugs.
- They are furious because this feature worked in version 2.1 but not on the newly released version 2.2
- The team admits that they only tested the new functionality of version 2.2
- After some investigation, the development team realizes that some changes to the new version have affected more core parts of the application.
- THE RESULT: users are unhappy and not trusting your app, the company is unhappy with the extra effort and development team is afraid of refactoring.

- This is what unit tests are attempting to solve — if you can solve this problem by catching regressions in application code, then all the things that worked in the previous version should also work in the next.

# 2. JUnit to the rescue!!
- Most Java IDEs (i.e. Eclipse, IntelliJ IDEA, NetBeans) have explicit support for JUnit, making it ideal for those of you who are interested in getting started with unit testing.

a. What you SHOULD NOT test:
* Other framework libraries (you should assume they work correctly)
* The database (you should assume it works correctly when it is available)
* Other external resources (again you assume they work correctly when available)
* Really trivial code (like getters and setters for example)
* Code that has non-deterministic results (Think Thread order or random numbers)
* Code that deals only with UI (e.g. Swing toolkit, Wicket)

b. What you SHOULD test:
- https://zeroturnaround.com/wp-content/uploads/2015/12/YourAppV2-min.png (consider this image)
- In this case, the highlighted part in gold is where you should focus your testing efforts. 
- This is the part of the code where usually most bugs manifest. 
- It is also the part that changes a lot as user requirements change since it is specific to your application.
- You should prioritize things a bit and just write tests for the following:
* Core code that is accessed by a lot of other modules
* Code that seems to gather a lot of bugs
* Code that changes by multiple different developers (often to accommodate new requirements)

- Real example with an e-shop product:
 * Let's assume we have the following code/module that calculates the total weight of an order.
```java
public class BasketWeightCalculator {

	private int totalWeight = 0;

	public void addItem(int itemWeight) {
		totalWeight = totalWeight + itemWeight;
	}

	public int getTotalWeight() {
		return totalWeight;
	}

}
```
* Now it’s time to write the unit test.
* The suggested way of writing unit tests is to do it gradually. 
* You start with very basic assumptions and continue to more advanced ones. 
* The first thing to test, of course, is a simple case of using just one or two items.

```java
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class BasketWeightCalculatorTest {
	@Test
	public void oneItem() {
		BasketWeightCalculator weightCalculator = new BasketWeightCalculator();
		weightCalculator.addItem(5);

		assertEquals("Expected 5",5 ,weightCalculator.getTotalWeight());
	}

	@Test
	public void twoItems() {
		BasketWeightCalculator weightCalculator = new BasketWeightCalculator();
		weightCalculator.addItem(5);
		weightCalculator.addItem(13);

		assertEquals("Expected 18",18 ,weightCalculator.getTotalWeight());
	}

	@Test
	public void orderDoesNotMatter() {
		BasketWeightCalculator weightCalculator1 = new BasketWeightCalculator();
		weightCalculator1.addItem(5);
		weightCalculator1.addItem(13);

		BasketWeightCalculator weightCalculator2 = new BasketWeightCalculator();
		weightCalculator2.addItem(13);
		weightCalculator2.addItem(5);

		assertEquals("Expected 18",18 ,weightCalculator1.getTotalWeight());
		assertEquals("Expected 18",18 ,weightCalculator2.getTotalWeight());
	}
}
```
TO BE CONSIDERED (3 entry points):

- The name of the class is “name of the class being tested” + “Test” This is not a strict requirement but it is a good convention followed by several tools. Ex: BasketWeightCalculatorTest
- The test methods are annotated with the @Test annotation, but the test methods themselves can have any name.
- The most crucial step is the assert Method, found at the end of each test method. This indicates whether the test fails or not.

WHAT TO EXPECT?
- There are several assert methods for each basic Java type but their syntax is similar. 
- The second argument is the expected result, while the final argument is the actual result. 
- If those two match, then the test is a success. If not the test fails.
- Now you can save this file in your source folder and since we are using IntelliJ/Eclipse for this example, you can right-click on it and select “run as a unit test”. 
- If everything goes ok you will see the infamous green bars!

# 3. Spy vs Mock in Mockito:
a. What is a Mock and when to use it?
- Bla Bla
- Example
b. What is a spy and when to use it?
- Bla bla
- Example

# 4. Final Notes and use cases (Marfeel):
- Sources (bla bla)
- Examples (bla bla)
- Tutorials (bla bla)
- Use cases in Marfeel (bla bla)

# 5. Q & A