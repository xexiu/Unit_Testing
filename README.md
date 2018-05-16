# Unit_Testing
> How and why SHOULD make tests for our code/methods??!! Mock vs spy in mockito. Unit Testing. Examples. Use Cases in Marfeel.

# 1. Why doing tests?
 - Developers use unit tests as an internal control on the functionality and compatibility of their applications when changes to features, code or the environment happen.

> Imagine the scenario:
- We have our app on version 2.1 and its working perfectly.
- Meanwhile, our team is working to release the new version 2.2
- After some months of working, fixes, and bug solving, the new version is released and uploaded to production.
- Some days after this new release, the team wants to add new features but seems that what they want to do, completely fails and causes more bugs.
- They are furious because this feature worked in version 2.1 but not on the newly released version 2.2
- The team admits that they only tested the new functionality of version 2.2
- After some investigation, the development team realizes that some changes to the new version have affected more core parts of the application.

> THE RESULT: users are unhappy and not trusting your app, the company is unhappy with the extra effort and development team is afraid of refactoring.
> This is what unit tests are attempting to solve — if you can solve this problem by catching regressions in application code, then all the things that worked in the previous version should also work in the next.

# 2. Unit Testing to the rescue!!

a. What you SHOULD NOT test:
* Other framework libraries.
* The database.
* Other external resources (image for example).
* Really trivial code (like getters and setters for example).
* Code that has non-deterministic results (like random numbers).

b. What you SHOULD test:
https://zeroturnaround.com/wp-content/uploads/2015/12/YourAppV2-min.png

- In this case, the highlighted part in gold is where you should focus your testing efforts. 
- This is the part of the code where usually most bugs manifest.

> You should prioritize things a bit and just write tests for the following:
* Core code that is accessed by a lot of other modules.
* Code that seems to gather a lot of bugs.
* Code that changes by multiple different developers (often to accommodate new requirements).

> Let's take as an example of e-shop product (tests on JUnit):
 * Let's assume we have the following code/module that calculates the total weight of an order.
```java
public class BasketWeightCalculator {

	private int totalWeight = 0;
	private int maxSize = 50;

	public void addItem(int itemWeight) {
		totalWeight = totalWeight + itemWeight;
	}

	public int getTotalWeight() {
		return totalWeight;
	}

	public void resetTotalWeight() {
		totalWeight = 0;
	}

	public string showMsg() {
		if(totalWeight >= maxSize) {
			return "Your reached the maximum size allowed!!";
		} else {
			return "The total weight has not reached its maximum size...";
		}
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
	BasketWeightCalculator weightCalculator = new BasketWeightCalculator();

	@Before
	public void beforeEachTest(){
		// runs before each test
	}

	@After
    public void reset() {
		// runs after each test
		weightCalculator.resetTotalWeight();
    }

	@Test
	public void shouldCalculateTotalWeightWithOneItem() {
		weightCalculator.addItem(5);

		assertEquals(5 ,weightCalculator.getTotalWeight());
	}

	@Test
	public void shouldCalculateTotalWeightWithTwoItems() {
		weightCalculator.addItem(5);
		weightCalculator.addItem(13);

		assertEquals(18 ,weightCalculator.getTotalWeight());
	}

	@Test
	public void shouldCheckTheMaximumAllowedSize(){
		weightCalculator.addItem(52);

		assertEquals("Your reached the maximum size allowed!!", weightCalculator1.showMsg());
	}

	@Test
	public void shouldCheckTheMinimumAllowedSize(){
		weightCalculator.addItem(49);

		assertEquals("The total weight has not reached its maximum size...", weightCalculator1.showMsg());
	}

	@Test
	public void theOrderOfTheItemsShouldNotMatter() {
		weightCalculator.addItem(5);
		weightCalculator.addItem(13);

		BasketWeightCalculator weightCalculator1 = new BasketWeightCalculator();
		weightCalculator1.addItem(12);
		weightCalculator1.addItem(6);

		assertEquals(weightCalculator.getTotalWeight(), weightCalculator1.getTotalWeight());
	}
}
```
TO BE CONSIDERED WHEN TESTING:

