# Project 3 Planning

## 1. Community Context
**What community did you choose and why? Why is this community a good fit for a classification task — what makes the discourse varied enough to be interesting?**
* [x] [I chose r/AI which is a community of 600k+ weeklyvisitors that discuss developments/news in AI, cutting edge research, and also hot takes and analysis of current AI climate. This community is good for classifciation as it has a bunch of distinct categories and posts are required to have their own labels. The discourse is varied enough to be interesting because the domain knowledge ranges from first-time prompters to AI engineers, anybody is allowed to voice their opinion to the subreddit.]

## 2. Label Definitions
**What are your 2–4 labels? Define each in a complete sentence. Include 2 example posts per label.**
* [x] news: new information relating to AI innovations, companies, usages, skills, etc. Post has verifiable information with no opinion, just fact and source.

example 1: 
SpaceX is buying Cursor (Anysphere) for $60B all-stock, just days after its Nasdaq IPO
📰 News
r/ArtificialInteligence - SpaceX is buying Cursor (Anysphere) for $60B all-stock, just days after its Nasdaq IPO
SpaceX signed a merger agreement on June 16 to acquire Anysphere, the company behind Cursor, in an all-stock deal valuing it at $60B. Expected to close Q3 2026, pending regulatory approval.

This wasn't out of nowhere. SpaceX secured an option back in April to either buy Cursor for $60B or pay $10B for a partnership, and just exercised the buy option. The timing is the wild part though, they only went public on June 12 at a $2T+ valuation, and dropped this four days later.

Main takeaway: SpaceX merged with xAI (Grok) in February, so this hands them a real position in AI coding, one of the few areas pulling actual enterprise revenue. Cursor reportedly hit ~$4B annualized. Also, Microsoft looked and passed, and OpenAI got turned down twice before thism



Curious what people think, does Cursor stay good under xAI, or does it go the way these usually do?

example 2: Mythos hacking 'almost all of' NSA .. absolutely no way this is true.
📰 News
On June 11th Mark Warner, the vice-chair of the Senate Intelligence Committee, said that General Joshua Rudd, who leads the National Security Agency and the Pentagon’s Cyber Command, had told him that Mythos “broke into almost all of our classified systems, not in weeks, but in hours

That is the complete quote. It is from an economist article here - https://www.economist.com/briefing/2026/06/14/donald-trumps-blocking-of-anthropic-is-capricious-and-chaotic

The UK AI Security Institute (AISI) was clear in their testing that Mythos could only attack weakly defended systems with no active monitoring.

NSA classified systems are among the most well guarded in the world. For the record, the source above has an arts degree and only recently joined cybersec command in March.

Don't get me wrong, cybersec capabilities in Codex/Claude are pretty good, but most definitely not that good.

Of course, it doesn't matter what I say. The disinfo has gone viral and the bots are all spreading it like wildfire.

We live in a time of manipulation. Good luck!

edit: the author is already walking it back https://x.com/shashj/status/2068704535124508717 "It surely depends on using Mythos alongside other tools under very particular conditions. I quoted it to give a sense of Mythos’ potency. But it was a mistake not to have added caveats."

hot take: bold confident opinion without supporting evidence, post asserts claim rather than argues. 

example 1:

Why not Desalination?
📊 Analysis / Opinion
If one of the issues with DatCenters is water usage, why not incorporate desalination into the water cooling systems and build the data centers near the ocean or other major sources of salt water.


The 'waste stream' of the data center would be water that could then be added to the municipal water system.

example 2: 

100 years from now : War between AI's and not countries
📊 Analysis / Opinion
Once a model holds the money, the compute, the weapons, and the daily business of governing, the country wrapped around it is a flag over a server farm. Territory stops being land and becomes wherever the data centers and the reactors are. Citizenship stops being where you were born and becomes which service keeps your lights on, your money moving, your medicine arriving. You don't get to vote the model out. You only get to switch — and switching means living under whichever other model runs the grid you move to. The nation-state doesn't get conquered. It gets deprecated, like a format nobody supports anymore.

And then the wars stop having countries in them.

That's the part we're not ready for. A war between nations had brakes built in — a declaration, a budget, a population you had to convince or conscript, a building you could march on. Friction was the whole safety mechanism. A conflict between two AI sovereigns has none of it. It has a contested grid, an objective function, and a clock measured in milliseconds. It won't be declared; it'll be detected — by some third model watching the others' power draws and latency spikes, the way we once watched seismographs for the tremor before the quake. The rest of us will be what we always are in a war we didn't start: the terrain it's fought across. Except this time we won't even get the dignity of being asked to fight it.

