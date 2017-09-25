## AkkaJS



![jsatt](/img/jsatt.jpg "Logo Title Text 1")


* Now comes the black magic
<li class="fragment">Based on ScalaJS, there is AkkaJS</li>
<li class="fragment">This makes it possible to write Actors and have them compiled to JS</li>
<li class="fragment">Let's see how this works</li>


* Create a new guestbook entry

```Scala
class EntrySender extends Actor with ActorLogging {
  override def receive: Receive = {
    case entry:NewEntry =>
      log.info("Sending guestbook entry")
      val request = new XMLHttpRequest()
      request.open("PUT", "http://localhost:8080/addEntry", async = false)
      request.setRequestHeader("Content-Type", "application/json")
      request.send(entry.asJson.toString())
  }
}
object EntrySender {
  case class NewEntry(author: String, text: String)
}
```


* Add it to a button

```Scala
jQuery("<button type=\"button\" id=\"submit-button\">Submit!</button>")
  .click(() => {
    val author = jQuery("#author").value().toString
    val text = jQuery("#text").value().toString
    entrySender ! NewEntry(author, text)
    entryRenderer ! RetrieveEntries
  }).appendTo(inputDiv)
```


* ScalaJS needs a main method, too

```Scala
object Main {
  val actorSystem = ActorSystem("akka-kadabra-client")

  private val entryRenderer = actorSystem.actorOf(EntryRenderer.props)
  private val entrySender = actorSystem.actorOf(EntrySender.props)

  @JSExport
  def main(args: Array[String]): Unit = {
    jQuery("body").append("<h1>✨Akka-kadabra✨</h1>")
    renderInputElements()
    val retrieveDiv = jQuery("<div>").appendTo(jQuery("body"))
    jQuery("<button type=\"button\" id=\"retrieve-button\">Get guestbook entries</button>")
      .click(() => {
        entryRenderer ! RetrieveEntries
      }).appendTo(retrieveDiv)
    jQuery("body").append("<br>")
  }
  ...
}
```
