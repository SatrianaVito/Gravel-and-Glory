# Claude Code prompt: "Gravel & Glory" Episode 1, pixel-art animation

*Paste everything below the line into Claude Code, in an empty folder. If you have the script, put it in the same folder as `ep01.txt`. This prompt doesn't depend on it: every scene, line and detail the animation needs is quoted here.*

---

## The job

Build a **pixel-art animated short** that retells the notable events of **Episode 1 of my TV drama *Gravel & Glory*, "If Buffum Can Do It"**. It should run **about 3½ to 4 minutes** and play like the opening of a 16-bit-era game: tiny sprites, chunky backgrounds, speech bubbles, title cards, and a chiptune score.

**Deliver:**
1. **`index.html`**: one self-contained file that plays the whole film in a browser. Everything inside it is drawn in code. No external images, fonts, libraries or network requests.
2. **`export/`**: a script that records the film to **`gravel-and-glory-ep01.mp4`** (and a short **GIF** of the Hump jump). Use Playwright's bundled Chromium, step the timeline frame by frame, screenshot each frame, then join them with ffmpeg. Install anything missing (`npm i playwright`, `apt-get install ffmpeg`, or `pip install imageio-ffmpeg`).
3. **`README.md`**: how to open, play, scrub and export.

**Work in stages and show me after each one** (see "Build order" at the end). Don't build all of it in one go.

---

## The show, in two lines (for tone)
Two brothers from Calumet, Michigan, **Dean** (driver) and **Tommy** (co-driver) **Koski**, win a small-town rally in their dad's old Datsun and decide to take on the 1983 World Rally Championship. Their father **Ike** says yes on his own terms; an old friend, Texan oilman **Harlan Bowie**, pays; and they end the episode in Scotland, buying a Renault 5 Turbo and trying to win over a furious mechanic, **Rab McCallum**. **The tone is warm comedy with a lump in the throat:** big jumps, small family moments, a crow that disapproves of everything.

---

## Technical spec

- **Canvas:** native **320 × 180** pixels, scaled up by an integer factor (letterboxed to fit the window), `image-rendering: pixelated`, `imageSmoothingEnabled = false`. Never draw at sub-pixel positions: round everything.
- **Frame rate:** a fixed **12 fps** for sprite animation (the look), with the timeline clock running in real time. The export renders at 12 fps, scaled ×6 to 1920 × 1080.
- **Everything procedural:** sprites as small pixel arrays or string maps (`"..##.."`) in code, with a palette lookup. Backgrounds built from tiles and simple shapes with dithering. Reuse sprites across scenes (the Datsun, the Renault, each character).
- **Timeline engine:** a list of scenes, each `{ id, title, duration, draw(t, ctx) }`. Within each scene, a small keyframe/tween helper (position, opacity, camera pan, shake). One global clock so playback and export are deterministic: **no `Math.random()` without a fixed seed.**
- **Controls:** space = play/pause; ← / → = previous/next scene; a thin scrubber bar with scene ticks at the bottom (hidden in export mode); `M` mutes; `?export=1` in the URL hides all UI and lets the export script drive the clock (`window.renderFrame(n)`).
- **Text:** a hand-made **5×7 pixel bitmap font** in code (capitals, lower case, digits, basic punctuation, `£ $ ° #`). Speech bubbles with a pixel border and a tail; text typed on at about 30 characters a second, with a soft blip per character. Keep each bubble short (the lines below are already trimmed).
- **Sound (Web Audio, generated in code):** a short **original** chiptune loop per section (square + triangle + noise channel), engine notes that rise with the car's speed, a gravel crunch, a splash, the crowd's cheer (filtered noise), a horn, a rotary-phone dial, rain. **Write original melodies only. Don't quote or imitate any real song.** (The script mentions a Bob Seger tape on a boombox: show the boombox, play your own tune.)
- **No real logos or trademarks** drawn on anything: no car-maker badges, no beer or brand labels, no TV-channel marks. The fictional names below (KOSKI SERVICE & REPAIR, BOWIE OIL, the Copper Kettle, the signs) are fine and wanted.
- **Real people:** no likeness of any real person. The "John Buffum" newspaper clipping is a headline only, with a blurred grey photo block. The Audi on the diner TV is a generic white boxy car with stripes, no badge.

