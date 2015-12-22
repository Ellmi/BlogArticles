title: Writing Email Regular Expression with TDD Thinking
tags: [TDD,Regex]
postLink: using-tdd-writing-email-regex
date: 2015-12-19 21:30:14
index: In this article we will explore the email Regex with TDD thinking which is good for beginner to understand TDD and learn Regex. The reason of TDD Thinking not TDD in title is there is a little difference between the real project and my demo in implementation details. And Regex as an useful weapon of developer we really has no reason not to get close to it...
---
In this article we will explore the email Regex with TDD thinking which is good for beginner to understand TDD and learn Regex. The reason of **TDD Thinking** not **TDD** in title is there is a little difference between the real project and my demo in implementation details. And Regex as an useful weapon of developer we really has no reason not to get close to it？

## TDD Thinking

TDD（Test Drive Development）thinking is a process：thinking what you want before logic composing when developer write a function or interface.And your logic developing is driven step by step by your thinking results.

Specifically, expressing "what I want" depends on tests in TDD. For example, I wanna write a `function A` and the first thing is thinking what I want A do. Assuming my conclusion is 'Store data to result only when flag field x is true' then we need to do,

* Write one test to cover one condition of our thinking results.
* Developer write functional code to let test pass.

Repeate above process write another test again, modify functional code again until the `function A` match all your wishes。

## Regular Expression

Regular Expression (Regex) mainly used in string matching. And we can always see it's appearence in path matching,log processing,infomation validating. And you will in it strongly after you refactor a number of line's code into one by using Regex.

## Demo

**Goal：Using TDD thinking writing email Regex**

Language：java
Framework：junit
IDE：idea

In the initial exercise of TDD, you may meet with "Woo, testing first? What should I do, I have no idea totally". One thing you may need this time is **baby step** , liking baby learn to walk just give a simplest and smallest step. First let's think that what an email address regex should be(the same with we consider a valid email address should like what), just go down with a simplest thought. For me it is

* Email address must contains a ‘@’ character in the middle.Means the regex require a valid emails must contains ‘@’.

``` java
@Test
public void shouldHasAtMarkInEmail() {
    assertTrue("Should has a @ mark in the middle!", Pattern.matches(emailRegex, "yanmin@test.com"));
    assertFalse("Should has a @ mark in the middle!", Pattern.matches(emailRegex, "asdfhj"));
    assertFalse("Should has a @ mark in the middle!", Pattern.matches(emailRegex, "@hjddh"));
    }
```

**Run test：failed**

In test, `emailRegex` is the regex we wanna get. It can't pass compile if we run test at this moment cause we even did not give a definiation of the variable `emailRegex` . Thus in order to pass through the test we are driven to write the functional part. As below:

``` java
String emailRegex = ".＋@.＋";
```

**Run test：successful**

Note: In real project we will call the interface/function in test but not include the functional code in. Besides in most case will not write tests just for getting regexp.

---
Finished our baby step then to add the second test,for driving us write a better regex.

* The characters in email address around ‘@’ only can be'a-z' or '.', means the regex require a valid emails match this rule.

``` java
@Test
public void shouldBeSpecificCharacters() {
    assertTrue("Characters should be a-z or dot!", Pattern.matches(emailRegex, "yanmin@test.com"));
        assertFalse("Characters should be a-z or dot!", Pattern.matches(emailRegex, "asASh@hjddh"));
    }
```

**Run all 2 tests：first test successful, the second one failed**

modify `emailRegex` as follow：

``` java
 String emailRegex = "[a-z.]+@[a-z.]+";
```

**Run all 2 tests：all successful**

---
add third rule

* '.' only can appear after ‘@’, 1~3 times, can't be adjacent with another one and not at the end position, means the regex require a valid emails match this rule.

``` java
@Test
public void shouldHasCorrectDotMarks() {
    		assertTrue("Wrong dot!", Pattern.matches(emailRegex, "yanmin@test.com"));
        assertFalse("Wrong dot!", Pattern.matches(emailRegex, "asadf@hj"));
        assertFalse("Wrong dot!", Pattern.matches(emailRegex, "asadf@.sfsd.com"));
        assertFalse("Wrong dot!", Pattern.matches(emailRegex, "asa.df@sfsd.com"));
        assertFalse("Wrong dot!", Pattern.matches(emailRegex, "asadf@hj..com"));
    }
```

**Run all 3 tests：first 2 tests successful, the third failed**

modify `emailRegex` as follow：

``` java
 String emailRegex = "[a-z]+@[a-z]+([a-z]+\.){1,3}[a-z]+";
```

**Run all 3 tests：all successful**

---
add forth rule

* There is 2~4 characters after the last ‘.’ in email, means the regex require a valid emails match this rule.

``` java
@Test
public void shouldHasTwoToFourCharactersAfterTheLastDot() {
    		assertFalse("Wrong ending!", Pattern.matches(emailRegex, "ymxing@test.comcosf"))
    }
```

**Run all 4 tests：first 3 successful, the forth failed**

modify `emailRegex` as follow：

``` java
 String emailRegex = "[a-z]+@[a-z]+([a-z]+\.){1,3}[a-z]{2,4}";
```

**Run all 4 tests：all successful**

---
add fifth rule

* The max length of email is 50 characters, means the regex require a valid emails match this rule.

``` java
@Test
public void shouldHasMaxLengthFifty() {
    		assertFalse("Wrong length", Pattern.matches(emailRegex, "asdfghjklkjasdfgdsdfdsasdfghjgfdsadfghfhgfdsasdfghjkjhgfdjhgfg@thoughtworks.com"));
    }
```

**Run all 5 tests：first 4 tests successful, the fifth failed**

modify `emailRegex` as follow：

``` java
 String emailRegex = "^(?=.{1,50}$)[a-z]+@([a-z]+\.){1,3}[a-z]{2,4}";
```

**Run all 5 tests：all successful**

Now one email address regex was done by TDD. Do you get the process and thinking of TDD? As for the regxp in this article we are not focus on it, please check the related articles in this website to learn deeply.

