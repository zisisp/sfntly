# Unit Test #

sfntly C++ port uses Google testing framework for unit testing.

All test cases are under src/test.

To run the unit test, you need to compile sfntly first, then copy Tuffy.ttf from data/ext to the folder that unit\_test(.exe) is located.  Run the unit\_test.

If you are only interested in one test case, for example, you are working on name table and only wants name editing test, you can use the gtest\_filter flag in command line.

```
unit_test --gtest_filter=NameEditing.*
```

For more info regarding running a subset of gtest, please see [GTest Advanced Guide](http://code.google.com/p/googletest/wiki/V1_6_AdvancedGuide).

It's strongly suggested to run unit\_tests with valgrind.  The following example shows a typical test command

```
valgrind --leak-check=full -v ./unit_test --gtest_filter=*.*-FontData.*
```

FontData tests are the longest and you may want to bypass them during development.  They shall be run in bots, however.