---
slug: migrating-old-netcoreapp
title: Migrating a Webapp from netcoreapp2.1
date: 2021-10-14
blurb: "Migrating .NET projects is not too bad!"
---

Recently, we had an important API service suddenly stop working at work.

This service is supposed to send out a personalised link to everyone in the team, but it randomly stopped working, and stayed that way for 3 weeks. Here is how we fixed it.

There were many reasons why it took so long to be fixed:

1. Everybody that had once worked on it weren't part of the team anymore, so nobody really understood the codebase.
2. Nobody in the current team had the knowledge or confidence to troubleshoot and solve the issue. The service was written 2018 with the `netcoreapp2.1` framework, and most of the team was fairly new to C#, learning on the `.net5` framework.
3. There was other, more interesting work to do.

Eventually, it was becoming a bigger issue, as people weren't getting their personalised links anymore, we were losing observability. It was just too tedious and error-prone to have to manually customise the links for everyone.

At about week 2, four of us dedicated some time diving into the codebase and trying to troubleshoot the issue. We found that the service that wasn't working was dependent on another API, which was served with a Let's Encrypt certificate.

The [DST Root CA X3](https://community.letsencrypt.org/t/openssl-client-compatibility-changes-for-let-s-encrypt-certificates/143816) expired around the day our service stopped working, and there was an issue with OpenSSL versions 1.0.0 through 1.0.2, where they will reject Let's Encrypt certificates, regardless of whether ISRG Root X1 is installed on the client. Lo and behold, [.NET Core 2.2 and earlier versions prefer OpenSSL 1.0.x over 1.1.x](https://docs.microsoft.com/en-us/dotnet/core/compatibility/cryptography#net-core-30-prefers-openssl-11x-to-openssl-10x).

Digging into this further, we ran our service in docker with different images from .Net Core 2.1 to .Net 5, and tried curling the external API.

| Runtime | curl result |
| ------- | ----------- |
| .Net Core 2.1 | SSL certificate problem: certificate has expired |
| .Net Core 2.2 | SSL certificate problem: certificate has expired |
| .Net Core 3.0 | 200 |
| .Net Core 3.1 | 200 |
| .Net 5.0 | 200 |

Great! So it seems like we just need to update to .Net Core 3.0 and our issues should be fixed.

Luckily for us, Microsoft has some pretty good [docs for migration ASP.NET Core versions](https://docs.microsoft.com/en-us/aspnet/core/migration/21-to-22). We just followed the docs, skipping over parts that we found weren't necessary. We made the decision to update to .Net 5 instead of sticking to .Net Core 3.0 so that hopefully when we eventually move on others will have less work to do, and have an easier time fixing anything.

What I learnt from this was: even if I don't know what's wrong or what to do, just volunteer and I'm bound to learn something in the process. The worst that could happen is the service stays down.
