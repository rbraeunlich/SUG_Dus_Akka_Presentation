## What is Akka?


* Akka is

>a set of open-source libraries for designing scalable, resilient systems that span processor cores and networks.

Akka documentation


* Akka's core is based on the Actor Model
<li class="fragment">This shall allow high concurrency while easing the developers burden</li>
<li class="fragment">Additionally, actor monitoring follows the "Let it crash" approach</li>
<li class="fragment">So, what is the Actor Model?</li>


## Actor Model  


* First mentioned 1973 in the paper "A Universal Modular ACTOR Formalism for Artificial Intelligence" by Hewitt, Bishop and Steiger
<li class="fragment">They described an architecture for AI which supports a high degree of parallelism</li>
<li class="fragment">The architecture is based on a single object: the Actor</li>


<li class="fragment">Actors encapsulate control flow and data</li>
<li class="fragment">Actors can only communicate by exchanging messages</li>
<li class="fragment">A scheduler controls the execution of Actors</li>
