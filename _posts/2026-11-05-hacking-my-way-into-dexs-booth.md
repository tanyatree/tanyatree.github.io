---
layout: post
title: "I Had to Hack a Save File to Attend a Meeting in Cyberpunk 2077"
date: 2026-11-05 22:05:00 -0300
tags: [python, debugging, reverse-engineering, gaming, learning]
---

Tonight I was playing Cyberpunk 2077 on my Mac, trying to do something very simple: go to a meeting.

If you've played the game, you know the moment. It's "The Heist". Jackie and I walk through the Afterlife to see Dex. Jackie went through the door to Dex's booth. The door stayed closed, with me on the wrong side of it, and I couldn't go back either. I was locked in the bar.

A few hours later, an AI and I had decompressed a save file, found V's coordinates in a binary blob, and moved her just over a meter, to the other side of a closed door. This is how a normal evening of gaming turned into that.

## Act 1: I made it worse

My first idea was reasonable: maybe my game files were corrupted. So I asked GOG Galaxy to verify them. It seemed to take forever, so I cancelled and quit. Then I tried to uninstall the game.

The uninstall failed, and then Galaxy wouldn't let me reinstall because there wasn't enough disk space. At this point I opened Claude Code and asked for help.

The logs told a much better story than the UI did:

- **The verification had already finished.** It ended with `files to download 0`. Nothing was corrupted. It got stuck *after* that, and that's when I cancelled.
- **The uninstall half happened.** It deleted a few small metadata files, then failed on the app bundle with a permissions error.
- **Galaxy carried on anyway.** My favorite line of the night: `uninstall failed; proceeding with remove from DB`.

So the game was still on my disk, but Galaxy no longer knew about it. All 99 game files were there, and their total size matched the manifest to the byte: 79,880,707,760. The "not enough space" error was most likely Galaxy wanting room for a second copy of a game that was already installed. Only 4 tiny metadata files were missing.

**More → Manage → Import folder** linked it back. Galaxy then re-downloaded 13 GB of DLC that was already sitting in the folder, because apparently that's how we do things, but I didn't have to reinstall all 80 GB.

The game worked again. And the door was still closed. Of course it was: the files had never been the problem.

## Act 2: The black room

Then something went wrong with the debugging too, and I think it's the most useful part of this story.

Claude read my save files' metadata and noticed two autosaves with a later objective, `participate_briefing`. It concluded I must have already been inside the meeting at some point, so loading one of those would skip the door.

I told it that was wrong. I had never been in that room.

Those saves turned out to be real, but they showed something else. Their screenshots were almost entirely black. When I loaded one, the briefing started without me. I was standing in a dark space outside the booth while Dex got increasingly annoyed that I wasn't doing anything. Honestly, fair.

The data was accurate, but the conclusion wasn't. Only I could catch that, because only I knew what had actually happened in the game.

We then found [a post on the CD PROJEKT RED forums from December 2020](https://forums.cdprojektred.com/index.php?threads/dexs-booth-the-heist-bug.11048756/) describing the same thing: the door won't open, Jackie "wall glitches in", and you end up "in a black room outside the Booth". So I'm not the only person this has happened to, even if the report is almost six years old. The suggested fix was to force quit and restart. It didn't work for me.

## Act 3: So, can you just put me inside?

I looked for mods first. The Mac options I found targeted the Steam version, and I'm on GOG. So I asked the obvious question: could we move V in the save itself?

It turns out you can. I'll be upfront that Claude did the digging here, while I watched and asked questions. Here's what a save looks like, at least on my 2.5 Mac build:

- The file starts with `VASC`, followed by a table marked `FZLC` listing the compressed chunks.
- The data is split into 256 KiB LZ4 chunks, each starting with `4ZLX`.
- At the very end is a table of named sections, marked `ENOD`. One of them is `LocalPlayer`.

Decompressed, my 2.1 MB save became 7.7 MB. V's position was right there in `LocalPlayer`, as three plain little-endian floats. We found it by searching for the coordinates already listed in the save's JSON metadata:

```python
import struct

x, y, z = struct.unpack_from("<fff", raw, local_player + 12)
# (-1435.88, 976.81, 16.83)
```

The same coordinates also appeared in a second section, `PersistencySystem2`, so we changed both.

The next question was where to put her, since nobody had coordinates for the inside of Dex's booth. But the save knew. The `GPSSystem` section held the objective marker, only 1.25 meters away from V. A few other objects sat a little further in the same direction, so that's probably where the room was.

We didn't need to write a real compressor, either. LZ4 allows a block made of one long run of uncompressed bytes, so we only rewrote the two chunks we had changed, and left the rest of the file alone:

```python
def lz4_literal_block(data: bytes) -> bytes:
    """A valid LZ4 block with no actual compression."""
    if len(data) < 15:
        return bytes([len(data) << 4]) + data
    header = bytearray([0xF0])
    remaining = len(data) - 15
    while remaining >= 255:
        header.append(255)
        remaining -= 255
    header.append(remaining)
    return bytes(header) + data
```

Because the chunks got bigger, the offsets in the chunk table and the pointer to the `ENOD` table had to be updated too. The save grew from 2.10 MB to 2.47 MB, but the script checked that only the coordinates had changed in the decompressed data: 14 to 16 bytes out of 7.7 million.

We made three new saves, each a little further inside, and never touched the original. The first one, placing V right on the objective marker, worked. She was finally on the right side of the door.

I got to attend the meeting. I have never been so happy to be briefed by a fictional fixer.

## What I'm taking away from this

- **Read the logs before you trust the UI.** "Verifying" looked stuck, but the log had already said nothing was wrong. I could have skipped the whole uninstall disaster.
- **Check what you can see against what actually happened.** The save metadata was correct, and the conclusion drawn from it was still wrong. Knowing what actually happened in the game mattered as much as parsing the file.
- **Back up first, and write to copies.** It's what made it safe to try something this silly.
- **Binary formats are less scary than they look.** I've spent years thinking this stuff was for "real" low-level programmers. Sometimes it's a table of offsets and some floats, and Python's `struct` can read it just fine.

I still don't know why the door doesn't open. I've written up a report that I'm going to send to CD PROJEKT RED, because this fix shouldn't be anyone's solution. And if you're stuck in the same place, save editing is at your own risk: I only tested this on my own save, on this one build.

But I do have to admit that I started the night wanting to play a game about a hacker, and it ended up being the most hacker I've ever felt.

Tanya

***
