https://twitter.com/brandonphilips
github.com/philips

Brandon Phillips

OpenSourced a project called CoreOS DEX.
https://github.com/coreos/dex

Demo can be found at github.com/philips/hacks

https://speakerdeck.com/philips

Identity is the plumbing of the web.
When it goes wrong, it goes really wrong.
Because of the importance, no one wants to work on it.

Identity is really important, because otherwise amazon couldn't re-target ads to you [sic].

Goal:
- Web standard based auth for Kubernetes.
- Also needed to be able to manage identity for Tectonic (their first paid service).
- quay.io is a private docker repository. Needs identity for access control.

Why Dex?
Feel like it's really important to build projects as open source. Get a lot of new use cases for free. Get good testing on security. Get the power of a community.

OIDC was their identity protocol of choice.
This (Confusingly) has nothing to do with OAuth 2.0.

OIDC is really like OAUTH 2.0 with types.
OIDC has been adopted by Google, Amazon, etc. So no reason to fight that. The web has made it's choice.

Spec is at https://openid.net/connect

(Goes into a demo of how OAuth 2.0 works)
OIDC renames the entire OAuth workflow (ugh).

JOSE
Javascript object signing and encryption

JWK
Cryptographic key object

JWS
JSON web signature (similar to a public key signature)

Dex infrastructure:
- postgres
- dex-worker (horizontally scalable)
- dex-overlord (cycles the key)

Connectors:
- Local connector (email and pass auth)
- OIDC connector (Dex as a relaying party)
- Future connectors (SSH is something they'd like to build - you can login with your SSH public key)

Dex features:
- Automatic key rotation (Through the overlord)
- Horizontally scalable
- friendly API for managing users

Roadmap: (THEY WANT HELP WITH THIS!)
- grouping users into roles (for permission access control)
- More connectors
- Alternative storage backends
- U2F or Google Authenticator for 2FA
- More than one remote identity per users

(launches into a Tech demo)

They wrote their own go-oidc library to help make dex possible.

Q: Why build your own?
A: We wanted something that was really easy to deploy on top of Kubernetes. Plus, it was a lot of fun!
