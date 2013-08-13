akka-kata
=========

##### The akka kata for Krakow Scala User Group

The purpose of this kata is to try out few basic, but probably most useful Akka mechanisms.  
We will build distributed system for decrypting hashed passwords.
Input data (encrypted text) will be delivered by the external, remote master server. During the processing some errors can happen, and they must be handled appropriately, ideally using Akka supervision and monitoring. Computation result (decrypted text) need to be send back to the master server. At the end, we should ensure that everything is working as expected by writing tests for our system.

##### Input data

Upon the request server will send us back list of hashes which our program should be able to decrypt:
```scala
type Hash = String
List[Hash]
```

##### Processing

We will provide jar file containing simple hash decrypting library. It will have method like:
```scala
def decrypt(hash: Hash): String
```
which takes hash and returns back decrypted text, if possible. In case of error this method can throw various exceptions.
We should handle them, and try to recompute. However, there are two special cases:
* UnableToDecryptException  
In this case we should just mark this value as impossible to decrypt.
* ContextCorruptionException  
This means, that something inside our library (like complicated cache) is broken, and the whole context need to be reset before
we can call decrypt method again. Be aware that it's some kind of global state and it can affect also computations done
using other threads. Method to reset global library context looks like this:

```scala
def reset() : Unit
```

##### Output data

We should return to the server list of tuples, each containing Hash and decrypted text.
If it was impossible to decrypt the hash, we should pair it with the empty string:
```scala
List[(Hash, String)]
```

##### Testing
Tbd.

##### Grand finale

Progress and statistics of all participants will be measured by the master server.
At the end of the kata we will compare results and look at different approaches to the problem.
Also during the whole kata we will be able to observe behavior of all clients using Typesafe Console:  
![Typesafe Console](http://resources.typesafe.com/docs/console/_images/console_first.png "Typesafe Console")

