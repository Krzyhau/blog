---
title: FEZ spectrogram images - personal endeavor
image: /assets/2025-02-05-fez-spectrograms-adventure/thumbnail.jpg
---
I've generated a crisp-clean render of images hidden within FEZ original soundtrack. All it took was a couple of scripts and... smuggling of audio tracks from France?

![](assets/2025-02-05-fez-spectrograms-adventure/banner.jpg)
## 1. Adventure

[FEZ](https://store.steampowered.com/app/224760/FEZ/) is a game that has been close to my heart since I played it. I remember my younger self slowly figuring out all the meta puzzles and falling in love with the formula of metroid-brainia. To this day, I look back with joy to the day I ran back from school just to make it in time to collect an anti-cube from the clock tower. Shortly speaking, FEZ heavily influenced me both as a gamer and a game developer. So, not surprisingly, every once in a while, I'll try to join my passion for completely random technical bullshittery with some of the puzzles from this fantastic game (some of which are yet to be solved to this day!)

One of these puzzles are hidden within the game's soundtrack. Its composer, Disasterpeace, approached the game's creator, Phil Fish, with an idea of hiding images within audio tracks (about which he wrote [on his own blog](http://disasterpeace.com/blog/fez.postmortem.html)). The idea is to have an image encoded into a very specific sound wave, so that when it's viewed as a [spectrogram](https://en.wikipedia.org/wiki/Spectrogram), the image appears. Quite fun thing to do for something like a meta puzzle or an [ARG](https://en.wikipedia.org/wiki/Alternate_reality_game). In fact, an earlier instance of this technique being used in the mainstream that I know of is [Portal ARG](https://half-life.fandom.com/wiki/Portal_ARG), where the team behind Portal 2 teased the fans with a series of spectrogram images you could decode from sounds transmitted from radios you could find throughout the game.

Here's a small example of such thing that I made using an online converter. I've put both the original image and the image produced by previewing the encoded audio file in a spectrogram view in Audacity below, as well as the audio itself. Just be warned, this audio does awful things to your ears, so lower down the volume before trying to play it back.

![](assets/2025-02-05-fez-spectrograms-adventure/check-this-shit-comparison.png)
<audio src="assets/2025-02-05-fez-spectrograms-adventure/check-this-shit-audio.wav" controls preload></audio>

I've decided that recovering those images from the soundtrack is a fun idea for a side project! However, just opening a spectrogram viewer and screenshotting every track doesn't sound like something I'd enjoy doing (also, it was done multiple times before me already). What I'd like is to automate this process, so I can easily iterate on my tweaks and apply them to all tracks at once, then see how clean of images I can recover with a bunch of tweaks and slight post-processing. On top of that, well, once I have these images, I might do some kind of artsy thing with them! 👀

## 2. Puzzle

To start up, I focused on figuring out the best method of extraction. I wanted the output to be as clean and detailed as possible. And here's where the first problem emerged - while Disasterpeace tried to insert them in places where there's as little noise as possible, in some places the generated sound is mixing with a background noise from the soundtrack, resulting in a complete mess...

![](assets/2025-02-05-fez-spectrograms-adventure/what-a-mess.jpg)

I tried different methods - audio mixing, image processing etc. -  but there's only so much you can do without *additional data*... hmm, so what kind of *additional data* could help here?

You see, there's this cool thing called **phase cancellation**. As a general simplification - sound waves work like math:
- When you play two sound waves together, you get one that is a mix of two: `A + B = C`.
- Likewise, if you have that mixed audio and play it along with one of the original waves, but with an *inverted phase*, you get the other one in isolation: `C - B = A`.

This means that, if I had the same exact soundtrack but **unpuzzlified** (as in, without spectrogram images in it), I could invert its phase, then mix it with the **puzzlified** version (the one with spectrogram images) and get a pure audio file of just spectrogram images with absolutely zero additional noise! This meant it was time to begin the hunt for the unpuzzlified version of the OST!
## 3. Beyond

I'm actually not the first person to come up with this idea. A Reddit user named Supersaiyan_IV [has made a post](https://www.reddit.com/r/Fez/comments/1ynlh1/fez_ost_image_analysis_idea/) about their version of the OST also being unpuzzlified, and using it to decode clearer versions of hidden images. Interestingly enough, they obtained it through a Humble Indie Bundle 11 offer. Unfortunately for me, these offers are time-limited, and I'm already about 11 years late for it as of writing this article. However, if the unpuzzlified version appeared in something as typical as Humble Bundle, then surely it can be easily obtained somewhere else... right?

In my hunt, I used the track `03 - Beyond` as a reference - the hidden image in this track is right at the beginning and it has an amplitude big enough to be heard quite easily as a high-pitched noise. You can listen for the difference yourself (headphones are recommended):

<audio src="assets/2025-02-05-fez-spectrograms-adventure/beyond-puzzlified.wav" controls preload></audio>
<audio src="assets/2025-02-05-fez-spectrograms-adventure/beyond-unpuzzlified.wav" controls preload></audio>

Here are all the sources of the soundtrack that I managed to find:
- [Disasterpeace Bandcamp album page](https://music.disasterpeace.com/album/fez-ost) - when bought, you get access to variety of audio formats (FLAC, WAV, MP3 and so on...). Unfortunately, all puzzlified.
- [Steam's store page](https://store.steampowered.com/app/224763/FEZ_Original_Soundtrack/) - gives you two versions, FLAC and MP3. Both of them puzzlified. However, there are witnesses which claim Steam was distributing an unpuzzlified version for a short moment.
- Officially released vinyl record (it has no rights to [look this cool](https://coloredvinylrecords.com/disasterpeace-fez.html)) - based on [a video of a rip I found on YouTube](https://www.youtube.com/watch?v=A8ENJhW1YqY) I can tell it has unpuzzlified soundtrack pressed onto it, and presumably it came with a digital version as well, but these days it's incredibly hard to come by
- **Humble Indie Bundle 11** - it had unpuzzlified soundtrack, but as I said before, I'm 11 years too late... I could ask someone who bought it that way to give me the files (I was actually talking with someone who did when researching this project), but I was eager to see if there's a way to obtain the unpuzzlified soundtrack through more *legal* means these days.
- **All the popular streaming services (YouTube, SoundCloud, Apple Music etc.)** - surprisingly, they all have unpuzzlified versions of the soundtrack! However, in most of them, the tracks are compressed (which would influence the phase cancellation process) or there's no legal way of ripping those tracks out of these services.

At this point, my search has led me to [Qobuz](https://www.qobuz.com/us-en/album/fez-original-game-soundtrack-disasterpeace/vhpqon0wdtloa) - some kind of french streaming service I've never heard of. It had FEZ soundtrack on it, and it allowed download of raw, lossless music track files. And despite the fact I couldn't be sure these tracks are unpuzzlified, I had a reason to believe so: it seemed like it was distributed by a music label which was in close association with IDOL - a music distributor who distributed FEZ soundtrack on other streaming services.

![](assets/2025-02-05-fez-spectrograms-adventure/found-it.jpg)

There was really only a one way to find out if they had what I was looking for, and that was to simply put some money on a table. Unfortunately, I had some slight difficulties of making a purchase, since Qobuz is region-locked, and it wasn't letting me register an account in Poland, nor it accepted my transactions. But that's what you've got social connections for! I've contacted a French colleague of mine from the [old Portal 2 TASing days](https://www.youtube.com/watch?v=MZi1dXwCqG8) to act as my FEZ OST smuggler and spend 5 euros for a soundtrack of a game he barely heard of (I still owe him a beer lol). After a short exchange of words and making fun of French language... I've got it! I became a proud **legal** owner of unpuzzlified FEZ Original Soundtrack! 🥳

First thing I did - I downloaded one of the tracks to perform phase cancellation in Audacity, and my goodness, I've never seen this image being as crisp as this (well, apart from [the original](https://www.youtube.com/watch?v=EyfOh2za-po)). If you compare it with a screenshot I posted earlier in this post, the difference is shocking!

![](assets/2025-02-05-fez-spectrograms-adventure/heehee.jpg)

## 4. Progress

As I said, I wanted to automate the isolation process, so that I could tweak the parameters and achieve the best looking result for all images at once. 

To achieve this, I first prepared a small database of where in the soundtrack spectrogram images are located - track name, time, duration and frequency range. It was as daunting as it sounds... For about an hour or so, I was in a loop of opening a track, looking for a hidden image, measuring the time span and frequency range, then writing it down. I could've automated it, but I've decided that fine-tuning the algorithm and filtering out bad apples would take longer than doing it manually.

Ultimately, I ended up with this list:

```
01 Adventure > (none)
02 Puzzle > 2:08.900 - 2:12.250, 800-20000Hz
03 Beyond > 0:00.000 - 0:06.500, 800-20000Hz
04 Progress > 4:10.400 - 4:16.000, 1200-13000Hz
05 Beacon > 2:29.140 - 2:32.340, 500-20000Hz
06 Flow > 0:02.000 - 0:05.190, 500-20000Hz
07 Formations > (none)
08 Legend > 1:15.370 - 1:18:270, 100-20000Hz
09 Compass > 2:37.350 - 2:44:050, 120-20000Hz
10 Forgotten > 2:32:110 - 2:34:360, 800-20000Hz
11 Sync > (none)
12 Glitch > (none)
13 Fear > 3:29.410 - 3:32.050, 800-20000Hz
14 Spirit > 2:48.130 - 2:52.180, 900-20000Hz
15 Nature > (none)
16 Knowledge > (none)
17 Death > (none)
18 Memory > 1:13:630 - 1:16.270, 800-11000Hz
19 Pressure > (none)
20 Nocturne > (none)
21 Age > 2:46.450 - 2:56.200, 90-20000Hz
22 Majesty > 3:15.360 - 3:21:810, 700-20000Hz
23 Continuum > 2:29.010 - 2:35.860, 450-20000Hz
24 Home > (none)
25 Reflection > 8:13.900 - 8:20.480, 800-21000Hz
26 Love > 1:02.870 - 1:09.090, 450-20000Hz
```

Thanks to recent technological advancements (and fortunately for my lazy butt) I didn't have to write a custom parser or manually rewrite the list into a more usable format, and I could just let AI create a JSON file for me! Honestly, saying that we live in the most convenient times for technical experiments like these is an understatement.

![](assets/2025-02-05-fez-spectrograms-adventure/yo-mr-chat.jpg)

Once I had the JSON file, it was pretty straight-forward as well. I've decided to use Python - I cannot put enough emphasis on how easy it is to prototype stuff with it these days. Even though I started knowing barely anything about specific libraries I'd need for the job, with massive amounts of information both within official documentations and on StackOverflow, as well as being assisted by tools like GitHub Copilot, I've gotten from nothing to a finished product within a couple of hours.

The created script does the following for each track:
- loads both **puzzlified** and **unpuzzlified** versions of the track,
- performs **phase cancellation** on them, isolating a track which contains purely the sound generated from the image,
- cuts the audio using the database I previously created and saves it to a file,
- generates a spectrogram image using Fourier transform algorithm from [librosa](https://librosa.org/) and then plots it using [matplotlib](https://matplotlib.org/).

Additionally, there were some interesting edge cases that I had to handle that I think are worth mentioning, mainly:
- Track `09 Compass` is actually a few seconds longer in a puzzlified version, to fit the image in the silent part at the end of the audio file. I simply left that part unmodified, since, logically, there was no audio to perform phase cancellation on in the first place.
- Both tracks `10 Forgotten` and `14 Spirit` have the aplitude of the audio lowered down in the puzzlified version to make the hidden images more visible (oh irony). I had to figure out how much it was lowered down to be able to make a perfect phase cancellation. **15dB** seemed to be the magic number.

With the script done, it was just a matter of fine-tuning... lots of fine-tuning. I tried my best to find a set of Fourier transform parameters giving the cleanest results, but I also relied on doing some math on the resulting data set to tweak brightness and contrast across different frequencies (the resulting images were much brighter on lower frequencies). Apart from that, I've also tried some random ideas in hope of improving the quality, including:
- Using different parameters for specific ranges of frequencies - I observed that some parts of the spectrogram image were becoming a little bit sharper when using specific parameters for Fourier transform. I abandoned this idea due to some weird technical issues that I didn't bother to fix, but it might be worth exploring.
- Randomize parameters to generate a set of outputs, then return an average of them - I actually kept this in the project. The resulting image is less noisy, with a small cost of being slightly blurrier.

If you have any ideas on how I could get more clearer images from these audio tracks (not including obvious approaches like standard de-noising algorithms) then I'd like to hear them! But for now, I'll stick with what I've got right now and move on to the artistic part of the project!

![](assets/2025-02-05-fez-spectrograms-adventure/heehee2.jpg)

## 5. Nature 

Once the technical part was done, I could do anything with those images - having a computer do the heavy lifting for me means I could go creative and represent the data in any way I wanted.

What I aimed for was an effect similar to contact sheets - I imagined images rendered in gray-scale, arranged one next to another, with some additional cool metadata written around them on a frame. This kind of old-school lost media vibes seem rather fitting, and doesn't sound that hard to replicate in the most basic form.

![](assets/2025-02-05-fez-spectrograms-adventure/contact-sheet-example.jpg)

This is the moment where I nerd out over **matplotlib** for a second - WOW, what an amazing library! I thought that I'd need a separate image processing library to present all additional details properly, but it turned out it allowed me to do all the key things I wanted while being fully configurable! It also feels like I only glanced over the tip of the iceberg, so I might need to have a rendezvous with it at some point and just plot a bunch of useless graphs.

With some tweaking, I made the script spit out an absolute beauty of an image! The labels not only add up to the vibe I wanted to achieve, but they are actually informative as well: they tell you where in the audio track you can find those images - both in time and frequency. Honestly, it's something I'd happily put on a poster or a t-shirt!

![](assets/2025-02-05-fez-spectrograms-adventure/fez_spectrogram_images.png)

## 6. Love

This is where I leave this project to collect the dust. I've put everything that I've created for it in a [GitHub repository](https://github.com/Krzyhau/fez-spectrogram-reader), both code and output files (available to download from Releases tab) - go wild with them! 

To wrap it all up, I've decided to make it look even cooler by slapping a quick 3D scene in Blender. It's probably one of the coolest things I've done in a while. 

Let's hope I'll make something even cooler soon enough, about which I'll make sure to write a blog post... whatever that will be!

![](assets/2025-02-05-fez-spectrograms-adventure/fez-spectrograms-blender-render.jpg)