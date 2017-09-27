## How does Akka work?


* Let's take a look at a simple Actor

```Scala
class Guestbook extends Actor {
  var entries: List[Entry] = List()

  override def receive: Receive = {
    case GetAll => sender ! entries
    case AddEntry(entry) => entries = entries :+ entry
  }
}
```

<li class="fragment">Don't fear no mutable state, let Akka handle this for you!</li>


* The base of everything is the `ActorSystem`
<li class="fragment">It manages its own threads</li>
<li class="fragment">Every `Actor` has its own inbox</li>
<li class="fragment">Messages that are sent to an actor are stored</li>
<li class="fragment">A dispatcher provides threads to take a message from an inbox and execute the actor code</li>
<li class="fragment">The guarantee is, that no two threads will execute an `Actor` concurrently</li>


Simply put

![akka_dispatcher](/img/Akka_Dispatcher.png "Logo Title Text 1")


* An important concept in Akka is the `ActorRef`
<li class="fragment">One never receives a reference to the actual `Actor`</li>
<li class="fragment">That ways, the object behind the `ActorRef` can change or be replaced</li>
<li class="fragment">E.g. when the `Actor` crashed and is being restarted</li>
<li class="fragment">Additionally, an `ActorRef` hides where the `Actor` is located</li>


The full picture

![full](/img/44.jpg)

<div style="font-size:10px">https://jraviton.files.wordpress.com/2015/01/44.jpg Saeid Siavashi 2015</div>
