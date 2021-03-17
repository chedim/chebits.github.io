# Passwordless, databaseless authentication with JWT

Json Web Tokens is an amazing technology, and I worked with it for a while already. Adopting this technology can save a company lots of resources as it allows to validate user session purely on the client and without any requests to authentication servers.

Working on your own projects, however, allows one to experiment with JWTs even further -- for example, I've just finished writing a simple nodejs(+express) authentication server that authenticates users via their email and never asks them for their passwords (aka "Slack magic link") -- all users need to do is just enter their email and click on the magick link in an email message that this server sends to them. The "magic link" contains a short-living JWT token that is used to verify that user has access to the email address. After the token is verified server marks it as revoked (in the only database table it uses) and then issues new access token (that also serves as a refresh token) -- very simple workflow that doesn't require passwords at all :)

What project am I going to use it for, you ask? Of course it will be used to provide you and Blink keyboard buyers with new experience on chebits.com :)
