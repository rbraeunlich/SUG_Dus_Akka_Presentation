## How?


* The base for everything is the `ActorSystem`.

* It manages its own threads.


<img src="img/mailbox.jpg" width="200">

* Every `Actor` has its own inbox.

* Messages that are sent to an actor are stored there.

* Idle threads take a single message from an inbox and execute the actor code.
