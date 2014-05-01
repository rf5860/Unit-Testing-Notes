Unit-Testing-Notes
==================

Notes on Different Testing Strategies

- Hamcrest - We were previously using 1.1. Upgraded our libraries to 1.3 early this week.
 - Matchers
   Nice (new) matchers:

```java
assertThat(1, is(both(greaterThanOrEqualTo(1).and(lessThanOrEqualTo(2)))));
```

 - Writing your own

We want to do something pretty simple. Imagine we have a date, and want to check if occurs between yesterday and today. Pretty basic:

```java
Calendar today = Calendar.getInstance();
Calendar yesterday = Calendar.getInstance();
yesterday.add(Calendar.DATE, -1);
Calendar tomorrow = Calendar.getInstance();
tomorrow.add(Calendar.DATE, 1);
```

Then our check is just:
```java
assertThat(today, is(both(greaterThanOrEqualTo(yesterday)).and(lessThanOrEqualTo(tomorrow))));
```

Somewhere along the lines though, we lost some readability here. It'd be nice if we could abstract the right-hand side away to some other more readable form. Luckily, we can.

```java
public static Matcher<Calendar> between(Calendar start, Calendar end) {
    CombinableBothMatcher<Calendar> both = both(greaterThanOrEqualTo(start));
    return both.and(lessThanOrEqualTo(end));
}
````

This is nice - but we can do better. We're just dealing with calendars here, but the concept of what we're doing checking that "something" is in a range applies to more types than just Dates. The most obvious other example being numbers! We can take advantage of generics to come up with a solution that works with a broader range of types (Anything comparable):

```java
// From our AssertUtil Library
public static <T extends Comparable<T>> Matcher<T> between(T start, T end) {
    CombinableBothMatcher<T> both = both(greaterThanOrEqualTo(start));
    return both.and(lessThanOrEqualTo(end));
}
```

Once written, can be used like normal

```java
assertThat(today, is(between(yesterday, tomorroww)));
```

Awesome!

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
