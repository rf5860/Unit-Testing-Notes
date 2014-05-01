Unit-Testing-Notes
==================

Notes on Different Testing Strategies

- Hamcrest 
 - New Items in 1.3
 - Matchers
   Nice (new) matchers:

```java
assertThat(1, is(both(greaterThanOrEqualTo(1).and9(lessThanOrEqualTo(2)))));
```

 - Writing your own

```java
// From our AssertUtil Library
public static <T extends Comparable<T>> Matcher<T> between(T start, T end) {
    CombinableBothMatcher<T> both = both(greaterThanOrEqualTo(start)); // <-- We need to do this on a separate line because of a JDK Compiler Issue.
    return both.and(lessThanOrEqualTo(end));
}
```

<3>

<4>

- Fest
 - Comparison to Hamcrest
  - Less imports (Not so much an issue with Eclipse Favourites)
  - Slightly different syntax ==> Some items are easier to read
<Code 1>

vs.

<Code 2>

- JUnit 4
 - New features
  From JUnit 4.4 onwards, Hamcrest (the core part) bundled with JUnit
