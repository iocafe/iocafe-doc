About security vulnerabilities
===============================

Technically it is relatively easy to create implement networks that are almost impossible to break.
With almost impossible I mean, a network that is technically so secure that authorized people network 
users and suppliers are the only real risk. The people are a risk in three ways, they can be coerced 
to give access tokens to outsiders, they can use those access tokens for their own personal benefit 
or vendetta, or they can simply be relaxed about the security. There are ready tools, like TLS, 
client/server certificates, user accounts, firewalls, and system-level security, which (if used correctly) 
will provide practically bulletproof protection.

Biggest threat is that legitimate users can make it really easy for a hacker, ay worst they collect all 
that is needed to log into VPN into one folder (certificate, certificate password, VPN username, and password) 
so that they get it working on other computers or when it breaks. They may even put a post-it note to their
monitor to remember what is the password this month and so on...

Threat created by either malicious or careless people can be addressed by educating the workforce and 
contractually enforcing responsibility to people who are authenticated to use it. The risk grows with
the number of authenticated people.

There are few good publications worth something. RFC2196 is probably the best I have come across, 
and that is what we try to follow. https://tools.ietf.org/html/rfc2196
