---
layout: post
title: "PHP anonymous classes use case: testing abstract classes"
---

I recently wanted to test an abstract class that already came with some written out functions. 

I ended up with a setup where I had an anonymous class extend the abstract class. 
```php
$dependency = new Dependency();
$myClass = new class($dependency) extends AbstractClass {};
```
As you see, you can also pass in necessary dependencies. Cool, isn't it!?

[Support for anonymous classes was added in PHP 7.](https://www.php.net/manual/en/language.oop5.anonymous.php)