### Palette (about 32 colours; add a few if a scene needs them)
- **Michigan night and snow:** near-black blue `#0b1020`, forest `#14261f`, birch `#d8d4c8`, snow `#e9f0f5`, snow shadow `#9fb3c8`, old-nickel sky `#5f6b78`.
- **Loretta, the Datsun:** pale yellow `#e8d77a`, mud brown `#6b4a2b`, lamp white `#fff6d0`.
- **Texas:** caliche `#e2c9a0`, sky `#6fb6e8`, hot white `#fff3e0`, mesquite `#5d6b3a`, pool turquoise `#2ec4c4`.
- **Scotland:** wet slate `#3a4450`, rain `#8fa3b5`, sodium orange `#e8963a`, Clyde grey `#566270`.
- **Skin and people:** three skin tones, plus reds, plaids, denim, orange (blaze-orange hunters' jackets).
- **Accents:** blaze orange `#ff6a1a`, Christmas red/green/yellow, brass/gold `#c9a227`.

### The cast (sprites about 16–24 px tall; one close-up portrait each, about 48 px, for key moments)
- **Dean Koski** (29): square jaw, **a big mustache he's extremely proud of**, a grin; open-face helmet with a hand-painted checkered stripe; later a letterman jacket and a Detroit-style blue-and-silver stocking cap (no logo).
- **Tommy Koski** (26): thin, **wire-rimmed glasses taped at one hinge**, a clipboard strapped to his thigh, a sensible parka.
- **Ike Koski** (58): barrel-chested, **silver crewcut**, huge hands, **red-and-black buffalo-plaid Mackinaw**; a **shiny burn scar on the back of his left hand** (it matters in the Texas and Scotland scenes).
- **Bev Lahti** (56): diner owner, a fake-fur parka hood, a clipboard.
- **Arnie Perttula** (70s): leaning out of an old pickup with a CB mic.
- **Gary Voss** (34): feathered hair, a satin team jacket, leaning on a **white Mazda RX-7, #1** (draw it as a generic low wedge coupé).
- **Harlan T. Bowie** (57): big man, **white Stetson, white suit, ostrich boots, a gold Texas-shaped bolo tie**. **Connie Bowie** (48): sharp, blonde, a turquoise pantsuit, a tray of margaritas.
- **Tiny:** Harlan's driver, refrigerator-sized, black Stetson, mirrored aviators.
- **Mrs. McGlashan** (70s): four foot eleven, cardigan buttoned to the throat, glasses on a chain.
- **Ali Guthrie** (44): sheepskin car coat over a shiny suit, a gold chain.
- **Davie Baird** (19): skinny, bad haircut, black T-shirt under oil-black overalls with sleeves too long.
- **Rab McCallum** (56): **built like a fire hydrant**, a grey beard "more of a threat than a beard", scarred tattooed forearms (a thistle; "MAGGIE" crossed out), blue eyes.
- **The crow:** black, on a stump, judging. **It appears in the first and last shots of the Michigan section** and leaves every time something loud happens.

### The cars (side view, ¾ view where needed, about 32–48 px long)
- **Loretta, the Datsun 510:** a boxy little 1970 two-door saloon, **pale yellow where it isn't mud-brown**, **a bar of four big round lamps** on the front, "KOSKI SERVICE & REPAIR, CALUMET, MICH." hand-lettered on the doors (a few unreadable pixels is fine at this size), **a big crooked #7**. Animate: suspension bounce, a gravel rooster tail, lamp beams.
- **The Renault 5 Turbo:** a cheeky little hatchback at the front that **swells behind the doors into enormous flared rear arches** ("a bulldog under a bedsheet"), side air intakes, **plain white**, a few numbers taped on the doors. No badges.
- **Supporting:** Harlan's **white Cadillac convertible with six-foot longhorns on the bonnet**; a **red CJ-7-style jeep with no doors** and a Texas flag on a whip antenna; Ike's **'74 Pontiac with a coat-hanger aerial**; a **small silver Vauxhall Cavalier** (generic boxy hatch); **Rab's battered Bedford van** with a trailer; Ali's dark **Jaguar saloon** (generic, no badge); a white airliner; pickups with headlights on.

---

## The scenes

Times are targets; adjust so the whole thing breathes. **Lines in quotes are from the script: use them as written (trimmed is fine), in speech bubbles.** Text cards are full-screen, white on dark, in the bitmap font.

### 0. Title (0:00–0:08)
Black. Pixel snow falls. **"GRAVEL & GLORY"** builds letter by letter, then **"EPISODE 1: IF BUFFUM CAN DO IT"**. A distant engine screams.

### 1. Briggs Grade, the cold open (0:08–0:20)
- **Card:** "COPPER COUNTRY PRO RALLY. KEWEENAW PENINSULA, MICHIGAN. NOVEMBER 1982."
- Dusk, a sky "the colour of an old nickel", birch and hemlock, the first snow only on the crown of the gravel road. **A crow on a stump.** The engine gets louder; the crow leaves.
- **Loretta comes over a crest with all four wheels in the air**, lamps blazing like a locomotive, lands with a crunch (camera shake), throws a rooster tail of gravel and is gone. **The crow comes back. Thinks better of it. Leaves again.**

### 2. In the car (0:20–0:34)
Cut-away interior (two portraits side by side, the road rushing in the windscreen).
- Tommy: **"Hundred. Four left over crest. Don't cut. Stump inside."** / Dean: "Heard ya." The car crests. Dean cuts. **BANG** (screen shake). Tommy: "That was the stump." / Dean: "That was a pinecone."
- Tommy, checking the stopwatch taped to the dash: **"We need to take eleven point four seconds off him on this stage to win."** / Dean: "Eleven seconds. That's nothing. That's a sneeze."
- **Quick cut to the finish clearing:** pickups nose-in with headlights on, a burn barrel, forty locals in blaze orange; **Ike** with a stopwatch, **Bev**, **Arnie** on the CB ("they went by sideways, over."). Ike: **"Sideways is good. Sideways is fast."** Across the clearing **Voss** taps his watch: "Eleven seconds, Ike!"

### 3. Hautamaki Creek (0:34–0:46)
Tommy: "Water splash… It's deep." / "How deep?" / "I don't know, Dean, I didn't bring a yardstick. Deep." **Loretta hits the ford at sixty: a wall of brown water** over two marshals in a jeep. Soaked marshal, deadpan: **"Well, that's a Koski."** The engine **coughs, the lamps flicker**; Dean: "C'mon, Loretta." It catches. Tommy: "That cost us three seconds." / Dean: "Then I'll find five."

### 4. The deer (0:46–0:56)
Dean: "You know what John Buffum would do right now?" / Tommy: **"Five right! Don't believe, TURN!"** Then: **"DEER!"** A white-tailed doe in the lamps, looking personally offended. Dean: "Where's that in the notes?!" / "It's a DEER, Dean, they don't come in the NOTES!" **A Scandinavian flick: Loretta pendulums left-right around her with eighteen inches to spare.** The doe chews. Dean: **"I love this sport."** / Tommy: "I'm gonna throw up in my helmet."

### 5. Dad's Hump (0:56–1:14), the centrepiece
- Tommy: **"Crest into big jump. Caution. CAUTION. That's Dad's Hump."** Both go quiet. Dean: "How fast did Dad take it in '61?" / "Too fast. He ended up in a tree." / **"Then we take it a little faster than too fast."** Dean shifts up.
- A crest lit by spectators' trucks, thirty people well back, **a hand-painted sign nailed to a birch: "IKE'S HUMP, EST. 1961. SLOW DOWN DUMMY."**
- **Loretta flies.** Go to **slow motion**: a long flight across the screen, the crowd's cheer rising, snow flecks frozen in the air. A cut-in shows the cabin in mid-air: Dean, eyes wide, joyful; Tommy reading to a car that isn't touching the planet: **"…land… hundred… flying finish…"**
- **It lands: the rear bumper takes a bite of Michigan. Sparks.** It keeps going.
- *(This is the scene to export as the GIF, about 6 seconds.)*

### 6. One point nine (1:14–1:40)
- The finish: Ike clicks his stopwatch. **"I got it by two."** / Bev: "Two what?" / "Seconds, Bev." Voss: **"Your watch is from a cereal box!"** / Ike: **"My watch is from Sears, Gary, and it's never been wrong in its life!"**
- The stop control: steam, one headlamp pointing at the sky; a teen marshal in a purple snowmobile suit takes the time card. Tommy does the sum twice; his eyes go wide. **"We won the Copper Country by one point nine seconds."** Both brothers **scream** in their helmets; the horn goes off; the marshal drops her pencil.
- **Ike** runs up the road, hauls Dean out and **bear-hugs both sons off their feet**, a beer can still in his hand, spilling down Tommy's back: **"My boys! MY BOYS!"** Bev kisses Tommy's cheek (a lipstick pixel). Ike: **"You went flat over the Hump… You beautiful maniac."**
- Dean: "Hey Gary! Snowmobiles, huh?!" A road flare; **a bottle rocket goes into a tree.** Tommy alone for a second, hand on Loretta's roof "like a horse that just won the Derby".
- **Last shot: the crow on the stump, observing all of it with contempt.** Smash to the title again, briefly.

### 7. Calumet (1:40–2:02)
- **Fifth Street, morning:** red sandstone fronts, snowbanks, a bedsheet banner spray-painted **"CONGRATS DEAN & TOMMY!! COPER COUNTRY C…"** (the misspelling is in the script; keep it). A snowplow honks twice; an old woman bangs a pot from a window.
- **The Copper Kettle Diner:** fogged windows, booths, **the cheap trophy on the counter with a pasty placed next to it, "as if feeding it."** The diner applauds the brothers. Tommy's notebook: prize money minus everything = **"Negative a hundred and ten dollars"**; Ike: **"…and the best night of my life!"** The newspaper: **"LOCAL BROTHERS FLY TO RALLY WIN."** On the corner TV, a white four-wheel-drive rally car sliding through European snow; **the brothers look at each other.**
- **The garage party, night:** **Loretta on the lift in Christmas lights with a paper crown**, crockpots, a boombox, three men singing badly (your tune). In the office, Tommy's fat file: **"WRC 1983. EST. BUDGET."** Dean's soft old clipping: **"VERMONT'S BUFFUM TAKES ON EUROPE'S BEST."** Dean: **"If Buffum can do it, the Koskis can do it."**
- **Ike on a creeper under the pastor's van.** Tommy: "We want to run the World Rally Championship next year." **Ike laughs, a big belly laugh**, then sees their faces and stops. He rolls back under the van and stares at the frame, lips moving: **he's doing the math.**

### 8. Mary, and the terms (2:02–2:24)
- **Lake View Cemetery, dawn:** white and blue, Lake Superior grey beyond. Ike brushes snow off a simple stone: **"MARY ANNE KOSKI. 1929–1979. BELOVED."** A frozen plastic poinsettia in a coffee can. Bubble: **"The boys won the Copper Country. You probably heard."** The boys stand behind him; Tommy cleans his glasses.
- **McLain Beach:** grey water, ice plates grinding at the shore, three men in parkas walking. Ike: **"Terms. My terms."** Four text cards, one by one, each with a small pixel icon:
  1. **"Safety's mine."** (a helmet)
  2. **"Tommy runs the money. When the money runs out, we come home."** (a coin)
  3. **"Nobody drives angry."** (a red steering wheel, crossed)
  4. **"If I say we pull out, we pull out."** (a flag lowering)
- Ike pulls both sons into a huddle. **Dean whoops; it echoes off the lake.** Tommy wipes his eyes "and pretends it's the wind."

### 9. The phone call (2:24–2:36)
Split screen. **Left:** Ike in the tiny office, a rotary phone, an old address book open at B (**HARLAN BOWIE**, two numbers crossed out), the dial clicking back slowly; behind the clock, an old photo: **two young men beside a burned-out car, "ALBERTA '64. NEVER AGAIN."** **Right:** a tower office over a flat brown plain of nodding pumpjacks; Harlan, boots on the desk, takes his hat off when he hears the name. **"Ike Koski, you sorry sonuvabitch! Eighteen years!"** … **"Get your boys on a plane."** After he hangs up, Harlan takes **the same Alberta photo** out of a drawer. His hand isn't steady.

### 10. Texas (2:36–3:02)
- **Midland, 81 degrees in December:** the Koskis on the airstairs in full Michigan winter gear, stopping dead in the heat (heat-shimmer dither). **Tiny** with a sign, "KOSKI (MICHIGAN)", and **the white Cadillac with longhorns on the bonnet.** Three pale Michigan forearms "glowing like fluorescent tubes" in the back seat.
- **The ranch:** a white hacienda, **a swimming pool shaped like Texas**, a helicopter. **Harlan lifts Ike clean off the ground in a hug.**
- **The bourbon test** (a top-down view of a scraped dirt figure-eight round two windmills and a stock tank, hay bales, ranch hands on the fence): Harlan in the jeep's passenger seat holding a **crystal tumbler filled to the brim**. Show the **meniscus** in a little inset that wobbles with every bend. Tommy at the window: **"Pretend there's an egg on the gas pedal."** Dean drives smooth; a cow wanders on; he glides round it; **at the fence, Tommy's lips move: he's calling the notes from a hundred yards away.** On the last bend Dean can't help it: **a big lurid slide**, a wall of dust over the fence and Connie… **and Harlan drinks the whole tumbler in one gulp.** The jeep stops. **He holds up the glass: empty.** The ranch hands roar.
- **Dinner** (one shot): Tommy's notebook: **"+$390,000"** and, very small, **"Holy wah."** Harlan's one condition flashes as a card: **"BOWIE OIL, big as it'll fit… on the roof so the helicopters can read it."**
- **The veranda, night:** stars "stupid out here", the Texas pool glowing. Two old men in rocking chairs. **Close on Ike's left hand: the shiny burn scar**, seen for the first time. No dialogue; just crickets.

### 11. Which car? (3:02–3:16)
- **The war room:** a map of Europe pinned over the calendar; index cards in a grid; **the cards get X'd out one by one** with comic sound effects and short cut-ins: Audi's receptionist ("please stop calling, I'm a grandmother"), a Turin telex ("good fortune… in the wild west"), Tommy at the diner pay phone with quarters, **the TOMMY'S EUROPE FUND jar with nine dollars in it**, Ike in a bathrobe: "You're the politest fella I ever talked to… So that's a no?" Cards to cross: QUATTRO, 037, ASCONA, CELICA TCT, 240RS, ESCORT. **One is left: RENAULT 5 TURBO.**
- **The roof:** the brothers on the snowy roof wrapped in quilts, the town's Christmas lights below and the dark mine shaft-house against the stars, big slow flakes. Dean: **"You're the reason I'm fast."** Tommy leans his head on his brother's shoulder for a second. Then he pulls out **the classified ad, circled in red: "FOR SALE: RENAULT 5 TURBO… GUTHRIE MOTORS, GLASGOW."**

### 12. Scotland (3:16–3:44)
- **Prestwick, 3:40 p.m.:** rain going sideways (diagonal pixel streaks), the Koskis soaked on the airstairs.
- **The Cavalier:** Dean opens the left door: **no steering wheel.** Tommy gets in the right: there it is. **They swap sides in the rain, passing at the bonnet.**
- **The roundabout (top-down):** the little silver car goes round **once, twice, three times**; a lorry blares; **a small elderly man at a bus stop raises a hand in a small, sad wave** each time it passes.
- **Mrs. McGlashan's B&B:** the rules scroll in a bubble ("Breakfast is at half seven. No' twenty-five tae eight."); a coin into the gas-fire meter, the fire glows… and goes out. **Breakfast:** Dean looks at the haggis, eats a piece, **his eyes widen.**
- **Guthrie Motors:** Ali with his arms spread, **"The Yanks! The Yanks are here!"**; the back workshop, a dusty cover on "a bulldog under a bedsheet"; **the cover comes off: the white Renault 5 Turbo**, those enormous rear arches, a light chord in the music. **Dean folds himself into the seat; Tommy squeezes in beside him; their shoulders touch; Tommy adjusts the map light and it fits his hand exactly.**

### 13. Rab (3:44–4:00)
- **The arch in Dumbarton:** a Victorian railway arch with **blue wooden doors** and a weathered sign, **"R. McCALLUM. MOTOR ENGINEERING. RALLY PREP. NAE TIMEWASTERS."**, the idle shipyard cranes on the Clyde beyond. **Davie**, sweeping puddles, sees the car, brightens, **trips on the broom.**
- Inside: every tool hanging inside its own painted white outline; an Escort shell on stands; legs sticking out from under it. Rab rolls out: **"Get oot."** Dean: "…if John Buffum can come over here and run with the best in Europe, we can too." / Rab: **"Ah don't care if John Buffum's the Second Comin' o' Christ in a fireproof suit."**
- **Ike crouches and runs his thumb along a weld on the cage.** Rab watches him. **Then Rab's eyes drop to Ike's scarred hand.** Close on both men's faces: two old men who know what a burning car sounds like. Rab: **"Ye want me? Ye'll show me. Tonight. The Rest an' Be Thankful."**

### 14. The Rest and Be Thankful (4:00–4:20), the ending
- **A Highland pass at night in the rain:** hairpins stacked up the glen like a folded belt, a knee-high stone wall, darkness below. Rab drives the Renault up slowly; **Tommy writes notes with a torch in his teeth**, his hand writing by itself while he watches the road. Rab: **"Ye've done this before."** / Tommy: **"Every day of my life, Mr. McCallum."**
- **The start line:** two oil drums, **Davie with a torch and a stopwatch**, the Bedford gone up to the summit. Ike at the window: **"Rule three."** / Dean: "I'm not angry, Dad." / **"I know. I'm reminding you so you don't get happy either. Happy's worse."**
- **He slaps the roof twice.** The turbo spools; a pause; then the little white car launches, and **its tail lights climb away up the hairpins into the dark**, getting smaller, switching back and forth across the screen.
- **Hold on the empty start line, the rain and Davie's torch. Fade out.** Final card: **"GRAVEL & GLORY. EPISODE 1."** (The episode's text I gave you ends on that launch. **Don't show their time or what Rab says at the top.**)

---

## Style notes
- Keep it **readable at a glance:** one idea per shot, clear silhouettes, big contrast between the warm (lamps, diner, Christmas lights, Texas) and the cold (forest dusk, the lake, Scottish rain).
- **Comedy is in timing:** hold a beat before the punchline frame (the empty tumbler, the third lap of the roundabout, the crow).
- **The emotional beats get stillness:** the cemetery, the roof, the veranda scar, Rab looking at Ike's hand. Slow the music, cut the sound effects, let the snow or rain do the moving.
- Camera: pans, gentle parallax (3 layers) in the driving scenes, a little screen shake on landings. No zooms that break the pixel grid (scale only by whole numbers, or cut to a pre-drawn close-up).
- Section music: **Michigan** (bright, brave, a bit country), **Calumet** (cozy), **the lake and the cemetery** (slow, three notes), **Texas** (big, swaggering), **Scotland** (rain on a minor key, then a hopeful turn when the cover comes off the Renault). All original.

---

## Build order (stop and show me after each)
1. **Engine and look:** canvas, scaling, timeline, controls, bitmap font, speech bubbles, palette; scene 0 and scene 1 only. Screenshot it.
2. **Michigan:** scenes 2–6 with Loretta, the crow, the deer and the Hump in slow motion. This is the test of whether the style works.
3. **Calumet and the terms:** scenes 7–8.
4. **The call and Texas:** scenes 9–10.
5. **The car hunt and Scotland:** scenes 11–14.
6. **Sound pass**, then **the export script**; render the MP4 and the Hump GIF, check them frame by frame for flicker or blur, and tell me the file sizes and running time.

**Checks before you call it done:** the film plays start to finish with no console errors; every quoted line is spelled as above; no real logos, no real people's faces, no borrowed melodies; text is readable at 1080p; the export is deterministic (render it twice and compare a few frames).
