---
layout: post
title: "Facebook and Public Key Encryption"
date: 2016-03-23
categories: essay
---
Considering all of the recent Apple news and a resurgence of the 90s ‘cryptowars’  I thought it’d be interesting to share an idea I’ve been thinking about.

While public key encryption enabled e-commerce, it never succeeded in getting widespread adoption for encrypted communication.  I suspect this is because it has a critical usability problem - key signing parties are a bad idea and relying on users to generate public/private key pairs and encrypt messages with the public key of other users they’re trying to contact is unrealistic.  Encrypted communication via PGP in its current state will never expand beyond the smallest group of technical users - even in cases where the [user’s life depends on it][moxie-link].

Governments have realized this and haven’t pushed for a restriction of encryption because it hasn’t posed much of a threat to access.  This is changing as companies begin to encrypt communication by default, but even with this change it's unclear how secure the communication is and if it can still be vulnerable to a man in the middle attack from its parent company.

Several companies have targeted making secure messaging easy so people will actually use it.  In the best case there’s [Signal][signal] whose implementation is available and secure, but still suffers from the difficulty of driving users to their platform.  In between there’s iMessage with a massive platform of users, but unclear implementation and its thought that the possibility still exists for Apple to man in the middle the messages.  The worst case examples are apps like Telegram that claim to be secure, but don’t provide much to verify their claim.

Even in the best case example - all of these applications are siloed, they require all users to be a part of their individual application in order to securely communicate.

[Keybase.io][keybase] is trying to make PGP better, recognizing the issues inherent to PGP - notably that key signing parties to verify identity are a bad idea.  They’re making pretty interesting tools to try and build an accessible API for people and I think their concept of using social media accounts to validate identity is a good idea.  Unfortunately keybase.io has a big challenge in getting users to their platform, but there is a platform that already exists that has a large percentage of the earth already using it: Facebook.

I think Facebook should generate a public/private key pair for each individual user and then create APIs to allow third parties to access user’s public keys as well as use mutual friend information in order for users to verify identities.  For most users Facebook could just host the private key encrypted for them (like Keybase does), but they should also allow users to upload their own Public key instead - something Facebook already does.

This would immediately create the world’s largest public key server and abstract the complexity away from the majority of users.  It would also allow third party apps to rely on a system that allows interoperability between services making it easier for users to have actually secure messaging without having to have every application individually implement it for its own users.  Facebook has the platform and Keybase is working on the tech - I think it’d be an interesting acquisition.  Facebook could also likely implement this themselves - though there would still be the challenge of getting developers to actually use it.  At least with that audience there’s some hope.

[moxie-link]: http://www.thoughtcrime.org/blog/gpg-and-me/
[signal]: https://whispersystems.org/
[keybase]: https://keybase.io/
