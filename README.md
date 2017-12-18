# PHP OOP Non if-else code styling

### TABLE OF CONTENT
- [Introduction](#introduction)
- [Principles](#principles)
    - [Principle 1: No conditions in OOP](#principle-1-no-conditions-in-oop)
    - [Principle 2: At least PHP 7.0](#principle-2-at-least-php-70)
    - [Principle 3: There are tree kinds of return](#principles-3-there-are-there-kinds-of-return)
    - [Principle 4: Parameters should have type declarations](#principles-4-parameters-should-have-type-declarations)
- [Level 0: stop using 'elseif' and 'switch' statements](#level-0-stop-using-elseif-and-switch-statments)
- [Level 1: stop using 'else' word](#level-1-stop-using-else-word)
- [Level 2: stop using 'if' word](#laravel-2-stop-using-if-word)
- [Level 3: stop using if shorthand](#level-3-stop-using-if-shorthand)
- [Level 4: Do not use any type of conditions!](#level-4-do-not-use-any-type-of-conditions)

## Introduction

This document contains examples of how to became a real PHP OOP ninja and stops to using conditional statements in your code. First read about principles. Then - when you are ready - proceed the secret techniques of unconditionally programming.

## Principles

There are few principles that you have to note before you begin your adventure with new kind of PHP code writing. So let's begin!

### Principle 1: No conditions in OOP

*There should NOT be any conditions in clear Object-Oriented Programing Logic Layer.*

The first and the most important principle. Each object should contains methods that are responsible for acting and not for making decisions. In the other words: The method does not depend on what it receives - it always tries to do what it is responsible for - regardless of the input parameter.

So we can say that object-oriented programming is uncompromising and:

1. It's clear!
2. It's testable!!
3. It's has readable code and programming interface!!!

```
    class Opener
    {
        public function open(Openable $openable): void
        {
            $openable->open();
        }
        
        public function close(Closeable $openable): void
        {
            $openable->close();
        }
    }

    class BagPacker
    {
        private $opener;
    
        public function __construct(Opener $opener)
        {
            $this->opener = $opener;
        }
        
        public function pack(Wardrobe $source Bag $target): void
        {
            $this->opener->open($source);
            $this->pack($target, $this->getStuff($source));
            $this->opener->close($source);
        }
                
        private function pack(Packable $packable, StuffStack $stuff = null): void
        {
            $stuff && $packable->pack($stuff);
        }
        
        private function getStuff(Stuffable $stuffContainer): ?StuffStack
        {
            return $stuffContainer->getContent();
        }
    }
```

So someone can ask: *But wait! What with making decicions in my application? How can it be automated!?*

As I've metioned in the first words - No conditions should be used in the Application or Busines Logic Layer. All the decicions should be make in the View, Controller (MVC) or Command Bus Layer (CQRS). And that's all what we should get.

### Principle 2: At least PHP 7.0

[PHP 7.0](http://php.net/manual/en/migration70.new-features.php) have most of, and [PHP 7.1](http://php.net/manual/en/migration71.new-features.php) have all solutions needed (to forget about the `if` statements and other conditional statements) such as:
- type hints and scalar type declaration
- return type declaration (and nullable types)
- null coalescing operator
- Throwable interface
- multi catch exception handling 
 
### Principle 3: There are tree kinds of return

We can distinguish three kinds of return values:
- "The type of desire" - this is the type that is declared as a return type of the method. In some circumstances it can became a void type of `null`.
- Void values - which mostly represents command bus methods.
- Exception - all the variety of the exceptions and errors (everything that implements `Throwable` interface)

### Principle 4: Parameters should have type declarations

All the method's parameters should have type declaration. And that's all for now. You will see later why type hints are so important.

## Level 0: stop using 'elseif' and 'switch' statements

First step is to drop on using `elseif` and `switch` statements. I belive that it's easier than you think.

You can easly change this:

```
    function getFooBar($someVar)
    {
        $result = $someVar;
        if ($someVar > 1) {
            $result = 'foo';
        } elseif ($someVar < 0)
            $result = 'bar';
        }
        return $result;
    }
```

to this:

```
    function getFooBar(int $someVar): string
    {
        if ($someVar > 1) {
            return 'foo';
        } 
        if ($someVar < 0)
            return 'bar';
        }
        return (string) $someVar;
    }
```

It's more clear and we gain one assignment operation less.

So what about switches? 

```
    function getWeekDay($day)
    {
        $dayOfWeek = '';
        switch ($day) {
            case 0: $dayOfWeek = 'It's Sunday!';
                break;
            case 1: $dayOfWeek = 'It's first day of a week';
                break;
            case 2: $dayOfWeek = 'It's second day of a week';
                break;
            // etc.
        }
        return $dayOfWeek;
    }
```

How do we can change them? We cn go and change it exacely like the example above or we can try to map functions like that:

```
    public function getWeekDay(int $day): string
    {
        $functionName = sprintf('get%s', date('l'));

        if (method_exists($this, $functionName)) {
            return $this->{$functionName}();
        }

        throw new \Exception(sprintf('There is no week day with index: %s', $day));
    }

    private function getSunday(): string
    {
        return 'It's first day of a week';
    }

    private function getMonday(): string
    {
        return 'It's first day of a week';
    }
```


## Level 1: stop using 'else' word

## Level 2: stop using 'if' word

## Level 3: stop using if shorthand

## Level 4: Do not use any type of conditions!
