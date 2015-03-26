# sfntly C++ Porting Guidelines #

## Goal ##
A **correct**, **faithful** port of the Java version sfntly.

## How It Works ##

  1. Take one snapshot of the Java source, run all its unit tests and make sure there are no known problems.
  1. Coordinate with other developers to work on the same snapshot.  Once the snapshot is done, then move on next version.
  1. **The code must be built and tested on all three supported platforms (Windows, Linux, Mac).**  Our clients count on us to do a good job.

The least thing a faithful port wants is out-of-sync versions among files.

## Guidelines ##
  * Follow Google C++ Style Guide http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml with following exceptions:
    * Exception handling is used. This is unavoidable when we port Java programs. However, the port must also consider clients that do not use exceptions (i.e. `SFNTLY_NO_EXCEPTION` is defined).
    * Nested class are allowed.  We try to keep the same class structure for better maintainability between C++ and Java branches.

  * File structure mapping:
    * `src/sfntly` maps to `java/com/google/typography/fonts/sfntly` and all include files stay with the source.  Keep its folder structures intact.
    * All C++ port specific files are placed under `src/port`.
    * All external dependencies are placed under `ext`.
    * All data files are placed under `data`.  Data acquired from external sources are placed under `data/ext`.
    * Tests and samples does not map to their Java counterpart and may have different set of tests and samples.  For example, smart pointer unit tests are meaningless in Java.  Tests are placed under `src/test` and samples are placed in `src/sample`.
    * Output files are stored in `bin` and `lib` folder.
    * We have a `vc` folder to store all vcprojects, and two solution files in root. The goal is to perform everything through cmake and sunset these files.

  * Keep existing class structures, except:
    * Namespaces are flatten to sfntly to simplify usage.
    * Public enums are promoted to sfntly namespace within a struct.
    * When we need to get around compiler bugs or compilation limitations.

  * Keep existing naming. Names are converted to conform C++ style guide but should be easily associated with their Java counterpart.

  * For container data types, they are not returned as object references.  Instead, use an additional function parameter to pass it in.  For example:
```
byte[] getBuffer();  // Java version, callee new the byte array.
void getBuffer(ByteVector* buffer);  // C++ version, caller allocate buffer.
```

  * C++ port uses reference-counted object. Please refer to [smart\_pointer\_usage](smart_pointer_usage.md).

  * Existing comments in Java code will be copied and maintained in a best-effort manner.  C++ only comments typically starts with `Notes:`.  For example:
```
// Notes: C++ port only.
```