## How does Akka work?


* Let's take a look at a simple Actor

```Scala
class Guestbook extends Actor {
  val entries: ArrayBuffer[Entry] = ArrayBuffer()

  override def receive: Receive = {
    case GetAll => sender ! entries
    case AddEntry(entry) => entries += entry
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
