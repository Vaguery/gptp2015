{::comment}
kramdown -o latex markdown/session.md > markdown/session.tex
kramdown -o latex markdown/manuscript.md > markdown/manuscript.tex
{:/comment}

# Why

More than a decade ago, Rick Riolo, Bill Worzel and I were working on a consulting project together that involved evolutionary algorithms and genetic programming. As we were chatting one day, Bill asked Rick what he'd most like to see as part of the research program of GP "moving forward". Rick's answer informs this contribution, as well as much of my professional work with GP in the years since.

As I recall, Rick answered that he'd like to better understand the "symptoms" we often see when we run an evolutionary search process: premature convergence, failure to improve, catastrophic lack of diversity, and the sense when we look at results or ongoing runs that some particular choices we've made setting the many parameters _just aren't quite right_. There are numerous well-written papers suggesting ways around local minima, proposing reasonable and exotic search operators, and running "horse races" between variant algorithms on benchmark problems. But years later, I take Rick's challenge not only to mean that it might be useful to have a benchmarking catalog which lists conditions in which search operator $$X$$ acting under contingency $$Y$$ tends to produce outcome $$Z$$, but also that _the things we identify as symptoms themselves_ are poorly understood, to the point that we think of them as "subjective".

Somewhere during that same project, I remember a Project Manager telling the story that when domain expert customers were shown a collection of Pareto-optimal solutions to the problem being explored, they were confused. "We don't want a choice, we want _the best_." This came after months of analysis, interviews, and a collective agreement that the problem was fundamentally multi-objective. But when the decision-makers were faced with the unquestionably successful results they had collectively specified, _those results weren't right enough_.

In this contribution we must face the fact that such a response is inevitable: not just from silly lay "customers" in an application project, but from ourselves; whether the project is "theoretical" or "practical"; whether it is "small" or "large". Projects resist our initial plans.

The "field" of GP has grown quite a bit in the intervening 15 years since these two anecdotes. All of us who follow it closely have played a role in the expanding front of new methodologies and techniques---the extraordinary ones we describe in our little workshop, and also the newsworthy and admirable public successes.

But this acceleration of practical successes has been accompanied by a dilution of the scope of the theoretical warrants we use to justify them: Most examples of "practical success" involve unique domain-specific quirks, each implementation makes numerous contingent (and sometimes arbitrary) design and architectural choices, and even the ontological foundations of "individual", "fitness", "behavior" and "population" start to get sloppy around the edges whenever we actually _look_ at What People Actually Do to Solve Problems With GP.

While this can be disappointing for those of us who follow the progress of theoretical work closely, it has more serious consequences. The lack of interest in GP among statisticians, planners, designers and mathematical programming aficionados is, I argue, not just a matter of disciplinary border wars. I suspect rather that skepticism arises from our own inability to say what's _happening_ in any particular GP run---_even when we look_. Nor for that matter can we be very helpful in explaining _why_, or what to do in response to any given contingency. We have learned that _more CPU time_ can sometimes get us past a tricky stage of "not working", but any project that lives very far from the bounds of the last century's version of "genetic programming" is unexplored territory.

This is not the fault of GP as a field, nor of our theoreticians and engineers. Rather I'll argue that a deep philosophical misconception presides over the work, both within and outside our field. This misconception undermines our understanding of what GP is _doing_, of the way it unfolds in theory and in practice, and even what it's _for_.

I will make my case here in the form of an exercise, or _kata_. This is not intended to be a "thought-experiment", nor is it a suggestion for a new way of working. Rather it is a formal exercise to be undertaken by those of us already working closely with GP systems. In the immediate case, my intent is to surface the three problems I've identified above:

1. What GP _does_
2. How GP _unfolds_
3. What GP is _for_

The rules for this exercise, "Doing GP as if you meant it", will feel the most artificial and restrictive to those of us with the most experience. Like the martial arts exercises from which it is ultimately inspired, it isn't intended to be simple or even pleasant for the participants. But because it emphasizes the interaction between the GP system and the user, it not only surfaces the philosophical mistake I've mentioned, but immediately suggests tools with which we can address Rick's decade-old wish.

