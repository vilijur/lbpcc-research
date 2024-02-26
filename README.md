# LittleBigPlanet™2 Cross-Controller Research
This repository contains research on the inner workings of how the Cross-Controller application and DLC works for LittleBigPlanet™2. It is a work-in-progress while I figure out what's what.

This readme covers the main issue with the Cross-Controller app which served as motivation to dig deep into the code to figure out what's going on and how everything works. There are also notes containing the current information I have gathered on Cross-Controller's inner workings.

## Repository Information
Now, this readme isn't gonna contain every piece of research. That would be long, and messy, and annoying to read. So this repository is split up into folders.

Below is an index of the repository. Each folder will have its own readme to explain its purporse:

[todo: add index]

# Introduction to Cross-Controller
Durring the PS Vita's lifespan, Sony released a feature known as Cross-Controller. This allows the PS Vita to be used as a second screen and as a second controller in supported applications (Think Wii U GamePad but the console and GamePad are sold seperately and isn't required for the system to function). One game to utilize this feature was LittleBigPlanet™2 with the paid DLC expansion known as LittleBigPlanet™2 Cross-Controller. This paid expansion would only be seen in LBP2 and wouldn't return in the game's sequel, LittleBigPlanet™3.

In LittleBigPlanet™2, the player could use Cross-Controller to make custom levels that utilize both the PS3 and PS Vita's screens on their moon for other people with Cross-Controller to play. Cross-Controller levels are split into two parts: One for the PS3, and one for the PS Vita. Included in the Cross-Controller DLC is a extra story mode that takes advantage of the dual screen setup. Like most paid stories in LittleBigPlanet™, it is realatively short compared to the main story mode, and contains prizes in each level (with cutscenes being an exception).

With all that being said, you may be inclined to pick up LittleBigPlanet™2 Cross Controller and play it for yourself, however...

## The Problem
LittleBigPlanet™2 Cross-Controller has two main components: The game itself and the PS Vita companion application. The PS3 runs LittleBigPlanet™2 as per usual, and inside the gamefiles is a directory called `CROSSDIR`, which contains the application needed to connect to the game. This is known as LittleBigPlanet™2 Cross-Controller on the Vita's LiveArea™. This application is forked off of LittleBigPlanet™ PlayStation®Vita, and as such has some leftovers from that game. Leftovers that happen to include the main problem you'll encounter if you try to play Cross-Controller.

You'll get a fustratingly worded error telling you that there's an issue with your Vita's network connecttion when there's not. Despite this all working over a local network, it just won't connect!

![The Cross-Controller error message. It reads "There is an issue with the PlayStation®Vita system's network connecttion. Please check your network settings within the PlayStation®Vita system Home Screen."](https://github.com/vilijur/lbpcc-research/blob/104b287b6218ef546e1454f584eb5a2e52a771af/images/ccerror.jpg)

This error message has no reason to occur. So why does it?

This had me stumped for quite some time. Why would the game refuse to connect to my PS3 when my Internet connection is perfectly fine? What had me even more confused is that m88youngling had managed to get the Cross-Controller application to work using [UnionPatcher](https://github.com/LBPUnion/UnionPatcher), while I had failed to get it to work. The error didn't go away. What was going on?

A Discussion on ProjectLighthouse's GitHub was opened, which you can read [here](https://github.com/LBPUnion/ProjectLighthouse/discussions/429). So, what happened?

## Explanation (with terrible visuals)

[todo: add better alt text]

When you press Connect on the application, the Vita connects to the LBP Vita servers to check the system's Internet connection. Think of this as the Vita saying hello to the game server.

![greeting](https://github.com/vilijur/lbpcc-research/blob/560d71aa7aceb04e1cc8af9479cb72ac3584fbc9/images/handshake1.png)

The server responds, which tells the Vita that it has a network connection. Satified with this response, it begins searching for a PS3 that's sending out a request for a Vita to connect to it for Cross-Controller

![search](https://github.com/vilijur/lbpcc-research/blob/560d71aa7aceb04e1cc8af9479cb72ac3584fbc9/images/handshake2.png)

All is well, everything works, case closed, everyone's happy.

But! This server no longer exists, which means it doesn't respond. This causes the Vita to believe it lacks a network connection, and then promptly throws up the error message.

![error](https://github.com/vilijur/lbpcc-research/blob/560d71aa7aceb04e1cc8af9479cb72ac3584fbc9/images/handshake3.png)

This is where the issue stems from. The Vita is reliant on a now non-existent server to do something over the local network.

In an ideal world, the Vita would skip the network check entirely and just search for a PS3 to connect to.

![ideal world](https://github.com/vilijur/lbpcc-research/blob/560d71aa7aceb04e1cc8af9479cb72ac3584fbc9/images/handshake4.png)

But we don't live in that world, so we'll have to do something else instead...

## The Solution

Now that we know why this happens, the solution is rather simple: Patch the game's executable (eboot.bin) to point to a server that isn't dead. But, we've already tried that with UnionPatcher and that didn't seem to help, so what's going on here?

The thing with the Cross-Controller application is that it has multiple URLs inside of the eboot, all leftovers from LBP Vita. But there's one part of the eboot that stood out to me and the others I was researching with: **`lbpvita.online.scee.com.sessionMaster`**. Unlike the other URLs, this one lacked `http(s)://` at the start, and strangely enough ended in `sessionMaster`. So I got to testing. I replaced this URL with the URL used for Beacon at the time, and low and behold...

It worked! So that's it, problem solved, right? For the most part, yeah! With this new discovery in hand, it was added to [the discussion](https://github.com/LBPUnion/ProjectLighthouse/discussions/429#discussioncomment-3479740), and a guide on how to patch Cross-Controller was written up by me in the LBP Union Discord server.

However there was one theory on my mind: This is a DNS query that the Vita sends, so it should be theoretically possible to make Cross-Controller work on OFW. After some quick testing, this turned out to be true, meaning it is possible to use Cross-Controller in current year without using any Custom Firmware! How neat is that?

# Now what?

[todo: find out what to do now]
