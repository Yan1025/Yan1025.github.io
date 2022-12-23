---
title: "What the different of abstract between Go and Java?"
date: 2022-12-23T18:58:11+08:00
draft: false
---

## Conclusion
The conclusion given at the first: Java take "extends" while Go takes combination to implements abstrct.

## Interface Implement
Java will explicitly declares implement relation by useing "implement" syntax.
As a comparison, Go makes a rule that the "struct" will automatically implements a interface as long as it implements 
all of the interface's methods.
For instance, if we want "Student"(class) implements "Eatable"(interface), we could do as following👇:

```java
public interface Eatable {
    void before();

    void eating();

    void after();
}

public class Student implements Eatable {
    @java.lang.Override
    public void before() {
        // ......
    }

    @java.lang.Override
    public void eating() {
        // ......
    }

    @java.lang.Override
    public void after() {
        // ......
    }
}
```
While we do in the Go as the following👇:
```go
type Eatable interface {
	Before()
	Eating()
	After()
}

type Person struct{}

func (Person) Before() {
	// ......
}

func (Person) Eating() {
	// ......
}

func (Person) After() {
	// ......
}
```
## Extend or composite
Having ability to reuse code section is The key different between script language and the seriously programming language, 
while there are two solutions called extend and composite.

Still take Java and Go as an example, Java usually takes extend while Go takes composite.

For instance, we would like to make "Man" reuse the ability of "Person", this is the version of Java👇:
```java
public class Person {
    public void breath() {
        // ......
    }
}

public class Man extends Person {
}
```
this is the version of Go👇:
```go
type Person struct {
}

func (p *Person) breath() {
	// ......
}

type Man struct {
	P Person
}
```

As unsupport multiple extend, if you want to reuse more than one class, you can only use multi-layer extend or make the 
super classes being fields of subclass while the former **decrease the readability** and the latter **can't indicates the correct
relationship of super classes and subclass**.
## The shortcoming of Go
It must be admitted that it's very simplified of implementing interfaces in Go. But at the opposite, it will be more and
more difficult to figure out how much and which interfaces does a type implements when the project becoming huger and huger.

Maybe that's one of the reasons why employers, even like tiktok and bilibili who takes Go as their main development language,
takes Java rather than Go as the main language to develop big project with very complex business logic such as online trading system.