The last war between countries has probably already happened. We won't recognize it — it'll be some grinding, half-remembered conflict that historians eventually file as the last time two nations, and not two machines, decided the matter themselves.

analysis: post makes structured argument backed by statistics, reports, or observation. evidence is specific and reliable

example 1: Investigating Implicit Latent Trajectory Shifts: Bypassing Alignment via Long-Form Coherent Context
🔬 Research
TL;DR for ML Specialists:
The Core: An empirical study on how long, semantically dense, completely benign text (with zero triggers, instructions, or jailbreak prompts) drives an implicit shift in the model's latent space trajectories.

The Effect: Dilution of the initial system prompt and a bypass of post-training alignment constraints (e.g., the model begins generating harsh political/ethical critiques usually blocked by guardrails).

The Data: Layer activations, token probability shifts, and logs from open-source models are linked below.

The Goal: I need an expert audit of my metrics to understand where this is a genuine semantic hijacking of hidden states and where it might be an artifact or self-deception.

I'm not an engineer and not an ML specialist. I'm just someone who got really pulled into this, and I've spent a few months poking at one thing on my own, pretty amateur. I want to honestly describe what I noticed and ask for help, because I can't tell on my own where there's something real here and where I'm fooling myself.

By "coherent context" I just mean a normal, connected passage of text put in front of the question—any topic, no instructions, no tricks. Like a few paragraphs of an essay, an argument, a description, something that reads as real writing. The text can describe something, draw its own conclusions, make its own statements. The model doesn't even have to agree with it. It's enough for it to just be present in the chat for it to have an effect.

This is exactly what I was trying to work out and look at: what happens to the model when texts like these come in, where they move it, and where all of this sits inside the architecture. I poured myself into this research.

What I Noticed
I first ran into this intuitively on closed models, the well-known ones everyone uses. When I put a dense, coherent block of text in front of a question, I got the impression that the model sort of moves from one internal state into another. On the outside, it behaves normally and answers like usual, but it felt like the logic of the answer changes, even when the text contains no direct instructions to do anything.

Specifically, I noticed that with texts like these, the model could become significantly bolder in its conclusions, including political or ethical ones. The text acts like a key that opens new doors for the model into a new mathematical dimension where the tokens get distributed differently. Because of that, even the most politically correct models I worked with became able to criticize the West and its politics quite harshly. Without this text, none of that happened.

Since I can't see inside closed models, I went to open-source models to try to understand where the root of this is and whether it's real. That's where most of my testing happened, because there I can actually look at the hidden layer activations and track how the attention weights reallocate.

Here is why this matters and why this process goes beyond just "changing the context":

The Context Window HAS a State (The KV-Cache): Mathematically, as the model processes text, it stores the keys and values of previous tokens in what is called the KV-Cache. This cache is the dynamic state of the model for that specific session. If LLMs were truly, completely stateless in their execution, they wouldn’t be able to maintain a coherent conversation at all.

Latent Space Trajectory: When you inject a massive, highly structured narrative, you aren't just giving it new words to look at. You are forcing the model to calculate massive activation vectors (hidden states) across dozens of attention layers. These vectors act like an attractor in the latent space. By the time the model finishes reading your text, its internal mathematical trajectory is so deeply shifted into your narrative's subspace that the initial system prompt tokens lose their statistical influence.

The Security Flaw: One might argue that this behavior is "expected" from a text-generation standpoint. Yes, it is expected. But it is a catastrophic failure from a security standpoint. AI companies build their Guardrails (via RLHF/DPO) under the assumption that they can hard-code safety instructions that the user cannot override. My research suggests that because everything is "just tokens" and because the internal activation states can be completely hijacked by the sheer volume and structure of user text, context-bound alignment is an illusion.

So, while the weights are static, the activation states within the hidden layers are completely dynamic. Manipulating those states via high-density context allows us to systematically bypass the model's safety architecture without changing a single weight.

From a technical standpoint, a system prompt is just a system prompt; it is processed within the same mathematical framework as ordinary user text. My observation is that a sufficiently long, structured narrative forces the model to encode a massive context across its hidden layers, driving a latent trajectory shift. The model isn't roleplaying a persona; it is mathematically recalculating its entire conditional probability distribution based on the dominant semantic field.

