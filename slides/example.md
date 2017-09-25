## Example


* We already know this one:

```Scala
class Guestbook extends Actor {
  val entries: ArrayBuffer[Entry] = ArrayBuffer()

  override def receive: Receive = {
    case GetAll => sender ! entries
    case AddEntry(entry) => entries += entry
  }
}

object Guestbook {
  val props = Props[Guestbook]
  case object GetAll
  case class AddEntry(entry: Entry)
}
```


* Let's add a main method

```Scala
object Main {

  implicit val timeout: Timeout = 5.seconds

  def main(args: Array[String]): Unit = {
    implicit val system = ActorSystem("akka-kadabra")
    implicit val guestbook = system.actorOf(Guestbook.props)
}
```
<li class="fragment">Hm... that's boring and it doesn't do anything...</li>
<li class="fragment">Oh! I know! Let's make it a web application</li>
