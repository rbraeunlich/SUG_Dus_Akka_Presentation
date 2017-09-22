## Akka HTTP


>The Akka HTTP modules implement a full server- and client-side HTTP stack on top of akka-actor and akka-stream.
Akka HTTP documentation
<div class="fragment">That doesn't sound very magically...</div>


Defining a `Route`
```Scala
val route = path("all") {
  get {
    val allEntries: Future[ArrayBuffer[Entry]] = (guestbook ? GetAll).mapTo[ArrayBuffer[Entry]]
    complete(allEntries)
  }
} ~
  path("addEntry") {
    put {
      entity(as[Entry]) { entry =>
        guestbook ! AddEntry(entry)
        complete("created")
      }
    }
  } ~
path("index") {
  getFromResource("index.html")
}
```
