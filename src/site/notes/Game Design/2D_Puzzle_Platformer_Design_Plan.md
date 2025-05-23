---
{"dg-publish":true,"permalink":"/game-design/2-d-puzzle-platformer-design-plan/"}
---


# Design Plan: 2D Puzzle-Platformer Inspired by *Animal Well* and *Braid*

## **Introduction**

  

This design outlines a **2D platform puzzle-solving game** for PC, developed solo on a custom engine. The game draws inspiration from the creative, layered puzzle design of _Animal Well_ and the innovative mechanics and philosophical tone of _Braid_. The goal is to deliver a **deeply engaging puzzle-platformer** that emphasizes exploration, mental challenge, and an atmospheric, thought-provoking experience. Players will explore a mysterious interconnected world, solve multi-layered puzzles, and uncover hidden secrets, all while experiencing a minimalist yet surreal narrative backdrop. The focus is entirely on **game design** – crafting mechanics, levels, pacing, aesthetics, and replayability – rather than technical implementation. The following sections detail the core gameplay concepts and how they build on lessons from _Animal Well_, _Braid_, and other acclaimed puzzle-platformers.

  

## **Core Gameplay Mechanics**

  

**Innovative Puzzle Mechanics:** The core gameplay will center on inventive mechanics that challenge players to think in new ways. Like _Braid_, which was “built entirely around time manipulation as the central gameplay mechanic” , our game will feature a unique primary mechanic that defines the player’s interactions. For example, the player might wield the ability to **shift between two parallel realities** (a “light” and “shadow” world). In one reality, certain objects or platforms exist, while in the other they don’t – requiring players to **swap worlds** to navigate platforms or solve puzzles. This twist on altering reality builds on _Braid_’s time-bending ideas but offers a different flavor of mind-bending challenge.

- _Multi-use Tools:_ Inspired by _Animal Well_’s items that have “multiple uses and let you manipulate your environment in surprising and meaningful ways” , the player will collect a small set of tools or abilities, each with versatile applications. For example, a **Lantern** item might illuminate dark areas, reveal hidden clues, and ward off certain shadowy creatures. A **Time Bubble** power could freeze objects in place or slow moving hazards, allowing creative solutions to otherwise impossible platforming challenges. Every tool is designed with **layered interactions** – encouraging players to experiment and discover more than one use (much like _Animal Well_’s firecracker plants that both light areas and scare enemies ). This provides a steadily expanding puzzle vocabulary without overwhelming the player at once.
    
- _Environmental Interaction & Physics:_ Core mechanics will involve interacting with the environment in clever ways. Following the _Animal Well_ approach of “collect items to manipulate your environment in surprising ways” , puzzles might require using the environment itself as a tool. For instance, players could push or rotate certain structures, redirect light beams, or lure creatures into positions that trigger switches. The game’s rules will remain consistent, but the **puzzle scenarios will be open-ended**, rewarding creativity. A player who tries an unconventional approach (like stacking objects or using a tool in an unintended way) might find a secret solution. By designing mechanics that support **experimentation**, we ensure puzzles are satisfying to solve when the “aha!” moment strikes.
    

  

**Building on Inspirations:** Our mechanics pay homage to the inspirations but also push into new territory. _Braid_ introduced one new time-manipulation twist in each world, with each puzzle built around that world’s rule . In our game, rather than segregating mechanics by level, we will **layer mechanics together** as the game progresses. Early on, the player might learn each tool or ability in isolation; later puzzles will combine them. For example, a late-game puzzle could require swapping realities mid-jump while also using the time-freeze bubble to suspend a moving platform – a multi-step solution that tests the player’s mastery of all core mechanics. The design intent is that **each mechanic is simple to grasp** on its own, but when combined, they yield deep and creative puzzle challenges.

  

## **Level Design Strategy**

  

