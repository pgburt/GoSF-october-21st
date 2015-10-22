David Calavera
https://twitter.com/calavera
david.calavera@gmail.com

It's all about Docker and how to extend it.
Docker is nice because it's isolated.

When you start using more complicated applications, you need to deal with a lot of different issues.

One of the big issues is managing passwords and secrets.
Maybe your app needs to connect to a database?
Or auth with AWS?

12Factor app recommends writing sensitive info as ENV variables.
Cool, good for somethings. This works well with Docker already.
Problem: passwords are still in plaintext.

Docker will be adding first class support for volumes in an upcoming release. This will allow Docker to have a more persistent kind of storage.

When you create a volume, you will have access to have something called "drivers".
Drivers are pluggable, and can be swapped. There are plugins for Drivers.

"Write a volume plugin" (shown as a page)

HashiCorp has a project called Vault.
It gives strong guarantees for protecting passwords and keys against thieves.
AND! The API is really simple.

https://www.vaultproject.io/

It's great that it's HTTP! But that still means you need a javscript client, mobile clients, etc etc.

Solution: There is an API that every single language implements that is already pretty safe. What is it? The FileSystem API!

What if we could create some kind of a FileSystem to manage the HTTP madness of REST?

(Launches into an improvised demo)
Fires up a vault server locally.
Performs a helloWorld example of storing a secret and retrieving.

You could actually implement Vault as a FileSystem API, and retrieve secrets.

The volume plugin is just a Go Binary.
Next he creates a volume named vault
> docker volume ______ (some commands)

> docker run --volume vault:/data -it alpine ash
> cat /data/secret/password
The output is the password!

The nice part here is he didn't have to actually install any HTTP client! This means using vault in conjunction with Docker can be completely language agnostic.

Write filesystems in Go using FUSE.

https://github.com/calavera/docker-volume-vault

## QnA
Q: What happens if you shut down the server?
A: If you try to access any file, it fails. The plugin disables any cache that the file system has. So, there is no cache.

Shutting down and requesting a file will say that the file does not exist. Docker would also fail to fire up new instances because it would be unable to connect to the FileSystem.

Q: How is this better than an Environment var?
A: The nice part is you don't really ever have the secrets anywhere on the server. It's all inside the container. Only the container can see it.

Q: What if you have different containers with different levels of privs?
A: Vault has a concept of policies. So you can specify what a token has access to.
Docker volume create -opt stuff-goes-here


Q: How is this better than ENV variables?
A: Again, what was said before, policies! It is easier to revoke a token this way, and it's also easily auditable.