- The name of the class is “name of the class being tested” + “Test” This is not a strict requirement but it is a good convention followed by several tools. Ex: BasketWeightCalculatorTest
- The test methods are annotated with the @Test annotation, but the test methods themselves can have any name.
- The most crucial step is the assert Method, found at the end of each test method. This indicates whether the test fails or not.
- The execution path. In the "showMsg" method, for example, we have an if/else condition. We should make two execution paths thats checks the result of the if/else condition.
- We can make usage of @Before (runs before each test) and @After (runs after each test) annotations to not repeat/duplicate code or just to setup some environment configuration.
- JUnit also supports parameterized tests. I encourage you to read: https://github.com/junit-team/junit4/wiki/Parameterized-tests

WHAT TO EXPECT?
- There are several assert methods for each basic Java type but their syntax is similar. 
- In this case, the second argument should be the same as the first argument.
- If those two match, then the test is a success. If not the test fails.
- Now you can save this file in your source folder and since we are using IntelliJ/Eclipse for this example, you can right-click on it and select “run test”. 
- If everything goes ok you will see the infamous green bars!

# 3. Spy vs Mock (Jasmine):

> What is Jasmine?
- Most of you already know that we use Jasmine for testing here in Marfeel (javascript). 
- Jasmine is an open source testing framework for JavaScript.
- It aims to run on any JavaScript-enabled platform, to not intrude on the application nor the IDE, and to have easy-to-read syntax.

> Features
- Supports asynchronous testing.
- Makes use of 'spies' for implementing.

> Usage - user system
```javascript
class User {
	constructor(name) {
		this.authenticated = false;
		this.admin = false;
		this.name = name;
	}

	getName(){
		return this.name;
	}

	setName(name){
		this.name = name;
	}

	isAuthenticated() {
    	return this.authenticated;
  	}

	isAdmin() {
		return this.admin;
	}
}
```
> Test
```javascript
describe('User Login', () => {
	let user,
		mockAdmin;

	// We can instead simply extend the class and override one or more specific function in order to get them to return the test responses we need, like so:
	
	class MockUser extends User {
		this.admin = true;

		isAdmin() {
			return this.admin;
		}
	}

	beforeEach(() => { 
    	user = new User('Foo');
		mockUser = new MockUser();  
  	});

  	afterEach(() => { 
    	user = null;
  	});

	it('Get the name of the user', function() {
		expect(user.getName()).toEqual('Foo');
	});

	it('Set the name of the user', function() {
		user.setName('Bar');

		expect(user.getName()).toEqual('Bar');
	});

	it('Should not authenticate the user', function() {
		expect(user.isAuthenticated()).toBe(false);
	});

	it('Should authenticate the user', function() {
		spyOn(user, 'isAuthenticated').and.returnValue(true);

		expect(user.isAuthenticated()).toBe(true);
	});

	it('User should be an admin', function() {
		expect(mockUser.isAdmin()).toBe(true);
	});
});
```

- There are a lot of matchers for Jasmine and lots of build in methods to be used on your testing.
- Check them out here: https://jasmine.github.io/2.0/introduction.html

a. What is a Mock and when to use it?
- A Mock object is a simulated object instance used to mimic a classes behavior using the same interface.
- In Jasmine, mocks are referred to as spies.
- In simpler terms, this is a fake class with the same method signature as the Real Thing.
- A mock object can be composed of multiple objects (and sometimes multiple mock objects), but most often should be created as simply as possible to keep your tests easily maintained.

b. What is a spy and when to use it?
- A Spy is a feature of Jasmine which lets you take an existing class, function, object and mock it in such a way that you can control what gets returned from functions.
- There are two ways to create a spy in Jasmine: spyOn() can only be used when the method already exists on the object, whereas jasmine.createSpy() will return a brand new function

```javascript
//spyOn(object, methodName) where object.method() is a function
spyOn(obj, 'myMethod')
 
//jasmine.createSpy(stubName);
const myMockMethod = jasmine.createSpy('My Method');
```
# 4. Use cases (Marfeel):
> We make use of tests for the follwing cases:
- Test core dependencies (MarfeelXP/Dixie/src/test > 3pi-widgets, garda, touch, k, cherokee, sw, commons, etc).
- Test MetaProviders.
- Test VideoDetectors.
- Test NextPages.
- Test AdDealer.
- etc.


# 5. Q & A
