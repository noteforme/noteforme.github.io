---
id: 272
title: 'Ready Interview this time'
date: '2024-05-30T02:03:52+00:00'
author: Jon
layout: post
guid: 'https://jonblog.site/?p=272'
permalink: /2024/05/30/ready-interview-this-time/
categories:
    - INTERVIEW
---



# Archtechtrue

## Android MVP MVVM,现在组件演进成MVVM，最主要的原因是什么。

数据绑定（Data Binding）：

MVVM 通过数据绑定库（如 Android 的 Data Binding Library 和 Jetpack Compose）实现了视图和数据之间的直接绑定，减少了样板代码，使得代码更加简洁和易于维护。
解耦性：

MVVM 模式通过 ViewModel 将视图逻辑与业务逻辑分离，使得视图和模型之间的耦合度更低。这使得代码更易于测试和复用。
响应式编程：

MVVM 借助 LiveData 或 RxJava 等响应式编程工具，使得视图能够自动观察数据变化并进行更新，从而简化了更新 UI 的过程。
生命周期感知：

MVVM 中的 ViewModel 是生命周期感知的，这意味着它们在配置更改（如设备旋转）时不会被销毁，避免了内存泄漏和不必要的资源重建问题。
适应现代架构组件：

MVVM 更加适合与现代 Android 架构组件（如 Room、ViewModel、LiveData 等）一起使用，这些组件设计之初就是为了与 MVVM 模式无缝集成，提供更好的开发体验。
这些优势使得 MVVM 成为 Android 开发中越来越流行的架构选择，帮助开发者更高效地构建和维护应用。