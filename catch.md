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
