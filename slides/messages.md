## Messages


* Again, let's take a look at some example messages

```
object Guestbook {

  case object GetAll

  case class AddEntry(entry: Entry)

}
```


* Basically, everything can be a message

<li class="fragment">The method signature of `Receive` is `Any => Unit` (Akka Typed will change this!)</li>

<li class="fragment">Best practice is to use case classes and objects</li>

<li class="fragment">There are also messages included in Akka like `Terminated`, `PoisonPill`, `ReceiveTimeout`</li>
