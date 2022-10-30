---
title: TEST_UNIT
comments: true
date: 2019-11-17 20:49:14
tags: Test
categories: TEST
---



Test Driven Development

MainPrinciple:Write the test case before the implementation of the function (only for unit tests)

1. Write the function signature
2. Write the test cases for the function
3. Write the function logic so the tests pass



You  should only have one assertion per test case.



```kotlin
object RegistrationUtil {
    private val exitingUsers = listOf("Peter","Carl")

    /**
     * the input is not valid if...
     * ...the username /password is empty
     * ...the username is already taken
     * ...the confirmed password is not the same as the real password
     * ...the pasword contains less than 2 digits
     */
    fun validateRegistrationInput(
        userName: String,
        password: String,
        confirmedPassword: String
    ):Boolean{
        if (userName.isEmpty()||password.isEmpty()){
            return false
        }
        if (userName in exitingUsers){
            return false
        }
        if (password!=confirmedPassword){
            return false
        }
        if (password.count{it.isDigit()}<2){
            return false
        }
        return true
    }
}
```

RegistrationUtilTest.kt

```kotlin
import com.google.common.truth.Truth.assertThat
import org.junit.Test


class RegistrationUtilTest{

    @Test
    fun `empty username returns false`() {
        val result = RegistrationUtil.validateRegistrationInput(
            "",
            "123",
            "123"
        )
        assertThat(result).isFalse()
    }

    @Test
    fun `valid username and correctly repeated password returns true`() {
        val result = RegistrationUtil.validateRegistrationInput(
            "Philipp",
            "123",
            "123"
        )
        assertThat(result).isTrue()
    }

    @Test
    fun `username already exists returns false`() {
        val result = RegistrationUtil.validateRegistrationInput(
            "Carl",
            "123",
            "123"
        )
        assertThat(result).isFalse()
    }

    @Test
    fun `incorrectly confirmed password returns false`() {
        val result = RegistrationUtil.validateRegistrationInput(
            "Philipp",
            "123456",
            "abcdefg"
        )
        assertThat(result).isFalse()
    }

    @Test
    fun `empty password returns false`() {
        val result = RegistrationUtil.validateRegistrationInput(
            "Philipp",
            "",
            ""
        )
        assertThat(result).isFalse()
    }

    @Test
    fun `less than 2 digit password returns false`() {
        val result = RegistrationUtil.validateRegistrationInput(
            "Philipp",
            "abcdefg5",
            "abcdefg5"
        )
        assertThat(result).isFalse()
    }
}
```



https://www.youtube.com/watch?v=W0ag98EDhGc&list=PLQkwcJG4YTCSYJ13G4kVIJ10X5zisB2Lq&index=3



https://developer.android.com/kotlin/coroutines/coroutines-best-practices