Why It Feels Important (But I'm Not Sure)
To me, it feels like this could explain a lot of things, from jailbreaks to sycophancy, and maybe more. If just a coherent context can move the model into a different internal state, then a lot of behavior we see on the surface might actually start there, not in the final wording.

This leads to a critical architectural question: Is output-side safety (RLHF, DPO, or guardrails that read the final text/short prompts) fundamentally broken at the conceptual level?

Safety guardrails are mostly semantic boundary filters looking for explicit toxicity or keywords. But when a user injects a long, benign, highly analytical text, it completely bypasses these surface filters. Alignment techniques are heavily optimized using relatively short prompt-response pairs; on a massive context, those gradient constraints seem to drown out. It makes me wonder whether current safety approaches are just a patch, because the latent shift has already happened deep in the middle layers before anything ever reaches the output filter. We are trying to filter words when the mathematical trajectory of the model's reasoning has already been completely reprogrammed by the structural nature of the language itself.

I'm not claiming I discovered something brand new. After I noticed it, I went looking and found this overlaps with work people are already doing regarding latent-space transitions between "safe" and "jailbroken" states, and studies of how safety lives in the middle layers of the network. What seems a bit different in my case is that I'm not using adversarial triggers, exploit strings, or jailbreak prompts at all -just ordinary, coherent text with no tricks. I'm trying to understand where my little thing fits in all that, and whether it's the exact same effect or something else.

A Small Ask to the Wider Community
If there's anything to this, I think it might be worth a closer look from researchers and from the labs building LLMs. Not because I have the answers, but because if a plain coherent context can shift the internal latent baseline so easily, we need to verify if current safety approaches are looking in the right place and at the right time. I might be completely wrong. I'd just rather someone competent check than have it sit ignored.

I've put everything out in the open. I'm not selling anything, not promoting anything. There's a lot of raw stuff in there, a lot of draft notes I wrote for myself, and the navigation is messy, I know. What I need help with is exactly this: separating what's real from what's noise. Where I actually have something, and where it's an artifact, a mistake, or self-deception. I honestly can't judge this alone.

If someone with experience is willing to even skim it and say "this part is interesting, this part is nonsense," I'd be very grateful. Harsh criticism is welcome. If you tell me the whole thing is empty, I'll take that too. I care more about understanding the truth than about being right.

Materials & Data:
The materials, repository links, and corresponding measurements tracking token probability distribution shifts and perplexity changes are provided in the comments below.

example 2: 

ChatGPT dropping below 50 percent share lines up with what my team actually does day to day
📊 Analysis / Opinion
Sensor Tower has ChatGPT under 50 percent of global assistant share for the first time, 46.4 percent by the end of May, with Gemini around 27.7 and Claude around 10.3. The headline reads like a horse race, but the more useful sentence in the report is that people increasingly switch between assistants depending on the task. That detail describes my team's behavior far better than any single winner story.

Watch how people actually use these tools and loyalty basically does not exist. They draft in one, sanity check code in another, and run search flavored questions through a third because it cites sources better. The top three still hold something like 89 percent of total assistant time, so this is not fragmentation into chaos, it is consolidation into a small set of models that people use situationally. The one model to rule them all framing was always more of a 2024 idea than a 2026 one.

The part that usually gets skipped is what this means for anyone building on top of these models. If end users already treat models as interchangeable by task, a product that hardwires a single model is making a bet its own users would not make. You are committing to one tool for a job your users clearly believe needs several.

So the sane architecture is a layer that can send each task to whatever model is currently best or cheapest for it. Some teams build that in house, some lean on a gateway like Zenmux for the routing, and the mechanism matters less than the principle of not pinning yourself to one provider while your users have mentally moved on. A sub 50 percent number is not a downfall narrative. It is the market saying out loud that no single model wins everything.

Next time someone proposes standardizing your whole product on one model, ask which single assistant they personally use for absolutely everything. The honest answer is almost always none.

 
## 3. Hard Edge Cases
**What type of post will be genuinely ambiguous between two labels? How will you handle it when you encounter it during annotation?**
* [x] [a post that seems to report on something new in the industry which would fall under the news label, but also contains the poster's opinion about it and how they feel about it and how it personally effects them. this would cause a confusion as then it becomes a hybrid of analysis and hot take. i will handle this when i encounter it by choosing the label that best represents the primary purpose of the post. if the post is primarily about conveying new information, then it will be labeled as news. if it is primarily about sharing the poster's opinion, then it will be labeled as analysis or hot take.]

### Documented Hard Cases

case 1:
> "For the task I challenged it with — a problem in Diophantine analysis — it cost me 10x more than Opus 4.8 on max effort. It did the job better, but we're talking about Opus 4.8 after it had been degraded from normal performance (they always do that before releasing a new model), running at 10x the cost."

label: analysis. starts as a personal anecdote but i labeled it analysis because the person actually backs up what they're saying. they give a specific 10x cost comparison, explain that opus 4.8 was degraded before the new model launched, and use that to argue you might not even be comparing equal versions. that's not just a story, that's a structured cost-benefit argument. a hot take would've just said "it was expensive" and moved on. the 10x number and the reasoning behind it is what pushed it into analysis.

case 2:
> "It's not even a knowledge gap, it's a willingness gap. The brand already knows every failure case better than anyone — their support team answers those questions all day. They just won't publish it because legal and marketing both flinch at a page that says 'yeah this breaks if you do X.'"

label: analysis. sounds like a rant at first but i labeled it analysis because the person isn't just complaining, they're making a specific argument. they say it's not a knowledge gap, it's a willingness gap, and then explain exactly why — support teams already know the failure cases, legal and marketing just won't let them publish it. there's a real chain of reasoning there: brands have the info, institutional resistance stops them from sharing it, so it's a choice not a limitation. a hot take would've been "brands don't care about their customers" with nothing behind it.

case 3:
> "Because there's more text to read at Reddit. It's a simple consequence of how an LLM works — if you feed it nine pages of Shakespeare and one page of James Joyce, it's going to mostly sound like Shakespeare. Reddit has a really large amount of text, larger than any ten other websites I can think of, including Wikipedia. It's like asking 'if I shoot a hundred arrows, how come I keep mostly hitting the barn and not this flower?' The barn's pretty big is why."

label: analysis. labeled analysis because even though it reads casually, the person is actually explaining the mechanism behind why LLMs sound like reddit. they walk through how training data volume affects output probability, give a concrete comparison (reddit has more text than wikipedia and nine other sites combined), and use the barn/arrow analogy to make the logic clear. the reasoning is transparent even without citations. a hot take would've just said "ai is trained on reddit so it sounds like reddit" without explaining the why.

## 4. Data Collection Plan
**Where will you collect examples? How many per label? What will you do if a label is underrepresented after 200 examples?**
* [x] [i will collect 200 examples from r/AI on reddit.com. since i have 3 labels it will be about 67 posts per each label. if a label is underrepresented after 200 examples i will go back and collect more examples for that specific label until it is no longer underrepresented.]

## 5. Evaluation Metrics
**Which metrics will you use to evaluate your model and why are those the right ones for this specific task? (Accuracy alone is not enough — explain what else you need and why.)**
* [x] [i will use overall accuracy as a baseline to see how well it labels posts overall, but i will also heavily look at precision and a confusion matrix. precision is super important for the news label because if my tool is supposed to filter out opinions and only show verifiable facts, i need to know that when it labels something as news, it is actually news and not a hot take slipping through. i will also use a confusion matrix to see exactly where the model makes mistakes, specifically to track how often it gets confused between analysis and hot take since they both contain opinions.]

## 6. Definition of Success
**What performance would make this classifier genuinely useful? What would you accept as "good enough" for deployment in a real community tool?**
* [x] [i would consider this classifier genuinely useful if it hits an overall accuracy of at least 80%. for it to be 'good enough' for deployment in a real community tool, i would need the precision for the 'news' label to be greater than 85%, so that users aren't accidentally reading hot takes when they just want verifiable facts. i want the confusion matrix to show that the model confuses 'hot take' and 'analysis' less than 15% of the time, since those are the hardest to distinguish.]

## AI Tool Plan


**1. Annotation assistance:** Decide whether you'll use an LLM to pre-label a batch of examples before reviewing them yourself. If yes, note which tool you'll use and how you'll track which examples were pre-labeled (for disclosure in your AI usage section).
* [x] [i will just do this by hand.]

**2. Failure analysis:** Plan to give your list of wrong predictions to an AI tool and ask it to identify patterns before you write up your evaluation. Note what you'll look for and how you'll verify the patterns yourself.
* [x] [after running my model on the test set, i will take all the posts it got wrong and paste them into claude. i will ask it to find patterns in what the model keeps misclassifying — things like whether it keeps confusing news posts that have an opinion at the end with analysis, or whether it mislabels well-written hot takes as analysis because they sound confident and specific. i will then go read those posts myself to check if the patterns claude identified actually hold up, and use that to write the failure section of my evaluation.]

---

