# Protocols as Types and Delegation

![Drawing](http://i.imgur.com/SNKZqeU.jpg?1)  

> If you cannot do great things, do small things in a great way. -[Napoleon Hill](https://en.wikipedia.org/wiki/Napoleon_Hill) 

## Overview 

In this lesson. we'll work with procols and delegates. 

## Learning Objectives

* List and explain the various places where you can utilize protocols 
* Explain the delegation pattern
* Create protocols and delegates amongst classes and structs

## Protocols

We know how to define a protocol. Here’s an example of a protocol with a single instance property requirement:

```swift
protocol FavoriteThings {
    var favColor: String { get }
    var favIcecream: String { get }
    var favSong: String { get }
}
```

The `FavoriteThings` protocol requires a conforming type to provide their favorite color, ice-cream and song. The protocol doesn't specify anything else about the nature of the conforming type-it only specifies that the type must be able to provide those three favorite things for itself.

Here's an example of a structure that adopts and conforms to the `FavoriteThings` protocol.

```swift
struct Person: FavoriteThings {
    var favColor: String
    var favIcecream: String
    var favSong: String
}

let jeff = Person(favColor: "Red", favIcecream: "Vanilla", favSong: "Foldgers In Your Cup")
```

Here's an example of a structure that adopts and conforms to the `FavoriteThings` protocol (again!):

```swift
struct Cat: FavoriteThings {
    var favColor: String
    var favIcecream: String
    var favSong: String
}

let frisky = Cat(favColor: "Blue", favIcecream: "Chocolate", favSong: "Meow Mix")
```

We now have a `Cat` struct and a `Person` struct that both conform to the `FavoriteThings` protocol. Even though protocols do not actually implement any functionality themselves, any protocol you create (like the `FavoriteThings` protocol here) will become a fully-fledged type for use in your code.

Because it is a type, you can use a protocol in many places where other types are allowed:
* As a parameter type or return type in a function or initializer.
* As the type of a constant, variable, or property
* As the type of items in an array, dictionary, or other container


Let's create a function we're familiar with.

```swift
func sayHelloTo(_ person: String) {
    print("Hello \(person)")
}

sayHelloTo("George")
// prints "Hello George"
```

`person` here is the name of the argument of type `String`. So the type of this argument is `String`. There's nothing stopping us from using our protocols as types here in arguments to a function, so lets do that.

```swift
func printYourFavThings(_ entity: FavoriteThings) {
     
}
```

We created a function called `printYourFavThings(_:)` that takes in one argument called `entity` of type `FavoriteThings`. In the scope of this function, we can _only_ access the properties and functions available to `FavoriteThings`. Those are `favColor`, `favIcecream` and `favSong`.  If I type `entity` out and a period to see what I have access to, it will only be these three items.

Lets finish the implementation:

```swift
func printYourFavThings(entity: FavoriteThings) {
    print("My favorite color is \(entity.favColor)")
    print("Icecream is great, my fav is \(entity.favIcecream)")
    print("The song I listen to the most is \(entity.favSong)")
}
```

So lets now call on this function passing in something that conforms to the `FavoriteThings` protocol. We know that any instance of a `Cat` or `Person` conforms to `FavoriteThings`. So lets pass in our `frisky` instance in.

```swift
printYourFavThings(frisky)
// My favorite color is Blue
// Icecream is great, my fav is Chocolate
// The song I listen to the most is Meow Mix
```

We're calling on this newly made function, `printYourFavThings(_:)`, passing in something that conforms to the `FavoriteThings` protocol (`frisky` does). When we want to print `entity.favColor` we know that this code will run because anything passed into this function conforms to this protocol, which means it _must_ have this property.

What about storing protocol types in an array?

```swift
let favorites: [FavoriteThings] = [frisky, jeff]
```

Here's an example of constant called `favorites` of type [`FavoriteThings`]. It is then assigned a value, which is an array that includes `frisky`, which is a `Cat`, and `jeff`, which is a `Person`. Even though `Cat` & `Person` are different types, they both conform to `FavoriteThings` and since Protocols themselves are types we are allowed to store both of these instances in an array like this.

Lets loop over the array and print out some items.

```swift
for element in favorites {
    printYourFavThings(element)
}

// My favorite color is Blue
// Icecream is great, my fav is Chocolate
// The song I listen to the most is Meow Mix

// My favorite color is Red
// Icecream is great, my fav is Vanilla
// The song I listen to the most is Foldgers In Your Cup
```

The first time through this for loop, `element` is `frisky`. Within the scope of this for loop, we don't know that `element` is a `Cat`, we just know that it's a `FavoriteThings`, which means we only have access to the functions/properties available to a `FavoriteThings` type, not a `Cat`. The second time through the for loop, `entity` is now `jeff`, which is a `Person`. Similarly, we only have access to the properties available on `FavoriteThings`.

# Delegation

Delegation is a design pattern that enables a class or structure to hand off (or delegate) some of its responsibilities to an instance of another type. What does that mean?

We're going to go into a story about a child and his mother.

![](https://media.giphy.com/media/rrnrVJ6SLR6Xm/giphy.gif)

Lets paint the picture.

```swift
class Baby {
    var name: String
    
init(name: String) {
        self.name = name
    }
}

let jim = Baby(name: "Jim")
```

```swift
class Mom {
    var name: String
    
    init(name: String) {
        self.name = name
    }
}

let maryann = Mom(name: "Maryann")
```

We have two types here. `Baby` and `Mom`. Neither perform any functionality right now, they both have an instance property called `name` of type `String`. 

Let's add some functionality to the `Mom` structure that will allow any instance of `Mom` to be able to feed a baby.

```swift
class Mom {
    var name: String
    
    init(name: String) {
        self.name = name
    }
    
    func feedBaby() {
        // feed the baby some food
    }
}
```

Now we want to add some functionality to the `Baby` structure that will scream up to any `Mom` instance passed into the function we are about to create.

```swift
class Baby {
    var name: String
    
    init(name: String) {
        self.name = name
    }
    
    func wantsFoodFrom(mom mother: Mom) {
        mother.feedBaby()
    }
}
```

If we wanted to call on this function, we would do so like this:

```swift
jim.wantsFoodFrom(mom: maryann)
```

This instance method on any `Baby` can be called. When it is called, you must pass in an instance of `Mom`. Here we're passing in `maryann`, which is an instance of `Mom`. 


So `jim` calls on this method, passing in `maryann` and all is right in the world.

But what if `maryann` needs to run to the store and tells her sister to watch over him. Well this function we created on `Baby` can only take in a `Mom`. Lets fix that using delegation!

```swift
protocol ParentDelegate {
    func feedBaby()
}
```

We've created a protocol called `ParentDelegate` that requires anyone conforming to it to implement the `feedBaby` function. We just learned that protocols themselves are types, so lets add a new property to the `Baby` class we created above.

```swift
class Baby {
    var name: String
    var delegate: ParentDelegate?
    
    init(name: String) {
        self.name = name
    }
}
```

There's now a property called `delegate` of type `ParentDelegate?`. It's an optional `ParentDelegate`, which means its default value is `nil`. We don't need to add onto the initializer because this new property has a default value of `nil` because it's an optional. 

So instead of having a function take in a `Mom` as an argument, we can do something like this:

```swift
class Baby {
    var name: String
    var delegate: ParentDelegate?
    
    init(name: String) {
        self.name = name
    }
    
    func wantsFood() {
        delegate?.feedBaby()
    }
}
```

This `wantsFood()` function can be called on by any instance of a `Baby`. It will then (using optional chaining) attempt to call on a function on the instance property named `delegate`, which is of type `ParentDelegate?`. If it's _not_ nil, then it will call on the `feedBaby()` function on that instance, which was assigned to the `delegate` property. If it is nil (in that nothing has been set to equal the `delegate` property, then the `feedBaby()` method will not get called.

But this behavior is exactly what we want. The baby doesn't care who the `delegate` is. It could be anything. As long as someone is around to be able to respond to feeding the baby, the baby is fine. 

If no-one is around to feed the baby (in that the `delegate` property was never assigned a value), then the baby will be screaming up to nothing and will get no response back.

Lets make Mom adopt and conform to this new protocol:

```swift
class Mom: ParentDelegate {
    var name: String
    
    init(name: String) {
        self.name = name
    }
    
    func feedBaby() {
        // feed the baby some food
    }
}
```

We need to no establish the connection. We know that the `delegate` instance property on `jim` is nil (by default). Let's fix that.

```swift
jim.delegate = maryann
```

This line of code will run. `maryann` is an instance of `Mom` and the `Mom` class adopts and conforms to the `ParentDelegate` protocol, so we are OK here. If we were to try to assign a value to this `delegate` property on `jim` to something that didn't adopt/conform to the protocol, we would be met with an error:

![](http://i.imgur.com/YJICPts.png)

Now, the only thing the baby needs to do is scream that he wants food.

```swift
jim.wantsFood()
```

When this method runs, because `maryann` has been assigned to equal the `delegate` property on `jim`. Because `maryann` is an instance of a `Mom`, we will jump to `Mom`'s implementation of this method.

But going back to what we stated earlier, what if `maryann` runs to the store. Who will watch over the baby boy? Let's create an Aunt class.

```swift
class Aunt: ParentDelegate {
    var name: String
    
    init(name: String) {
        self.name = name
    }
    
    func feedBaby() {
        print("Give the baby PIZZA!!!!!")
    }
}

let patty = Aunt(name: "Patty")
```

Here, this new class called `Aunt` adopts and conforms to the `ParentDelegate` protocol. It's implementation of `feedBaby()` is slightly different, and that's OK. Jim's aunt is awesome.

So now if we want to change the `delegate` property on `jim`, we do so like this.

```swift
jim.delegate = patty
```

If `jim` now screams up that he wants food, we get a message that prints to the console.

```swift
jim.wantsFood()
// prints "Give the baby PIZZA!!!!!"
```

This is an example of handing off responsibilities of one class to another. At the end of the day, it boils down to a form of communication between two classes or structs. It's a way for one instance to communicate to another instance. But not only communicating, it's a way for one class or structure to hand over some of its responsibilities.




<p class='util--hide'>View <a href='https://learn.co/lessons/ProtocolsAsTypes'>Protocols as Types</a> on Learn.co and start learning to code for free.</p>

<p class='util--hide'>View <a href='https://learn.co/lessons/swift-protocolType-lab'>Protocols as Types and Delegation</a> on Learn.co and start learning to code for free.</p>
