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
