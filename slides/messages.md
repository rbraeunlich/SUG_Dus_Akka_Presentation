## Messages


* Again, let's take a look at some example messages

```Scala
object Guestbook {

  case object GetAll

  case class AddEntry(entry: Entry)

}
```


* Basically, everything can be a message
<li class="fragment">The method signature of `Receive` is `Any => Unit` (Akka Typed will improve this!)</li>
<li class="fragment">Best practice is to use case classes and objects</li>
<li class="fragment">There are also messages included in Akka like `Terminated`, `PoisonPill`, `ReceiveTimeout`</li>


<ul><li>Messages can be exchanged in two ways:</li>
  <ul>
    <li>! (tell)</li>
    <li>? (ask)</li>
  </ul>
<li class="fragment">`tell` is the fire-and-forget way to send a message</li>
<li class="fragment">It doesn't expect any answer</li>
<li class="fragment">It is highly encouraged to use it</li>


* `ask` expects an answer and therfore returns a `Future`
<li class="fragment">`ask` has to keep track of the timeout and therefore has a lower performance</li>
<li class="fragment">Lastly, you should <b>never</b> wait for a `Future` inside an `Actor`</li>
