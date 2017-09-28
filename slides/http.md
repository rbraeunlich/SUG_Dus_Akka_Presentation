## Akka HTTP


>The Akka HTTP modules implement a full server- and client-side HTTP stack on top of akka-actor and akka-stream.

Akka HTTP documentation
<li class="fragment">That doesn't sound very magical...</li>


<div>1. Defining a `Route`</div>

```Scala
val route = path("all") {
  get {
    val allEntries: Future[List[Entry]] = (guestbook ? GetAll).mapTo[List[Entry]]
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
<div class="fragment">Hint: a little bit serialization magic is included here based on Akka HTTP Circe</div>


<div>2. Starting the webserver</div>

```Scala
val bindingFuture = Http().bindAndHandle(
  route,
  "localhost",
  8080)
    println(s"Server online at http://localhost:8080/")
    StdIn.readLine() // let it run until user presses return
    bindingFuture
      .flatMap(_.unbind()) // unbinding from the port
      .onComplete(_ ⇒ system.terminate()) // shutdown when done
```
<ul>
  <li class="fragment">Cool!</li>
  <li class="fragment">But after a restart all my guestbook entries are gone.</li>
</ul>