# How we treat GP, and how it treats us in turn

Genetic Programming[^gpabbr] embodies a very particular _stance_ towards the scientific and engineering work of modeling, design, analysis and optimization. I increasingly suspect the resistance we've all recounted towards GP from our prospective technical and lay audience has little to do with our technical results, but rather arises from uncertainty among that audience with GP's very particular "way of working".

[^gpabbr]: And not just Genetic Programming as such, but also the broader discipline to which I claim it belongs and which is not obliged to be either "genetic" or "programming". I prefer to call this looser collection of practices "generative processing", and will also abbreviate it "GP"; assume I mean the latter in every case.

There is a tacit assumption, even among GP theorists and practitioners, that science and engineering are only "rigorous" when they proceed through a sequence of ordered phases from planning to implementation. The "scientific method" is most often represented something along the lines of

> 1. conceptualization; 2. planning; 3. design; 4. architecture; 5. implementation; 6. testing; 7. debugging

I am sure very few scientists or engineers of my acquaintance would admit any _real_ project (in history) has ever followed this narrative arc in a literal sense. But that story nonetheless informs and constrains much of our work lives, from fund-raising to publishing reports:

> Because of the body of published work, an insight was had. The insight was framed as a formal hypothesis. The hypothesis (and current Best Statistical Practices) suggested an experimental design, one familiar and obvious to any in Our Discipline. The experimental design was undertaken, the data were collected, the hypothesis duly tested, and now we can be confident of its veracity because... well, you just heard me say "Best Statistical Practices", right?

Under trivial term substitutions---"cost--benefit analysis" and "requirements document" for "hypotheses" and "experimental design", for example---the same narrative can be used to describe almost any institutional project management and public policy planning process as well. The flow in every case is essentially from _vision_ to _plan_, _plan_ to _implementation_, _implementation_ to _verification_, and _verification_ to _validation_.

Of course, nobody "really believes" this narrative who has ever done any of the work. It is a matter for another day to draw parallels with the social construction of religious belief.[^veyne] There have been numerous philosophical challenges to this artificial narrative of course, from Peirce and Dewey nearly a century ago, to Kuhn and Lakatos in the 1970s, and many more to be found in the Table of Contents in any Philosophy of Science text.

[^veyne]: Paul Veyne's excellent _Did the Greeks Believe in Their Myths?_ might be an interesting starting point, though.

One in particular is my focus today: Andrew Pickering.  

## Pickering's "Mangle of Practice"

Andrew Pickering's _Mangle of Practice_ is a decade old, but surprisingly little-known outside the field of Science Studies. His approach is especially noteworthy here because I find it so close to our actual experience using GP. Indeed, most colleagues who hear it for the first time utter an inevitable "didn't we already know this?" I think the distinction Pickering's approach is able to highlight is exactly the problematic one in our work: between the illusory (but publishable) narrative of "scientific method", and the realized experience we have of _performing science_. At the cost of glossing a lot of his well-considered structure, let me summarize.

First, the act of "doing science" is _at no point whatsoever_ to be understood as one in which an isolated "researcher" works in an objective and static field of "externalities" and "facts". Rather, research begins only when the researcher undertakes to _makes some artifact_: a block of code, a technical instrument, an equation, a sketch, a maquette or even a thoughtful conversation at a conference. Everything before the researcher begins to make these things is a matter of building "vision" (my term); research proper only occurs when work is done in the real world. I'll call this work product _the thing made_, and keep in mind that it is a proxy for the vague collection we might otherwise call "the project".

We see Pickering's model moving quickly away from more traditional views of science when we realize he has given the inanimate _thing made_ an agency of its own. In any real project, we perceive the _thing made_ resisting---whether we imagine that it resists "in itself", or as an agent of externalities that impinge on the work in progress to make our driving vision differ from reality. The _thing made_ comes to represent, in other words, the facts of the actual world, the cultural assumptions and norms of one's discipline, the raw materials and toolkit available to a practitioner, and so forth. "Resistance" here is not merely a problem with a software bug or lost raw materials, but includes one's sense _on seeing it_ that something's not quite right. The _thing made_'s resistance, as perceived by the researcher, leads her to react in turn.