**Gradual Introduction & Tutorial Level:** The game will start by **easing the player into the mechanics** in a safe, controlled environment. Similar to how _Braid_ “eases the player in by introducing first the very basic mechanic of time manipulation, simple time rewinding” in a low-risk context , our opening area will present the core mechanic (such as reality-shifting) in a simple puzzle that can be solved almost by accident. This ensures players grasp the idea organically. For instance, a short prologue level might show a gap too wide to jump – with a prompt (visual or subtle) to use the reality-shift ability, revealing hidden platforms in the alternate world to cross. No explicit tutorial text is needed; the level design itself teaches the concept through **doing**.

  

**Structured Progression of Challenge:** Levels (or areas in an open world) will be structured to **gradually increase complexity** as players become more proficient. Early puzzles will use one mechanic at a time, in clear scenarios. As the game progresses, puzzles will start combining mechanics and requiring more steps. Drawing on best practices for learning curves, “well-designed mechanics gradually introduce players to new concepts and challenges, allowing them to learn and master at their own pace” . Each area of the game will introduce a twist or new use for a tool: for example, after learning the Lantern can scare simple shadows, a later level might present a friendly creature that is _also_ afraid of light, teaching the player through level layout that not everything reacts the same way. By the final stages, the player should feel like all the skills and knowledge they’ve accumulated are tested in clever, **multi-layered puzzles**.

  

**Non-Linear Exploration:** A key design goal is to encourage **exploration and experimentation** rather than rigidly linear progression. The world will be structured as a **semi-open, interconnected map** (in the vein of a Metroidvania), where multiple paths are available and backtracking is possible with new knowledge or items. _Animal Well_’s world is described as a “dense, interconnected labyrinth” , and we aim for a similar layout. Levels will have **multiple exits or secret passages**, enabling skilled or observant players to find alternate routes. While core puzzles gating main progress will have a intended solution, there may be _sequence breaks_ where a clever player uses an advanced technique earlier than expected to access later areas – the level design will account for this with hidden shortcuts or bonus rooms as rewards.

- _Introduction of Mechanics in Stages:_ The game’s map can be divided into themed regions, each highlighting one mechanic or tool. For example:
    
    - **Area 1:** Focus on core platforming and the reality-shift mechanic alone (basic puzzles to establish trust in the mechanic).
        
    - **Area 2:** Introduce the Lantern tool in a dark environment, teaching how light reveals secrets and alters creature behavior.
        
    - **Area 3:** Introduce the Time Bubble in a region with moving hazards, teaching how freezing time can aid traversal.
        
    - **Area 4+:** Mix these mechanics – e.g. a puzzle in an ancient ruin requiring reality-shifting to see hidden platforms _and_ using the Lantern to activate light-sensitive switches.
        
        Each area acts as a **learning sandbox** for a concept, then a final puzzle in that area serves as a culmination test.
        
    

  

**Encouraging Experimentation:** To make experimentation fun, the level design will **minimize penalties for failure**. If a player attempts a creative solution that doesn’t work, the game will allow quick retries (for example, instantaneous respawns or a rewind feature similar to _Braid_ to undo mistakes). Early levels will have visible cues and gentle hints – such as a strange symbol indicating something might be interactable in the alternate reality – but later on, cues become more subtle to increase the challenge. The design follows _Animal Well_’s philosophy where “nothing is accidental” in the placement of clues . A peculiar arrangement of objects or a suspicious detail in the background might hide a secret. By structuring levels as **puzzle playgrounds**, we encourage players to poke around and try ideas. Nearly every puzzle has at least one intuitive solution, but many have alternate solutions or hidden outcomes for those who experiment – making the world feel rich and responsive to creative play.

  

