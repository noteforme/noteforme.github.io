---
id: 501
title: 'Design Pattern Strategy'
date: '2024-06-18T03:24:51+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=501'
permalink: /2024/06/18/design-pattern-strategy/
categories:
    - DesignPattern
---



https://refactoring.guru/design-patterns/strategy

https://design-patterns.readthedocs.io/zh-cn/latest/behavioral_patterns/strategy.html

# Class UML

![](https://refactoring.guru/images/patterns/diagrams/strategy/structure.png)

behavioral design patterns

The Strategy design pattern is one of the behavioral design patterns used to define a family of algorithms, encapsulate each one, and make them interchangeable. The strategy pattern lets the algorithm vary independently from the clients that use it. This pattern is particularly useful for situations where a class needs to perform a specific behavior or service in different ways based on the context or data it is working with.

# Key Concepts

1. Context: The class that uses a Strategy. It is configured with a Strategy object and maintains a reference to the current Strategy object. It may define an interface for setting the strategy.

2. Strategy: The interface that is common to all concrete strategies. This interface is used by the context to call the strategy defined algorithms.

3. Concrete Strategies: Implementations of the Strategy interface. Each concrete strategy implements the algorithm using a different approach.

# Example Scenario

Imagine you have a Payment class that can process payments in different ways (e.g., credit card, PayPal, Bitcoin). Using the Strategy pattern, you can define a common interface for all payment methods and create separate classes for each payment method.

```kotlin
// Strategy interface
interface PaymentStrategy {
    fun pay(amount: Double)
}

// Concrete Strategy for Credit Card payment
class CreditCardPayment(private val cardNumber: String) : PaymentStrategy {
    override fun pay(amount: Double) {
        println("Paying $amount using credit card $cardNumber")
    }
}

// Concrete Strategy for PayPal payment
class PayPalPayment(private val email: String) : PaymentStrategy {
    override fun pay(amount: Double) {
        println("Paying $amount using PayPal account $email")
    }
}

// Context class
class Payment(private var strategy: PaymentStrategy) { // 对应Context
    fun setStrategy(strategy: PaymentStrategy) {
        this.strategy = strategy
    }

    fun processPayment(amount: Double) {
        strategy.pay(amount)
    }
}

// Usage
fun main() {
    // Using Credit Card payment strategy
    val creditCardPayment = CreditCardPayment("1234-5678-9876-5432")
    val payment = Payment(creditCardPayment)
    payment.processPayment(100.0)

    // Changing strategy to PayPal
    val paypalPayment = PayPalPayment("user@example.com")
    payment.setStrategy(paypalPayment)
    payment.processPayment(200.0)
}
```