Consider the language we use in the face of this resistance: it "feels wrong"; it "points something out"; it "wants to do X instead of Y"; it's "doing something too complicated for me to understand right now". 

And---assuming science, engineering, art or any other creative process is indeed what's being done---the researcher _changes in response to this resistance_. That vision changes, the plan adapts, or in some other way the _thing made_ causes a response in the state of the researcher herself. Pickering's Mangle is this emergent dance of inanimate agency: the researcher starting to follow a vision by making (or altering) a _thing_, and the _thing made_ in turn acting as a channel for the world itself to steer the researcher in another direction.

Pickering's "mangle"[^mangle] is this emergent dance of agency, undertaken between the researcher and the project: the researcher makes, the _thing made_ resists, the researcher is influenced and redirected, _accommodating_ the resistance. It may seem glib to say that "no plan survives contact with the enemy," but Pickering's narrative of creative work emphasizes the fact that no _revised_ plan survives unchanged, either. In the traditional narrative, we elide the work as it unfolded and re-frame it as a sort of idealized, apersonal Platonic truth: we use the passive voice, we hide the missteps and confusion, we paint a story moving from vision to plan to success.

[^mangle]:The word "mangle" he has chosen is itself interesting and insightful:
    
    > ...I find "mangle" a convenient and suggestive shorthand for the dialectic because, for me, it conjures up the image of the unpredictable transformations worked upon whatever gets fed into the old-fashioned device of the same name used to squeeze the water out of the washing. It draws attention to the emergently intertwined delineation and reconfiguration of machinic captures and human intentions, practices, and so on. The word "mangle" can also be used appropriately in other ways, for instance as a verb. Thus I say that the contours of material and social agency are mangled in practice, meaning emergently transformed and delineated in the dialectic of resistance and accommodation....

In a GP setting, the many modes of resistance we perceive are the very "symptoms" Rick mused about. Even though we as researchers know we've written all the code and set all the parameters, we're nonetheless willing to speak of a GP run "doing" things, as opposed to merely unfolding according to our plan. A GP system does not "resist" by, for example, "having the wrong population size". Rather it resists by _causing concern or dissatisfaction in the user_, which in turn sparks in that user a practical (if only explanatory) response, which leads them to change the _thing made_.

 
## GP as "mangle-ish practice"

The broader field of machine learning seems to take a much more traditional stance towards its subject matter: machine learning frameworks (excepting GP) are each discrete tools aimed at producing standardized and reproducible results to particular statistical questions. The result of training a neural network or even a random forest on a given data set is not expected to be a _surprise_ in any real sense, but rather the reliable and robust end-product of applying numerical optimization to a well-specified mathematical programming problem. Whether one describes these machine learning processes as "minimizing out-of-sample error" or "maximizing information gain", the supposed strength of most machine learning approaches is the _unsurprising_ nature of their use cases and outputs.

On the other hand, GP has the capacity to _tell us stories_---even in the relatively "simple" domain of symbolic regression. The space under consideration by GP is not merely a vector of numerical constants or a binary mask over a suite of input variables, but the _power-set_ of inputs, functions over inputs, and higher-order functions over those. We who work in the field can be glib about the "open-endedness" of GP systems, but that open-endedness puts them at odds with their supposed relatives in machine learning. While GP _can_ be used to explore arbitrarily close to some paradigmatic model, its more typical use case leads to the production of _unexpected_ insights---to the degree that a number of us feel justified in treating it as a strong candidate for "real" artificial intelligence.

I argue we have that leeway because of the way GP surfaces Pickering's Mangle. When the methodology "works", it does so by offering _helpful resistance_ in our engagement with the problem at hand, whether in the form of surprising answers, or validation of our suspicions, or simply legible suggestions of ways to make subsequent moves. GP _dances_ with us, while most other machine learning methods are "mere tools".