**Non-Linear and Layered Goals:** While there will be a main path that most players can follow to reach an ending, the level design also supports **non-linear goals**. For instance, players might encounter a locked door requiring three symbols; the three corresponding puzzles can be solved in any order (or perhaps a subset of them is enough, giving some choice). This non-linearity ensures that if one puzzle stumps a player, they can explore elsewhere and return later with fresh insight. The world will loop back on itself in clever ways (shortcuts, hub areas) to make exploration convenient and rewarding. The result is a level structure that **gradually teaches and then opens up**, giving players freedom to chart their own course through the puzzle landscape.

  

## **Player Progression and Pacing**

  

**Ability and Knowledge Progression:** Player progression in this game is measured not by experience points or stats, but by **the acquisition of new abilities and crucial knowledge**. Early in the game, progression is tied to tangible new tools (e.g. finding the Lantern opens all puzzles involving light). However, akin to modern puzzle-adventure designs like _Outer Wilds_ or _Tunic_, some progress will hinge on the player **understanding a clue or pattern** rather than acquiring a key. _Animal Well_ embodies this “Metroidvania where progress is mental (knowledge-based) rather than purely ability-based” . For example, an early area might have strange glyphs on the wall that seem irrelevant; only later does the player learn to interpret them, allowing them to return and solve a puzzle they previously couldn’t. This means **pacing is not just about items, but insights** – each major “aha!” effectively acts as a level-up in progression.

  

**Pacing of Discoveries:** To keep engagement high, the game will carefully pace out its puzzles, discoveries, and story beats. The design avoids long stretches of repetitive gameplay; instead, every area introduces _something new_, be it a mechanic, a new type of challenge, or a piece of story. A possible pacing outline could be:

1. **Introduction (Hour 0–1):** Establish basic movement and the core shifting mechanic. Quick payoff with a simple puzzle solved and a glimpse of the mysterious world.
    
2. **Emergent Exploration (Hour 1–3):** The player ventures into the labyrinth freely, encountering the first ability/tool and a variety of easy-to-medium puzzles. Sprinkle in small secrets (extra collectibles or bonus rooms) to reward curiosity early.
    
3. **Mid-Game (Hour 3–6):** Difficulty ramps up. More complex puzzles appear as mechanics start combining. At this stage, just as the player might start feeling confident, introduce a **plot twist or new environment** to renew the sense of wonder. For instance, perhaps the player descends into a deeper layer of the world where the rules slightly change (e.g. gravity is lower, or certain creatures now behave differently), forcing re-evaluation of known mechanics.
    
4. **Late-Game (Hour 6–8):** The main puzzles now require mastery of all abilities. Multi-step puzzles span multiple rooms or require linking clues from different areas – truly testing the player’s comprehension of the game’s logic. The narrative’s central mystery moves toward resolution, adding urgency.
    
5. **Climax (Hour ~8):** The final sequence presents the most challenging puzzle or series of puzzles, acting as a **master exam** of everything learned. The pacing here might temporarily quicken with a sequence that feels almost like a chase or a high-stakes trial, though still puzzle-centric (for example, a collapsing dream-reality that the player must navigate by quickly swapping between worlds and using tools under time pressure). This creates a thrilling finale without deviating into pure action.
    
6. **Resolution:** After the climax, allow a short, calmer epilogue section for the player to reflect, perhaps tying up narrative hints and showing the consequences of the player’s actions. This provides emotional closure before credits roll.
    

  

**Maintaining Engagement:** Throughout the game, we alternate between **intense puzzle-solving segments and moments of relief**. Solving a difficult puzzle is rewarding, but players need a breather afterwards to avoid mental fatigue. Thus, a challenging puzzle room might be followed by a more exploratory section with light platforming and atmospheric storytelling (a quiet area to walk through and absorb the scenery, possibly uncovering a bit of lore). This **puzzle-rest rhythm** keeps players from feeling overwhelmed and sustains long-term engagement. Additionally, the game will allow players to **set their own pace**: there might be optional puzzles or secret areas that a player can skip until later. If stuck, a player could wander in the world, perhaps stumble upon a clue elsewhere, then return with a new idea. This echoes _Braid_’s structure where you didn’t need to collect every puzzle piece immediately to see the basic ending – you could progress and come back later for the hardest ones, maintaining momentum. By designing the progression to be flexible and player-driven, we ensure that frustration is minimized and the sense of discovery remains front and center.

  

