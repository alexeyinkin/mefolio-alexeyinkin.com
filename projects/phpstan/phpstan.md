PHPStan is the most popular static analyzer for PHP.
I made a significant improvement to it and donated the code.

The problem:

Method arguments can be annotated with PHPDoc comments
to provide type info that cannot be represented in the code itself,
e.g. array types. However, PHPStan never supported inheritance
for PHPDoc comments meaning that each overridden method had to repeat
all arguments' types in case it expanded the type of one particular
argument or narrowed down the return type.

I [implemented the functionality for PHPDoc inheritance](https://github.com/phpstan/phpstan-src/pull/196).

Before:

```php
class Foo {
    /**
     * @param DateTimeInterface[] $arr
     * @return DateTime[]
     */
    public function method(array $arr): array { /* ... */ }
}

class Bar extends Foo {
    /**
     * @param DateTime[] $arr Expanded argument type.
     * !!! Return type is lost to PHPStan if not repeated !!!
     */
    public function method(array $arr): array { /* ... */ }
}

class Baz extends Bar {
    /**
     * !!! $arr type is lost to PHPStan if not repeated !!!
     * @return DateTimeInterface Narrowed down return type.
     */
    public function method(array $arr): array { /* ... */ }
}
```

This was extremely inconvenient given that most IDEs did handle this inheritance,
and programmers were reluctant to duplicate types for static analysis alone.
This prevented the adoption of PHPStan in [Calltouch](https://calltouch.ru),
a company I had been working at the time.

After my fix, the new PHPStan was integrated to the CI pipeline improving code quality
in my the company and in many others.

PHPStan has 11k stars on GitHub and is the most popular project I contributed code to
as of writing.