## Against replication

Nonetheless, there seems to be a widespread desire, inside and outside our field, to frame GP as a methodology for producing _unsurprising_ models from data, more in keeping with the traditional linear of scientific work. That is, an idealized user is expected to proceed something like the users of any other machine learning or mathematical programming framework:

1. frame your problem in the correct formal language
2. "get" a GP system
3. run GP "on your data"
4. (whatever this is, it's not our problem)
5. you have solved your problem

This is not original with the GP community; there is strong pressure from our peers in other disciplines and our users to promote this use case, not least because it is _exactly_ the stance expected in any planning or public policy setting, or in any scientific or programming project management setting. That is, we are under tremendous social pressure to treat GP as a _tool_ to be invoked in a known, predictable and well-described planning situation.

The resulting resistance from early-adopting users shouldn't be unexpected. _Being surprising_ may well be the worst conceivable behavior for any tool to be used in a traditional project setting. And given that pressure, it's no wonder that so much of GP research is focused on constraining tweaks to bring GP "into line". If only GP could be "tamed" or made "adaptive" so that step (4) above _never happens_.

I imagine this is why so many GP research projects strive for rigor in the form of counting replicates which "find the solution": they aim not to convince users of the known strengths of GP, but rather demonstrate to critical peers that GP can be "tamed" into a mere tool.

What would a "replicate" stand for, to a user who sought to exploit GP's strength in a project (whether theoretical or practical) where _search_ rather than the algorithm itself is the primary focus? Projects which authentically "use" GP _must necessarily_ be those searching for noteworthy answers---which is to say _surprising_ and _interesting_ answers---that they could not otherwise obtain. Thus, we should better think of a "replicate" as a sort of proxy for user frustration in step (4) above: that is, it represents a project in which search begins, stalls, and for which the user cannot see a way to move search forward towards more interesting and useful answers. 

But I think we would agree: it is a poor researcher who, when faced with a stumbling block in the form of a black box's misbehavior, doesn't attempt to work around that obstacle. Not to _begin the project again from scratch_, but to make changes and continue. Any researcher who is using GP _realistically_ will be watching, and adjusting, and engaging and interacting with the process of search itself.

When a GP user is viewed as working within the traditional _linear_ narrative of research, we see her run a population of 100 individuals for 100 generations, peer at the results that have been dumped into a CSV file, and find them wanting. Desiring an actual _answer_, she adjusts the GP parameters and begins another run of 100 generations... and repeats as necessary.

But there is _no discernible difference_ when we frame this same process of "many runs" as a single project involving initial researcher moves, resistances thrown up by the _thing made_, and subsequent accommodations made by the researcher to the new perceived truths. Except, that is, in the researcher's view of her own response to resistance: if she comes to the project with _plans_ for running "ten replicates", we can only assume she has learned from someone that a search algorithm is _supposed_ to forget everything it has learned every 100 generations....

I cannot help but be reminded of the fallacy, surprisingly common both in and outside of our field, that "artificial intelligence" must somehow be a self-contained and non-interactive process. That is, that an "AI candidate" loses all authenticity as soon as it is "tweaked" or "adjusted" in the course of operation. It is as if every new-born "AI" must be jammed into an air-tight computational container and isolated _until it learns to reason_, and that without exceeding a finite computational budget. If humans creating _real_ intelligences treated them anything like the way computer scientists insist we treat nascent _artificial_ ones, murder charges would be forthcoming.

There are several practical reasons for us to try something different.

Consider our hypothetical GP user. In keeping with the Behaviorist standards of GP, she is carefully "not interfering" with any given run of 100 generations; she can only peer at a results file after the fact. During the course of any 100 generations, all sorts of dynamics have happened: crossover, mutation, selection, all the many random choices. Imagine for a moment we were given perfect access to the entire dynamical pedigree of the unsatisfying results she receives at the end, and were able to backtrack to any point in the run and change a single decision. Before that point, it's unclear how badly things will actually turn out at the 100-generation mark; at some point after that juncture, it's obvious to anybody watching that the whole thing's a mess.

If such miraculous insights were available, then surely the correct approach would be to intervene and adjust the situation when the crucial point was reached... and then continue. Lacking (as we do) this miraculous insight, _why then does it seem reasonable to stop any run arbitrarily at a pre-ordained time point and begin again from scratch?_

It is, I think, because the myths of artificial intelligence and the linear narrative of science are so deeply intertwined. It is frowned upon to admit in a scientific paper, even when no mistakes were made, that the original vision and plan changed over the course of the project; rather we are expected to describe research _results_ as the inevitable outcomes of an ahistorical process, and erase all resistance and accommodation actually done by human beings in the context of their projects. Similarly, it feels somehow wrong to admit in a GP project, even if every parameter was set correctly, that the original vision and plan gave way to the inevitable surprises thrown up by GP's inherent tendencies to do just that.

But insofar as GP _surprises us_, and since that is its sole strength over more predictable and manageable frameworks, we must inevitably see a good fraction of those surprises at least in part as _disappointments_ rather than encouraging opportunities to change our plans.  Let's learn new ways to accommodate those disappointments, and stop trying to make them go away.

# "TDD as if you meant it"

As far as I can tell, Keith Braithwaite first described his [training exercise](http://cumulative-hypotheses.org/2011/08/30/tdd-as-if-you-meant-it/) for software developers in 2009. The target of the exercise is "Pseudo-TDD": the noted habit among software developers who claim to "know" and "do" test-driven development as part of their daily work towards a sort of thoughtless approximation of the technique.

I should note that a number of agile software development practices share informative relations to genetic programming's dynamics[^agileGP], but in this work I'll focus on those of TDD. In particular, test-driven development (or more accurately "test-driven design") _when done correctly_ can break down the complex design space of a software project into a value-ordered set of incremental test cases, focus the developers' attention on those cases alone, inhibit unnecessary "code bloat" and feature creep, and produce low-complexity understandable and maintainable software.

[^agileGP]: I imagine there is an Engineering Studies thesis in this for some aspiring graduate student: Genetic programming and agile development practices arose in the same period and more or less the same culture, and both informed by the same currents in complex systems and emergent approaches to problem-solving.

TDD _as such_ is a rigorous process, to the point where it can be described as "painful" (though also "useful") by experienced programmers. The steps are deceptively easy to trivialize and misunderstand, especially for those whose habits of thinking about code are ingrained:

1. Add a little (failing) test which exercises the next behavior you want to build into your codebase
1. Run all tests, expecting only the newest to fail
1. Make the minimal change to your codebase that permits the new test to pass
1. Run all tests, expecting them all to succeed
1. Refactor codebase to remove duplication

Each stage offers a stumbling block for an experienced programmer, but the most salient for us now is the iterative flow of implementation (or "design") that it imposes: Each cycle begins with a choice of _which little test should next be added_; each cycle ends with a rigorous process of refactoring, not just of the new code but of the _entire cumulative codebase_ produced so far. The middle three steps---implementing a _single_ failing test and modifying the codebase _by just enough_ so that all tests pass---feel when one is working through them as if they could be automated easily. The _mindfulness_ of the process lives in the choice of next steps and (though somewhat less so) of standard refactoring operations.

Braithwaite's exercise does an interesting thing to surface the formal rigor of this approach. In it, the participants (willing, of course, because the exercise is a _kata_ or "refresher" for experienced software developers to hone their skills) are asked to implement a nominally simple project like the game of Tic-Tac-Toe, given an _ordered_ list of features to implement and the artificial restriction that they must go farther than normal TDD practice asks. Rather than producing a suite of tests and a self-contained codebase, they are forced to use _only_ refactoring of code added to tests to produce their eventual "codebase". In other words, no code can be "produced" until the "smell" of duplication in the code added to multiple passing tests _provides a warrant_ for refactoring it out.

Further, a facilitator patrols ongoing work and deletes _any and all code not called for by a pre-existing failing test_. Words like "irritating" and "annoying" crop up in participants' accounts of  this onerous backtracking deletion the first few times it happens, as one might imagine. But as Gojko Adzik emphasizes in his descriptions of workshops, the resulting designs for even simple algorithms in this artificially amplified setting seems much more _open-ended_ than it would if the software were built under the typical norms and habits an experienced programmer uses in normal conditions.

A number of contextually positive benefits are attributed to agile software development practices, and to TDD within that suite of practices. But the one that brings us here today is that aspect surfaced particularly in Adzik's account of Tic-tac-toe:

> By the end of the exercise, almost half the teams were coding towards something that was not a $$3\times 3$$ char/int grid. We did not have the time to finish the whole thing, but some interesting solutions in making were:
> 
> - a bag of fields that are literally taken by players---field objects start in the collection belonging to the game and move to collections belonging to players, which simply avoids edge cases such as taking an already taken field and makes checking for game end criteria very easy.
> - fields that have logic whether they are taken or not and by whom
> - game with a current state field that was recalculated as the actions were performed on it and methods that could set this externally to make it easy to test

In other words: innovative approaches to the problem at hand began to arise, though there wasn't enough time to finish them in the time alloted for the exercise. The analogy to our collective experience to date with genetic programming should start to peek through at this point---though the thoughtful reader will hopefully wonder what utility there is in an analogy drawn between two similarly unsatisfying outcomes.

\[more here\]

# GP as if we meant it

In the same way that Braithwaite's onerous coding exercise is intended to drive the attention of its participants toward test-driven design with its obligation to write "real" code _only as a refactoring_, I'd like to be able to demand a _warrant_ for every step that moves our changing genetic programming setup away from just plain random guessing. Braithwaite's target of "Pseudo-TDD" suggests an analogous "Pseudo-GP": one in which the fitness function is the only "interface" with the problem itself, and where the representation language, search operators, search objectives and other algorithmic "parameters" are _fixed_.[^pseudoGP]

[^pseudoGP]: Braithwaite's participants often acknowledge they _know_ and _use_ TDD as it's formally described, but rarely take the time to do so unless "something goes wrong". I imagine many GP users will say they _know_ and _use_ all the innumerable design and setup options of GP, but treat them as adjustments to be invoked only when "something goes wrong". I offer no particular justification for either anecdote here, but the curious reader is encouraged to poll a sample of participants at any conference (agile or GP).

Not only do traditional search operators like crossover, mutation and \[negative\] selection not come "for free" in this variant, but in every case we must develop a cogent, data-driven argument in favor of starting them _as part of an ongoing search process_. Similarly, the initial selection criteria will be limited to a minimal subset of the training data, and expansion (and other alterations) of the training set will have to be made in light of measured progress, not assumptions that "more is better" in every case.

The result will be an incremental process of refinement of an ongoing search, carried out not at the level of externally-assigned parameter "tweaks" but rather by _opening_ the black boxes we typically demand and demanding we do surgery to correct their "pathologies" (and understand their mysteries) without killing them outright. It is not intended as an "algorithm" to supplant those used today, but rather as a forced re-description of what we actually already do.

## A tableau representation

The exercise proceeds as a sort of game, in which the User and the System take alternating turns. During the User's turn, she can see state of the system so far and make any of several _moves_ from a limited set, each of which involve changing the settings of a _tableau_, which completely describes the state of the search system. During the System's turn, it will execute a finite number of steps in which it creates new individuals. At the end of the System's turn, control reverts to the User, and vice versa.

The tableau, and the suite of moves available to the User, affect three core aspects of search: `operators`, `answers`, and `rubrics`.

### Operators

An `operator` is any function which takes as argument a (possibly empty) collection of `answers` and produces a new collection of `answers`. Operators thus include "random guessing", which in GP systems is often used to build an initial population, any "crossover" and "mutation" functions. No `operator` can refer to any `answer` not part of its argument set, not can any unset values be set within an `operator`.

So for example, if the User wished an `operator` could be constructed which took 30 `answers` as inputs, read their attributes (including `script` and that subset of their `rubric` values which were already set), and produce a single `answer` in its return set. It could _not_ take an `answer`, change it, and keep the new one "if it was better", since the new `answer` would have no scores at all.

Note that no process is provided for `answers` to be _removed_ from a tableau. The framework is purely cumulative.

All "parents" and other `answer` inputs required by an `operator` are chosen by _lexicase selection_ automatically, using the complete suite of `rubrics` in play when invoked. Lexicase selection samples each `rubric` with equal probability.

### Answers

An `answer` might traditionally be called an "individual" in GP literature. We can model them programmatically as key--value hash. All new `answers` are born with only a unique `id` and `script` field set. As an `answer` "matures" in the unfolding tableau, various other attributes will be set by the System player through the application of `rubrics`.

In the tableau visualization, we represent the unfolding collection of `answers` as the _rows_ of a spreadsheet-like table, and their attributes and scores as the _columns_.

### Rubrics

A `rubric` is any function which returns a scalar numerical value, given the instantaneous state of the tableau itself. For the most part these values can be understood as "scores" for various search objectives, though it is likely that a number of "implicit objectives" will also create new `rubrics`.

A `rubric` cannot store intermediate values, but can refer to the values of other `rubric` columns. Thus on specifying a `rubric` like "sum squared error over a training set", one must also create  `rubrics` for "measured error when provided input $$i$$" for every element $$i$$ of the training set. If the training set has 100 elements, then the single SSE `rubric` _implicitly_ represents a suite of 101 new columns added to the tableau.

A `rubric` function does however have access to the state of the entire tableau as needed, including "forcing" other `rubric` values to be calculated. So it is entirely possible to specify `rubric` functions which score:

- number of characters in the `answer`'s script
- maximum error measured in any of 35 other `rubrics`
- number of `div0` errors produced when running with a particular set of inputs
- number of stochastic instructions appearing in the `answer`'s script, compared to _all other `answers`_

### User moves

If we think of the tableau as a "spreadsheet" with a small list of `operators` and a large sheet of `answers` as rows and `rubrics` as columns, the User can add one new `operator` or one new column.

- add (or activate from a pre-existing list) exactly one `operator`
- add exactly one `rubric`, which may in turn create other _entailed_ `rubrics` as described above

### System turn

During its turn, the System player adds a specified minimum number of new `answers` to the tableau. If we think of the tableau as a "spreadsheet" the System can only add new `answers`. It follows a single rote cycle to do so:

1. select an `operator` from those in play, with equal probability
2. apply _lexicase selection_ to the tableau to select the required number of input `answers`, filling in missing `rubric` scores as needed
3. apply that one `operator` to the inputs to produce new `answers`, and append those new `answers` immediately to the tableau
4. `HALT` if the number of new `answers` added in this turn meets or exceeds the number present when the turn started (in other words, if the number of `answers` has doubled); otherwise, go to step (1)

### Initial setup and restrictions

- the only `operator` is "random guess", which creates a new `answer` with an arbitrary script
- no `rubrics` exist (only the `script` and `id` attributes of the `answers`)
- 50 new `answers` will be produced in the first System turn
- No `operator` can _evaluate_ any `answer` except in a self-contained local namespace which lacks access to any tableau values or state information
- There is no mechanism for _removing_ `answers` from the tableau
- The System player _always_ uses lexicase selection, and _always_ uses all `rubrics` as the selection features with equal probability.
- A `rubric` can only _run_ a single `answer` once; stochastic scripts will only be sampled one time, and no `rubric` score is ever recalculated after the first time

### Interface: the Goal of the Exercise

During the User's turn, they can of course interrogate the tableau in any way they want, without changing it. The point of the exercise is to drive the User to explore and create analytics and visualizations which can better inform their decisions over the course of the game.

# An example session

# Exploration and exploitation interfaces and affordances

# Final thoughts: What should it mean to _act intelligently_?

The idea of this exercise came from observing the differences between experienced and "novice" (though with strong technical skills) GP users as they explored new problems in a GP setting.