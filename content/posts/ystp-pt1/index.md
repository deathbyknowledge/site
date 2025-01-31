+++
date = '2025-01-30T18:42:35+01:00'
draft = false
title = 'YSTP: Cloudflare Workers and Durable Objects tutorial (Pt. 1)'
tags = ['software', 'cloudflare']
+++

This Christmas I was thinking on revisiting a Rust port of [croc](https://github.com/schollz/croc) I wrote some years ago to teach myself the language but ended up doing something different.

I had just been introduced to Cloudflare's [Durable Objects](https://developers.cloudflare.com/durable-objects/) and thought of **porting the port** and check how hard it would be to build the same thing on the Cloudflare stack.

It proved to be incredibly educational and quite fun so I ended up using it in an introductory talk for the stack.

# What are we building?
**YSTP** (short for **Your Super Transfer Program**) is a file transfer tool that allows sending a file privately to a party directly without relying on port-forwarding to be set up.

It's quick and easy to use and it all lives on Cloudflare's edge, built with Workers and Durable Objects. No storage anywhere, just a transfer.

{{< youtube 1XfHqnbAR-A >}}

You can use the one I built ([ystp.dbk.wtf](https://ystp.dbk.wtf/)) or you deploy your own. Let's go.

# Why are we building it?
Let's have a look at what happens when we share files nowadays.
1. **Alice wants to send a file to Bob.**
![a](ab1.jpeg)


2. **Alice uploads the file *somewhere* accessible by Bob**
![a](ab2.jpeg)

3. **Alice waits for the upload to finish...**
![a](ab3.jpeg)


4. **Alice shares the link to the file with Bob**
![a](ab4.jpeg)



5. **Bob waits for the download to finish...**
![a](ab5.jpeg)



6. **Profit!**
![a](ab6.jpeg)



7. **Profit?**
![a](ab7.jpeg)

## OK, what's going on?
I'm not going to explain how the internet works here but basically it is not straightforward for any device to connect directly to another device not public on the Internet (think **Alice's iPad** to **Bob's computer**). That normally requires extra setup on both ends, which we don't want to do if we're sending a file to a friend.
![a](internet.jpeg)

So, in the previous example **Alice** does an entire transfer to a publicly accessible server (like **Doogle Grive**) and stores the file there. Only then, can she share access to the file so **Bob** can do a *second* transfer from the server to his device.

Well, that's _one_ solution but we don't want to _store_ anything in between; we just want to _transfer_ to the end device. So how shall we do it?

# Game plan
We'll build a publicly accessible **relay**, where both clients can establish a WebSocket connection and communicate between each other directly, the **relay** acting as a bridge. 
![gameplan](gameplan.png)

So our protocol will follow these steps:
1. **Sender establishes WebSocket connection**
![step1](step1.png)
2. **Relay sends the session code**
![step2](step2.png)
3. **Sender shares file metadata**
![step3](step3.png)
4. **Receiver connects with code**
![step4](step4.png)
5. **Relay sends file metadata**
![step5](step5.png)
6. **Receiver accepts transfer**
![step6](step6.png)
7. **Relay tells sender to start transfer**
![step7](step7.png)
8. **Transfer happens**
![step8](step8.png)
9. **Transfer completes**
![step9](step9.png)
10. **Profit**
![step10](step10.png)

## Hold on... what about the Cloudflare part?
Well spotted, astute reader. As you might be expecting, we won't require an actual server to build our relay. Instead we'll build it on top of [Durable Objects](https://developers.cloudflare.com/durable-objects/) which, contrary to standard serverless technologies, allows us to have stateful code and even hibernate WebSocket connections, making it a great fit for our use case (hibernating is good because if we were to be charged for every second our **relay** had **active** connections, we wouldn't get very far with my budget).

Of course, Cloudflare will provide us with an endpoint to our Worker and Durable Object that will be publicly accessible, so it already covers all our requirements!

# Building the thing
I've already dumped enough text here, so check out {{< backlink "ystp-pt2" >}}.


