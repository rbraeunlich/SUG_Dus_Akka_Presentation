## Testing Akka


* Basic testing of actors is surprisingly easy
<li class="fragment">Messages define the public API of an actor</li>
<li class="fragment">We can evaluate the responses we receive</li>
<li class="fragment">Akka brings its own testkit to help</li>
<li class="fragment">Although discouraged, even the internal state of an Actor can be inspected</li>


* Testing `AddEntry`
```Scala
"save entries" in {
    val sender = TestProbe()
    implicit val senderRef = sender.ref
    val guestbook = actorSystem.actorOf(
      Props(new Guestbook("1")))
    val newEntry = Entry("author",
    "This is the best guestbook eva! <3")

    guestbook ! AddEntry(newEntry)  

    guestbook ! GetAll
    sender.expectMsg(List(newEntry))
}
```
<li class="fragment">`TestProbe` makes it possible to expect certain messages</li>


* Testing `GetAll`
```Scala
"return all entries" in {
    val sender = TestProbe()
    implicit val senderRef = sender.ref
    val guestbook = actorSystem.actorOf(
      Props(new Guestbook("2")))
    val newEntry = Entry("author1", "text1")
    guestbook ! AddEntry(newEntry)
    guestbook ! AddEntry(newEntry)

    guestbook ! GetAll

    sender.expectMsg(List(newEntry, newEntry))
}
```
<li class="fragment">Akka is so cool!</li>
<li class="fragment">I wish I could use it in the frontend, too!</li>
