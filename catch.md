## Unit Testing Fundementals Using Catch Framework

after craeting executable, you can use *-h*, *--help* or *-?* to see usage.

- *-f* option specifies input file which contains command line args

- *-o* or *--out* specifies output file, all stdout and stderr goes there

- *-a* or *--abort* stops tests after first fail


### Tags

Tags are included to the test file within square brackets, like in the following examples:

```c++
TEST_CASE("Test Name", "[tag1][tag2]")
```

Use can test for only specific tags, like following:
```
test.exe [tag1]
```

- *-t* option lists all of the tags used in the test files
- *-l* lists tests
- *-#* shows filenames


### Asserting

***REQUIRE*** is the keyword, appears at the end of the code
- Single macro for all/most assertions needs
- Assertion is written in plain code

```c++
TEST_CASE("This is a test name", "[Tag]"){
	MyClass myClass;

	REQUIRE(myclass.f() == 42);
}
```


*In the world of unit testing, the rule "single assert per test" as a best practice.*

***CHECK*** is another assertion keyword, it is almost same with ***REQUIRE***. The only difference is ***CHECK*** lets the test finish before failing it.

**!Note:** we can not use *!* in the assertions, instead there are two other asserions to check false results; ***REQUIRE_FALSE*** and ***CHECK_FALSE***.


multiple assertion example:

```c++
TEST_CASE("Find one letter word"){
	auto result = function() // returns a std::vector<string>

	CHECK(result.size() == 2);
	REQUIRE(result[0] == "abc");
}
```

or better way:

```c++
TEST_CASE("Find one letter word"){
	auto result = function() // returns a std::vector<string>

	auto = result = function();
	auto expected = vector<string>({"abc", "def"})''
	REQUIRE(result == expected);
}
```

**Catch Framework** has the ability for checking exceptions. Keywords are ***REQUIRE_THROWS*** and ***CHECK_THROWS***. And Both can be used in the same manner as corresponding asserion keywords.


#### More information to tests:

There are 4 kinds of macros in Catch.

- **INFO:** The message will be reported only if the test fails.

- **WARN:** Similar to INFO, but will always show regardless if the test fails or not.

- **FAIL:** Shows as well as a fail, immediately.

- **CAPTURE:** We can use to log the name of a value of a variable and works just like INFO.


Example:
```c++
INFO("Passed first step");
INFO("Customer name is: " << customer.get_name());

CAPTURE(someVarialbe); // someVariable := 3
```

To compare complex objects (i.e class types) you need to overload comparator operator, *==*. Or you can write toString method in the namespace Catch. But the values aren't shown exactly, instead you see a question mark. To be able to see each of the fields of the object, we need to overload the operator **

```c++

class SomeClass {
	public:
		int my_int;
		double my_double;

}


TEST_CASE("Test name"){

	REQUIRE(result == expected);
}

```


## Code Duplication in Automated Tests

**unit test:** test a single unit of work. They are focused, isolated, fast.

**integration test:** test in externalized enviroment. We can test DB, services and so on. They are extensive, slower and depends on enviroment.


***DRY:*** Don't repeat yourself.
***DAMP:*** Descriptive and meaningful phrases.


We can test a class with **TEST_CASE_METHOD** keyword, and they are called fixtures.

```c++
class MyFixture {

	MyFixture(){}
	~MyFixture(){}
};

TEST_CASE_METHOD(MyFixture, "Test name"){

	....
}
```


***Disadvantages of Fixtures:*** Hard to read and understand, difficult to fix failures, hides some of the test logic (especially in the constructor).


Better way of testing is using **SECTION**s. They reside inside the test.

Each section is a fork in which the code runs as a separate test, but following the common parts before or with the common parts after that specific section. For example, in the following code section 1 and 2 run the same initialization code.


```c++

TEST_CASE("This is a test case") {

	// initialization


	SECTION("Test section 1"){
		// test code
	}

	SECTION("Test section 2"){
		// test code
	}
}
```


### Behaviour Driven Development

In BDD, instead of text cases and text fixtures we usually have user stories and scenarios. User stories defined by product owner. And that story is split into scenarios. And scenarios are defined in three parts; **Given** , **When**, **Then**. See the following example


```c++

SCENARION("If words are known entering their encoding will return all valid words."){

	GIVEN("Hello is a known word"){

			Engine t9engine;
			t9engine.learnWords({"Hello"});

			WHEN("Called with the right encoding"){
					Digit digits= {4,3,3,5,6};

					auto result = t9engine.getwordsbydigits(digits);

					THEN("return hello"){

						REQUIRE(result.size() == 1);
						REQUIRE(result[0] == "hello");
						
					}

			}

	}

}


```

