---
id: code_review_guide
title: Android CodeReview Guide
hide_title: true
---

# Android CodeReview Guide

#### Conventions
1. Whether the code is standardized (whether the package name, class name, variable name, resource file name, resource id, etc. conform to the naming convention)，[For details, please refer to here](https://confluence.toolsfdg.net/display/Technology/Android+Coding+Conventions)

2. The number of commits for a PR should not exceed 5
#### Robustness
1. Whether the variable of the nullable type has a null check
2. Whether the array has a index check to prevent the IndexOutOfBoundsException
3. Avoid using !!, use requireNotNull instead
4. Are there any uncaught exceptions
  - Kotlin removed CE, some non-runtime exceptions can be compiled without try catch，such as FileNotFoundException,IOException和SQLException
  - The reviewer can judge whether to add try catch to some runtime exceptions based on experience and actual conditions

5. Whether the used resources can be released or closed normally, such as io stream and cursor and other resources

6. Whether the code is thread-safe, such as hashmap and arraylist, etc. are thread-unsafe, so pay attention to use
7. Can global variables be replaced by local variables

#### Comments
1. Is there any comment on the SDK method provided externally?
2. Are there any comments for some core methods
3. Are there any comments for some important algorithms

#### Logical
1. Whether the code logic in different conditional branches is the same, if the same, should to delete or merge the branches

2. Whether the minimum supported version of the api used is higher than the minSdkVersion of the project

3. Can the log or debugging code be deleted?

4. Do all the beans of the network request use string? type to receive data, and are there any proguard configuration?



#### Additional
1. Whether the code can meet business needs

2. Whether the code has been formatted, the template configuration can be unified among team members

3. Are there duplicate code blocks, and if so, can they be encapsulated into methods

4. Whether the data processing in the code is separated from the rendering of the view, and whether the data processing is completed in an asynchronous thread

5. Try to eliminate warnning in the code

6. Determine the storage location according to the scope of the code. The common classes/tool classes that are currently or expected to be used by multiple modules need to be sink to lib-base