**Reward and Feedback:** Pacing also involves rewarding the player regularly. Small victories (like discovering a hidden room or solving a minor puzzle) will be acknowledged, perhaps with an in-game chime or a visual reward (e.g. unlocking a beautiful new vista or earning a piece of lore text). Major milestones, such as obtaining a new ability or solving a key puzzle that unlocks a new region, will tie into the narrative – for example, a short cutscene or a surreal vignette might play, deepening the mystery. These rewards serve to **motivate players to continue**, making the pacing feel satisfying as peaks of difficulty are followed by rewarding revelations.

  

## **Aesthetic and Narrative Style**
![animal_well.png](/img/user/images/animal_well.png)_A surreal pixel-art scene from_ **_Animal Well_**_, exemplifying the mysterious atmosphere and strange creatures that inspire the game’s tone._

  

**Visual Style:** The game will adopt a **minimalist yet surreal art style** that enhances the sense of mystery. Given that this is a solo-developed project, a cohesive artistic vision is achievable. Like _Animal Well_ – which uses pixel art with modern rendering effects – our game can use **hi-bit pixel art** (retro-inspired but leveraging modern lighting and shaders for atmosphere) . Environments will be richly detailed in their own subtle way: dimly lit ruins, otherworldly landscapes, and abstract architecture that feels just slightly “off,” evoking a dreamlike quality. The color palette may shift with the game’s dual-world mechanic: for instance, the normal world could have muted, earthy tones, while the alternate reality is depicted with ethereal neon glows and silhouettes. This visual contrast not only differentiates gameplay states but reinforces the surreal tone. Each screen is designed with careful composition – empty space and darkness are used intentionally to create feelings of solitude or anticipation, while occasional splashes of color or motion draw the eye to points of interest (possibly hinting at puzzle clues). Overall, the aesthetic should feel **intentionally minimalist** (no excessive HUD clutter, very little on-screen text), allowing players to immerse in the world’s ambiance.

  

**Atmosphere and Audio:** The atmosphere will be **thick with mood** – quiet and contemplative for the most part, punctuated by moments of tension. Drawing inspiration from _Limbo_ or _Inside_ (which, like _Animal Well_, maintain an eerie atmosphere), the sound design will favor ambient environmental sounds (dripping water in a cave, distant echoes, creature calls) and a subtle musical score that swells at key moments. Silence is a tool as well; when nothing is happening, the absence of sound can create suspense. When the player enters a new area or discovers a secret, a gentle music motif or sound cue can highlight the significance. The narrative’s philosophical tone can also be reflected in the audio – for instance, a distorted music box tune playing in a forgotten place could hint at themes of memory or time. By using audio-visual elements in harmony, the game will cultivate an **atmospheric experience** that lingers with the player.

  

**Narrative Delivery:** The story will be delivered in a **minimalist, interpretative manner**. There will be no intrusive cutscenes or lengthy dialogue sequences. Instead, the narrative unfolds through _environmental storytelling_ and subtle cues, much like _Dark Souls_ or _Hyper Light Drifter_ (though those are different genres, their lore delivery is minimalist). For example, the player might encounter ancient murals or statues in the background that, when pieced together, suggest the history of this world. Short text snippets or symbols might appear when discovering important locations – cryptic messages rather than explicit explanations. _Braid_ took a slightly more direct approach with textual storybooks between levels, but our design leans even more on **surreal ambiguity**: the player is given pieces of a story but must connect the dots themselves. This encourages a personal interpretation and theorizing (e.g. “What does the broken clock tower in the dream world signify?”).

  

