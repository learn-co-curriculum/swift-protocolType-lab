# Protocols as Types, Delegation

![Drawing](http://i.imgur.com/SNKZqeU.jpg?1)  

> If you cannot do great things, do small things in a great way. -[Napoleon Hill](https://en.wikipedia.org/wiki/Napoleon_Hill) 

## Learning Objectives

* Utilize protocols as:
	* a parameter type or return type in a function or initializer
	* the type of a constant, variable, or property
	* the type of items in an array, dictionary, or other container
* Understand the delegation pattern
* Create protocols / delegates amongst classes and structs

## Outline / Notes

We know how to define a protocol. Here’s an example of a protocol with a single instance property requirement:

```swift
protocol FavoriteThings {
    var favColors: String { get }
    var favIcecream: String { get }
    var favSong: String { get }
}
```

The `FavoriteThings` protocol requires a conforming type to provide their favorite color, ice-cream and song. The protocol doesn't specify anything else about the nature of the conforming type-it only specifies that the type must be able to provide those three favorite things for itself.

Here's an example of a structure that adopts and conforms to the `FavoriteThings` protocol.

```swift
struct Person: FavoriteThings {
    var favColors: String
    var favIcecream: String
    var favSong: String
}

let jeff = Person(favColors: "Red", favIcecream: "Vanilla", favSong: "Foldgers In Your Cup")
```

Here's an example of a structure that adopts and conforms to the `FavoriteThings` protocol (again!):

```swift
struct Cat: FavoriteThings {
    var favColors: String
    var favIcecream: String
    var favSong: String
}

let frisky = Cat(favColors: "Blue", favIcecream: "Chocolate", favSong: "Meow Mix")
```

We now have a `Cat` struct and a `Person` struct that both conform to the `FavoriteThings` protocol. Even though protocols do not actually implement any functionality themselves, any protocol you create (like the `FavoriteThings` protocol here) will become a fully-fledged type for use in your code.

Because it is a type, you can use a protocol in many places where other types are allowed:
* (A) - As a parameter type or return type in a function or initializer.
* (B) - As the type of a constant, variable, or property
* (C) - As the type of items in an array, dictionary, or other container


**(A)** - As a parameter type or return type in a function or initializer

```swift
func favFlavorIsVanilla(x: FavoriteThings) -> Bool {
    return x.favIcecream == "Vanilla"
}
```

```swift
func loveItAll() -> FavoriteThings {
    let john = Person(favColors: "Brown", favIcecream: "Chocolate", favSong: "Blame Canada")
    
    return john
}
```




<p class='util--hide'>View <a href='https://learn.co/lessons/ProtocolsAsTypes'>Protocols as Types</a> on Learn.co and start learning to code for free.</p>