## Akka Persistence


* Akka Persistence is based on event sourcing
* All events are stored and replayed upon restart of the actor
* It is very important to give an Actor a `persistenceId`

| Sequence | Event       |
|----------|-------------|
| 1        | add 1       |
| 2        | substract 2 |
| ...      | multiply 3  |


* Two methods have to be implemented
<li class="fragment">First one is `receiveCommand`</li>
<li class="fragment">It basically does the same as `receive`; it only includes the call to `persist`</li>
<li class="fragment">Optionally, snapshots can be saved</li>


```Scala
def updateState(add: AddEntry): Unit = entries += add.entry

override def receiveCommand: Receive = {
  case GetAll => sender ! entries

  case add: AddEntry =>
    persist(add) { event =>
      log.info(s"Applying event $event. State: $entries")
      updateState(event)
      if (entries.nonEmpty && entries.length % 100 == 0) {
        saveSnapshot(entries)
      }
    }
}
```


* The second method is `receiveRecover`
<li class="fragment">As the name suggests, this one is used for recovery</li>
<li class="fragment">It either receives one of the saved events or a snaphot</li>


```Scala
override def receiveRecover: Receive = {
    case add: AddEntry =>
      log.info(s"Recovered event $add")
      updateState(add)
    case SnapshotOffer(_, snapshot: ArrayBuffer[Entry]) =>
      log.info(s"Restored snapshot $snapshot")
      entries ++= snapshot
  }
```


The persistent Guestbook

```Scala
class Guestbook(id: String) extends PersistentActor with ActorLogging {

  override def persistenceId: String = id

  val entries: ArrayBuffer[Entry] = ArrayBuffer()

  def updateState(add: AddEntry): Unit = entries += add.entry

  override def receiveRecover: Receive = {
    case add: AddEntry =>
      log.info(s"Recovered event $add")
      updateState(add)
    case SnapshotOffer(_, snapshot: ArrayBuffer[Entry]) =>
      log.info(s"Restored snapshot $snapshot")
      entries ++= snapshot
  }

  override def receiveCommand: Receive = {
    case GetAll => sender ! entries

    case add: AddEntry =>
      persist(add) { event =>
        log.info(s"Applying event $event. State: $entries")
        updateState(event)
        if (entries.nonEmpty && entries.length % 100 == 0) {
          saveSnapshot(entries)
        }
      }
  }
}
```