The narrative itself will have a **philosophical or metaphorical layer**. Without spoiling a hypothetical plot, we can envision the player character’s journey as an allegory – similar to how _Braid_’s tale of a missed princess can be read as a metaphor about regret and the atomic bomb . Our game’s story might revolve around themes of **knowledge and enlightenment** (fitting the puzzle-solving aspect): the act of solving puzzles and uncovering hidden layers could mirror the protagonist’s internal quest for understanding or redemption. By the end, the narrative should invite the player to reflect. It might pose a question or reveal a twist that recontextualizes the adventure (in true _Braid_ fashion, where the final level inverted the narrative perspective). Importantly, the **tone will remain subtle and reflective**, never breaking the fourth wall or overly explaining its themes. Players who just want puzzles can enjoy the game without being forced into cutscenes, whereas lore seekers will find plenty to ponder beneath the surface.

  

**Minimalism in Presentation:** Consistent with a minimalist narrative, the UI and presentation will also be kept simple. There is no on-screen tutorial text beyond perhaps icons or a few words. The character design might be a simple silhouette or a small figure (as in _Animal Well_, the protagonist is a little blob emerging from a flower ). This allows players of all languages or backgrounds to step into the character’s shoes easily, projecting their own interpretation onto a relatively blank slate. The overall artistic direction ensures that the **mystery and atmosphere take center stage** – every visual and narrative element should serve to deepen the intrigue or support the gameplay, without extraneous clutter.

  

## **Replayability and Hidden Depths**

  

One of the hallmarks of this design is the **layered depth** that encourages players to revisit the game even after reaching an ending. Taking strong inspiration from _Animal Well_’s multi-layer puzzle design and _Braid_’s hidden secrets, we will implement multiple tiers of secrets and optional challenges:

- **Multiple Puzzle Layers:** The game will be designed for _at least two or three “layers” of play_. The **first layer** is the base game that all players can experience: the main puzzles and an ending that concludes the primary story. A casual player might stop here, satisfied with a coherent adventure. However, observant players will likely notice clues that not everything has been solved. Thus, a **second layer** of puzzles awaits: secret challenges and collectibles hidden off the beaten path, for those who choose to dig deeper. Much like _Animal Well_’s 64 hidden eggs, secret rabbits, and candles which only dedicated players pursuing completion will uncover , our game will include hidden items or puzzle elements (for example, collectible fragments of a cipher, or hidden switches in hard-to-reach places). Solving these second-layer puzzles might unlock a **secret ending or additional content** – perhaps an extended epilogue or a reveal of the true nature of the world. Finally, for the truly passionate, a **third layer** could contain the most obscure puzzles, designed to stump even genre veterans and encourage community collaboration (e.g. meta-puzzles that require sharing hints). In _Animal Well_, some third-layer puzzles “aren’t exactly intended to be easily solvable by one person” and indeed required players to combine unique puzzle pieces from dozens of people to solve a riddle . In our game, we might include one grand mystery, such as a secret language or an ARG-style puzzle, that might take the community to unravel – ensuring the game’s secrets live on **for years** after release .
    
- **Optional Challenges and Alternate Endings:** Replayability is enhanced by giving players reasons to replay with new goals. We will include **optional challenge rooms or bonus puzzles** that are not required for the main story but offer extra content for enthusiasts. These could be especially difficult puzzle levels accessible only after finishing the game or via hidden entrances. Completing all optional challenges might reward the player with an alternate ending or a special in-game reward (like a cosmetic change or a dev commentary room as a nod to the solo developer’s journey). _Braid_ famously had 8 hidden stars that were extremely hard to find (one even requiring an hour-long wait on a cloud); collecting them all revealed a secret ending and an alternate perspective on the story. We plan a similar approach: a **“100% completion” reward** that gives a deeper interpretation of the narrative for those who master every secret. Crucially, these hard secrets remain completely optional – they enrich the lore and provide bragging rights, but a player can enjoy the core game without them. This way, players of different dedication levels each get a fulfilling experience.
    
