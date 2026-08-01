---
title: The art of portal placement sparks - short research
image: /assets/2026-08-01-portal2-sparkmaps/thumbnail.jpg
---
One man's game bug is another man's tech-art project!

![](assets/2026-08-01-portal2-sparkmaps/banner.jpg)
## 1. A spark of betrayal

Portal placement in Portal 2 is one of those game mechanics that feel intuitive enough that people don't put any thought into it. You shoot a portal and it appears on a wall - seems simple enough. However, in that split of a millisecond, the game performs a bunch of clever algorithms in order to detect the edges of a surface that was just shot, attempts to nudge the portal away from them and ensure the portal is properly placed on a portalable surface. All of that to make sure player's intention is properly addressed - a wall is shot, so portal should appear on it, as long as it can fit there.

Unfortunately, the more complex the system is, the more likelihood of a glitch to occur. Portal placement is no exception. Anyone who played this game for long enough can tell you how sometimes the game decides to simply ignore player's intention and prevent the portal from being placed, despite there being enough space for it. You can imagine how betrayed someone can feel when they expect a portal to be opened, but instead they're greeted with pesky spark particles, normally seen by shooting non-portalable walls. Outrageous and unfair!

![](assets/2026-08-01-portal2-sparkmaps/lies-deceptions.jpg)

For causal players, it's not a big deal - they'll shoot it again and it will usually work. However, for speedrunners, encountering this issue even once can feel like being punished by the game for no apparent reason. This naturally pushed them to find a way to prevent their portal shots from sparking. It was quickly noticed that some sections of portalable surfaces are more likely to cause a spark. It helped with consistency, but it was still a guesswork. If only there was a way to visualize those sections...

## 2. The beginnings of sparkmapping

