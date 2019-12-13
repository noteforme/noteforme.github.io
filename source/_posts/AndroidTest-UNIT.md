---
title: AndroidTest_UNIT
comments: true
date: 2019-11-17 20:49:14
tags: Test
categories: TEST
---



UnitTest

```
class EmailValidatorTest {

    @Test
    fun emailValidator_CorrectEmailSimple_ReturnsTrue(){
      assertThat(EmailValidator.isValidEmail("name@email.com")).isTrue()
      assertThat(EmailValidator.isValidEmail("nameail.com")).isFalse()
    }
}
```