- **Community and Puzzle Sharing:** The game’s design will foster a sense of **community discovery**. Given the complexity of some secrets, players will be naturally inclined to discuss theories and share hints. As a solo developer game, building a community can greatly extend its life. Features like a cryptic in-game poem or a series of nearly inscrutable clues might be laid out such that no single player is likely to solve them alone – a technique that _Animal Well_ successfully employed to get players talking and working together . For example, we could randomize a certain puzzle input per player (like each player’s copy of the game generates a unique rune), so that only by comparing notes can the full solution be found. This not only encourages replay (players might start a new file to see a different permutation of a clue) but also **drives community engagement** outside the game. While these collaborative puzzles are not required for solo completion, they add a meta layer of replayability and excitement for the most devoted fans.
    
- **Layers of Interpretation:** In addition to gameplay secrets, the story and thematic elements will have **multiple layers of interpretation**. The narrative will be intentionally open to speculation, meaning that players might replay the game to seek clues supporting one theory or another. For instance, after seeing the ending, a player might realize that certain early scenes hinted at that twist – prompting a second playthrough to appreciate the foreshadowing. We will include a few subtle variations in the game on a second playthrough (New Game+ mode) to acknowledge this – perhaps small changes in dialogues or visual details that weren’t present initially, which further illuminate the story’s meaning. This design echoes how _Braid_’s narrative could be re-read in a different light once you knew the final reveal, or how games like _Undertale_ altered content on replays to enrich their themes. Our goal is that players who complete the game will still feel there are mysteries to unravel and reasons to return, whether to solve puzzles they missed or to deepen their understanding of the story’s **philosophical subtext**.
    

  

**Long-Term Engagement:** By combining the above elements, the game achieves a high degree of replayability. The **main playthrough** provides a satisfying adventure of moderate length (for example, 8-10 hours to finish the story and basic puzzles). The **extended content** – secrets, harder puzzles, and multiple endings – can easily double that playtime for those inclined, much like _Animal Well_ left a reviewer spending “nearly 30 hours… and still discovering new things” due to its “seemingly infinite depth” . Importantly, this is done not by brute-forcing length, but by **adding depth**. The game is, in essence, a _puzzle box_: easy to open at first, but containing more boxes within boxes for those who keep probing. The design ensures that even without updates or new levels, the game “remains interesting and giving back far into the future… players will be discovering secrets for years” . This not only respects the solo development scope (by maximizing content through clever design rather than sheer quantity) but also honors the inspirations – delivering a puzzle-platformer that players will remember, discuss, and revisit, far beyond the first completion.

  

## **Conclusion**

  

In summary, this design plan presents a comprehensive vision for a 2D puzzle-platformer that marries **creative mechanics, thoughtful level design, deliberate pacing, atmospheric artistry, and rich layers of secrets**. By learning from modern classics like _Braid_ and _Animal Well_, the game will offer players a journey that is at once challenging and delightful: a series of puzzles that engage the mind, an intriguing world that invites exploration, and a narrative that resonates on a deeper level. With its custom-engine foundation, the game can achieve a distinctive look and feel, ensuring it “looks and plays like no other game” while still echoing the greatness of its influences. Crucially, every element – from mechanics to story – is designed to complement each other, resulting in a cohesive experience. Players will gradually master innovative mechanics, traverse non-linear labyrinthine levels, and uncover an escalating series of mysteries that keep them invested. The philosophical, minimalist narrative will linger in their thoughts as they piece together its meaning, just as they piece together the game’s puzzles.

  

By focusing on **depth of design and creative ideas**, this plan ensures that the final game isn’t just a one-time playthrough, but a compelling _experience_ – one that encourages discussion, collaboration, and replay. In the spirit of the puzzle-platformers that inspired it, this game aims to stand out as a love letter to the genre: **layered in secrets, rich in creativity, and ultimately, deeply rewarding** to those who venture into its world.