On March 1st 2022, past [SourceAutoRecord](https://github.com/p2sr/SourceAutoRecord) developer [mlugg](https://github.com/mlugg) has created a feature called `pp_scan` (portal placement scanner). The way it works is quite simple! For a defined 2D rectangle in the game's world, a fake portal shot is being triggered through the original game's code. Then, shot position is mapped to a pixel in a texture, and a result of a portal shot is stored as either green or red color, for successful or failed portal placement respectively. This process is repeated for every pixel in a target texture, which means that thousands, or sometimes even millions of portal shots have to be simulated in order to create a single image. The result gives you a precise picture of what part of a scanned surface would result in a spark.

That's where the art of **sparkmapping** was born, with those images being dubbed **sparkmaps**. Many people in the speedrunning community started to share not only sparkmaps for various places in the game, but also their feeling of contempt towards the game's internal behavior. 

![](assets/2026-08-01-portal2-sparkmaps/live-sparkmap-reaction.jpg)

Those images became a very useful resource - if anyone had trouble with their portals sparking at any spot, they could consult a sparkmap of that spot to know what's the most preferable part of the surface to aim for.

Now, it could be argued that an artistic value could also be spotted in them as well, especially with all of those shapes filled with repeating noise patterns that could be more commonly associated with glitch art. In fact, mlugg wasn't sure whether their first results were a correct representation of a game state or just a graphical error.

![](assets/2026-08-01-portal2-sparkmaps/mlugg-message-2.jpg)

Myself being very interested in internal behavior of Portal 2, and having several attempts at understanding how portal placement code works behind me (and even a successful one!) I was fascinated by this form of presenting its result. What can be learned from observing those patterns? How would they look like if it worked just a bit better? Can one learn to... read them?

## 3. Thinking with... line trace?

Before we start looking at those weird shapes, I'm gonna give you a chapter filled with some nerdy explanations. It'll be easier to understand sparkmaps if we understand what causes the sparks in the first place. I'll try to simplify things whenever possible, since not everything is directly related to the topic we're discussing, but just in case, I've put a massive amount of details in the P2SR Wiki article about [Portal Placement](https://wiki.portal2.sr/Portal_Placement).

As previously mentioned, the game needs to determine if and where a portal should fit on a shot surface. Just by looking at a portalable surface, we could roughly tell that. But we're humans. We're naturals at pattern recognition (as presented by an image below)! Unfortunately, it's a privilege computers don't have, since they can't see the world the same way we do. 

![](assets/2026-08-01-portal2-sparkmaps/loss_placement.jpg)

We can try our best to develop algorithms to compensate that - image and geometry processing, shape recognition, multi-step collision resolution... but then suddenly our game needs more than 60th of a second to shoot a portal and player experiences a lag spike. That's bad! The game needs to be able to see its own world and do it efficiently.

This is where the concept of **collision line tracing** comes in! It's a nifty piece of gameplay logic which allows you to mathematically determine the physical properties of the game's world along a given line, including where the line intersects and what it intersects with. It's super cheap, especially in Source Engine, which is especially optimized for this operation, with most of the level geometry being built out of planes (and that's just [a couple of formulas](https://en.wikipedia.org/wiki/Line%E2%80%93plane_intersection) away from what we need!). Using a bunch of these bad boys in a couple of discrete positions, you can gain a rough approximation of what's going on in the game's world, with essentially no cost.

Portal placement is one of gameplay elements which heavily utilizes this technique - from a simple trace to determine where our portal shot lands after we shoot it, to hundreds of traces used to determine how the portal should fit on a surface. To visualize this, I hacked the game to draw lines for each line traced during portal placement and took a couple of screenshots. Here's one of them (with more to come later!):

![](assets/2026-08-01-portal2-sparkmaps/traces_visualized1.jpg)

Very cool, but how do we use this technology? After all, we can't just go and start blasting line traces everywhere. At some point, it would mitigate the performance benefits we're getting from this approach. Developers had to be selective about which discrete points to pick in order to find information about the surface on which they're trying to fit the portal.

The approach they went for revolves around the corners of a presumed portal, which explains the X pattern that emerges in the screenshot above. Seems logical - corners cover the entire surface behind a portal, so if they're on a portalable surface, the entire portal must also be on it! It makes perfect sense... until you realize edges are completely disregarded, and you can place portals clipping into corners like this just fine:

![](assets/2026-08-01-portal2-sparkmaps/questionable-portal.jpg)

Considering this is a bug that actually makes portal **not** spark, let us acknowledge its irrelevance and talk more about those corners.

We can easily detect if any walls are enclosing the surface by tracing a line slightly in front of the surface we just shot, going from the shot point to a potential corner point. Likewise, we can detect the edge of a solid surface by checking that line in reverse, but instead slightly within the surface. However, what about cases where the surface is flat - where both portalable and non-portalable surface is flush with each other?

Interestingly, the first Portal game doesn't even handle those cases, preventing a portal from being placed completely. This is why, in Portal, such cases of portalable surfaces are usually surrounded with invisible portal bumpers, to act as an enclosing wall for portal placement.

![](assets/2026-08-01-portal2-sparkmaps/portal1moment.jpg)

Now, whether it was a way to make life easier for map designers, or to support unpredictable amount of irregular portalable surfaces (especially with addition of conversion gel, which is able to create portalable surface seemingly anywhere any time with no seam defined by geometry), an algorithm for detecting such portalable surface edge has been implemented for Portal 2, and it involves doing a bunch of tiny line traces towards the shot surface, across the space between a center point and all corners:

![](assets/2026-08-01-portal2-sparkmaps/traces_visualized2.jpg)

Fortunately for game's performance, it's done in a smart way - instead of just blindly tracing lines to find the portal edge, a binary search is performed. This way, both position and orientation of a surface edge can be determined in just about 30 line traces! Unfortunately for the game's intent, this binary search is not deep enough to be precise enough, introducing a bit of an error.

 Even worse, there's a drastic issue with how edge orientation is determined. The idea is fine, and rather straightforward - a second point on the edge is determined by doing yet another binary search, similar to the first one, but in a circle around the first point, and then both points are used to determine orientation. 
 
 The issue is in implementation. Here's a C++ pseudocode I've created - try to figure out what's wrong before reading further!

```cpp
// we know "edgePoint + originToCornerOrthogonal * testDistance" is portalable,
// but "edgePoint + originToCornerDirection * testDistance" is not.
float invalidSurfaceAngle = 0.0f;
float validSurfaceAngle = 90.0f;
for (int i = 0; i < 10; i++)
{
    float testAngle = (validSurfaceAngle + invalidSurfaceAngle) * 0.5f;
    Vector testPoint = edgePoint
        + cosf(testAngle) * originToCornerDirection * testDistance
        + sinf(testAngle) * originToCornerOrthogonal * testDistance;

    if (IsPointOnPortalableSurface(testPoint)) validSurfaceAngle = testAngle;
    else invalidSurfaceAngle = testAngle;
}
```

The intention was to do this search starting from 90 degrees, which after 10 iterations of a loop, would result in either of angle variables being close to the value we're looking for, with an ~0.088 degrees margin of error. However, trigonometry functions take **radians** as parameters, not **degrees**. As a result, the initial angle for a binary search is 90 **radians**, which is roughly 5156.62 degrees. Because of that, the algorithm has a good chance to spit out nonsense, and even the best outcomes have a potential error margin of 5 degrees - big enough to cause some real issues. 

What's fascinating is that you can see results of that error for yourselves in the game right now! When console variable `sv_portal_placement_debug` is set to 1, the game draws a bunch of debug overlays when shooting a portal, which include lines showing what the algorithm thinks the edge of a surface is. Well... not sure I agree with the way it thinks, because those lines very are often not even close to being angled properly:

![](assets/2026-08-01-portal2-sparkmaps/whatarethooose.jpg)

Naturally, developers predicted some kind of imprecisions would occur *somewhere*. That's why, in most cases, whenever a portal is bumped from a checked portal corner, a "bump forgiveness" value of 2 units is added (for a scale, portal is 112x64 units). But some cases did not get a memo - more specifically, a case where portal is bumped away from a corner of a surface, and that's where you'll see the most of sparks. In fact, lack of any leeway in this case can make even the smallest errors lead to a portal sparking. And, if you recall, such error can also come from not only edge's orientation, but position as well.

I guess that's it for nerdy explanations. Well, this is the part where I patch the game.
## 3. The Part Where I Patch The Game.

Hello! This is the part where I patch the game! I'm using previously mentioned SourceAutoRecord plugin as a base for my modifications. One reverse-engineering session later, I gained signature scans for every place in memory that I want to change. More specifically, I wanted to:
- include bump forgiveness value when portal is bumped from a surface corner.
- increase the precision of broken binary searches for edge position and orientation detection.

Having those being applied dynamically by a plugin allows me to easily creates comparisons, and see how sparkmaps change when I enable or disable certain tweaks. I'm gonna go through several curious cases, taking a set of four screenshots each to see all permutations of tweaks.

Let's start with a simple example - a flat surface with corner portal surface. It can be found in level called Laser Chaining.

![](assets/2026-08-01-portal2-sparkmaps/pp_scan_1.jpg)

As you can see, without tweaks, that entire corner is filled with a repeating pattern. It makes sense, as the precision loss is dependent on where the binary search starts, so one would expect it to repeat in some way. You can also see the repeating in both axes, as the phenomenon I described before happens for both top and side edge of a surface. The pattern is actually much more complex - it's just what it converged into at this specific zoom level.

A bump tweak is eliminating this pattern almost entirely, leaving only a small region of 6 units still affected. It doesn't compare to a precision tweak, which removes the issue altogether with a sheer correctness of calculations. 

Curiously, with enough precision, a fundamental flaw of the edge orientation algorithm emerges. It can now incorrectly determine edge's orientation if a corner of a portal ends up being exactly a tiny distance away from a corner of a surface (I invite you to consult previously posted pseudocode and try to figure out why, but if you just want to know, [here's a very basic explanation that I've drawn in MS Paint](assets/2026-08-01-portal2-sparkmaps/puzzle_solution.jpg)). As a result, anything along a very thin diagonal line is detected as non-portallable. It just goes to show how complicated, and thus, fragile, this whole thing is!

Let's check another sparkmap. This one in co-op level Bridge Testing has actually been named by the community - "Speech Bubble". Go figure.

![](assets/2026-08-01-portal2-sparkmaps/pp_scan_2.jpg)

There are two interesting characteristics about this one. An obvious one is the circular shape. Map designers has placed something called a portal placement helper on this surface, which is essentially a spherical region that will automatically snap a portal that was shot within it to a correct position. It's a neat workaround for all of those bugs in critical places where you want your portal placement to always succeed, and it's used quite frequently across the entire game. Honestly, I could write an entire separate blog post about those! (I'm not sure if I should...)

Another unique trait is how stripes in spark regions are horizontal. Considering how vertical edges are all geometry based, and the game deals with them perfectly, we see oscillation in a pattern only by varying the distance from horizontal edges, hence why it changes vertically.

Again, we see a proof of bump tweak mostly fixing the issue, but not entirely, and precision tweak being necessary to completely eliminate this issue. Weirdly enough, when you combine both of these tweaks, a small non-portalable dot appears in the bottom right sector of that surface. Apparently, in that spot, the game decides there is not enough space to fit a portal, because of the fact we're bumping it that small extra distance. However, without precision tweak, the deviation happens to be helpful, preventing the portal from being bumped into an opposite wall. A weird combination, honestly.

In any case, it seems like my tweaks are actually doing something! Could it be that I solved portal sparking?! Let's check more extreme example, like this one from the level named Smooth Jazz:

![](assets/2026-08-01-portal2-sparkmaps/pp_scan_3.jpg)

Oh... well, it definitely seems to be less noisier after precision tweak...

You see, in cases like these, the problem doesn't lie in bugs in code, but in the approach itself. As hinted previously, small irregularities can make the algorithm give up, thinking it will never find enough space around given point, or even if it does, then the portal would be nudged too far away to feel natural. Funnily enough, in the case shown above, precision issues actually help the portal being nudged into a portallable surface just by sheer luck. 

We could dive into this rabbit hole much deeper, try to find viable alternatives for a portal placement algorithm, like pre-baked portal placement maps or mini physics solver... However, just for now, I'm going to accept what we have, appreciate that it works most of the time, and admire the abstraction it forms when it fails. (yes I'm lazy 🙃)

With that being said, here's an absolute masterpiece created by portal placement algorithm in Portal Stories: Mel. The world record holder of this Portal 2 mod, [Burger40](https://www.twitch.tv/burger40), has humbly named it "The Firestorm Bong":

![](assets/2026-08-01-portal2-sparkmaps/firestorm-bong.jpg)

## 4. I call this piece... "Spark".

I hope that, after slightly prolonged explanation and a short exploration of portal sparks, you can see why I find them so fascinating! They're a window to the game's internal behavior, a capture of what the game sees and what we can't, a unique perspective of thinking about the virtual world which is not only natural, but necessary for a machine, but abstract and alien to us humans! I find it incredibly important, especially in the age of machine learning algorithms, to think about methods of portraying how computers think.

As a final homage to this niche piece of technology, I'll leave you with what's probably one of my favorite videos yet, where I bring the art of portal sparking to its extremes, and create a live recording of sparking surfaces, as seen by the game's algorithm:

![](https://www.youtube.com/watch?v=BLdkiCwBoWg)

