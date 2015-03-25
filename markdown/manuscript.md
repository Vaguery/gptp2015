# Why

More than a decade ago, Rick Riolo, Bill Worzel and I were working on a consulting project together that involved evolutionary algorithms and genetic programming. As we were chatting one day, Bill asked Rick what he'd most like to see as part of the research program of GP "moving forward". Rick's answer informs this contribution, as well as much of my work since.

As I recall, he said he'd like to better understand the "symptoms" we often see when we run an evolutionary search process: premature convergence, failure to improve, the sense when we look at results or ongoing runs that the many parameters _just aren't quite right_. Years later, I take this not only to mean that a benchmarking catalog listing conditions in which search operator $$X$$ under contingency $$Y$$ tends to produce outcome $$Z$$ might be helpful, but also that _the symptoms themselves_ are poorly understood.

The "field" of GP has grown quite a bit in the intervening 15 years. All of us who follow it closely can see the expanding front of new methodologies and techniques---the ones we discuss in our little workshop, and also the newsworthy and admirable public successes. But those advances bring an accompanying dilution of the scope of the theoretical warrants we use to justify them. Each example of "practice" seems to pose unique domain-specific quirks, each implementation makes numerous contingent (and often arbitrary) design and architectural choices, and even the ontological foundations of "individual", "fitness", "behavior" and "population" start to get sloppy around the edges whenever we actually look at What People Actually Do.

The lack of interest in GP among statisticians, planners, designers and mathematical programming aficionados is not just a matter of disciplinary boundary-setting. I imagine much of the skepticism arises from our own inability to say what's happening in a GP run (_even when we look_), let alone why, or what to do in response. We have learned that _more CPU time_ can get us past the stage of "not working", but I argue that we have not been able to satisfactorily explain _why_ for any reasonable and interesting example.

I also argue that this is not the _fault_ of GP. Rather it's the source of GP's long-term survivability as an approach to scientific and engineering projects. In making this argument, I'm forced to stray into philosophical territory before coming back to build a concrete example. But along that wandering path there are several points where we can productively address Rick's concerns.

# How we treat GP (and how it treats us in turn)

Genetic Programming (and the broader body of work to which it belongs, which I prefer to call "generative processing" and also abbreviate as "GP") embodies a very particular _stance_ towards the scientific and engineering work of modeling, design, analysis and---though least of all---optimization. A case could be made that the resistance we have all recounted towards GP from the technical and lay audience has little to do with the technical results so amply demonstrated over the last quarter-century, but rather from discomfort among that audience in GP's particular "way of working" on problems. There is a tacit assumption, even among GP theorists and practitioners, that science and engineering are "rigorous", or even successful, only when they proceed through ordered phases of

1. conceptualization, the "vision thing"
2. planning
3. design
4. architecture
5. implementation
6. testing
7. debugging

Very few scientists or engineers would admit that any _real_ project has followed this narrative structure in practice, but in every phase of work life from fund-raising to published reports the story is framed in that way, especially in scientific work: Because of work gone before, an idea or insight was had. The idea was framed as a formal hypothesis. The hypothesis (and current Best Statistical Practices) suggested an experimental design obvious to any in the discipline. The experimental design was undertaken, the data collected, the hypothesis tested, and we can be confident of its veracity because, well, you just heard me say "Best Statistical Practices", right?

Under trivial term substitutions---"cost--benefit analysis" and "requirements document" for "hypotheses" and "experimental design", for example---the same narrative can be used to describe any institutional project management and public policy planning process as well. The flow in every case is essentially from _vision_ to _plan_, _plan_ to _implementation_, _implementation_ to _verification_, and _verification_ to _validation_.

Of course, nobody "really believes" this narrative who has ever done any of the work. It is a matter for another day to draw parallels with the social construction of religious belief.[^veyne] There have been numerous philosophical challenges to this artificial narrative of course, from Peirce and Dewey nearly a century ago, to Kuhn and Lakatos in the 1970s, and more today.

[^veyne]: Paul Veyne's excellent _Did the Greeks Believe in Their Myths?_ might be an interesting starting point, though.

## Pickering's "Mangle of Practice"

Andrew Pickering's _Mangle of Practice_ is the one I'd like to use here. This approach is noteworthy because it is so close to our actual experience using GP, of the "didn't we already know this?" sort. At the risk of eliding a lot of extraordinarily well-considered structure, let me summarize:

The act of "doing science" is _at no point whatsoever_ a behavior undertaken by an isolated "researcher" in an objective field of externalities. Rather, the researcher who initiates this process with a vision, intuition, hypothesis or hunch begins always by _making some artifact_: code, an instrument, a proof, a sketch, a maquette or even a thoughtful conversation at a conference.

The thing, inevitably, _resists_. That is, Pickering grants it agency, or rather makes it the agent of all those externalities that impinge on the work to make the _vision_ differ from _practice_: the facts of the actual world, the cultural assumptions and habits of discipline, the raw materials and toolkit available to the practitioner, and so forth. "Resistance" here is not merely the obvious trouble of a software bug or some missing parts, but includes one's sense _on seeing it_ that something's not quite right; the realization that more (or less) is needed. The language we use, in the face of this resistance, is that the _thing made_ "feels wrong" or "points something out", that it "wants to do X instead of Y", or that "it's doing something too complicated for me to understand right now".

And---assuming science is indeed what's being done---the researcher _changes in response to this resistance_. That vision changes, the plan adapts, or in some other way the _thing made_ causes a response in the state of the researcher herself. Pickering's Mangle is this emergent dance of inanimate agency: the researcher starting to follow a vision by making (or altering) a _thing_, and the _thing made_ in turn acting as a channel for the world itself to steer the researcher in another direction.

The "mangling" of Pickering's metaphoric name refers not to wounding but to the mechanical laundry apparatus, the antique wringer through which sheets and linens are run to drain the water, and which as a _side-effect_ impose a novel higher-dimensional structure and juxtapose unexpected components with one another. Here, the response of the researcher to resistance she experiences in the course of her project is no less a part of the dynamics of "science" than the act of writing and running code, the authority warranted by particular statistical practices in her discipline, or the raw pressure of physical laws.
 
## GP as "mangle-ish" practice

The broader field of machine learning leans seems to take a much more traditional stance towards its subject matter: machine learning frameworks (excepting GP) are each discrete tools aimed at producing standardized and reproducible results to particular statistical questions. The result of training a neural network or random forest on a given data set is not expected to be a _surprise_ in any real sense, but rather the sufficiently robust result of applying numerical optimization to a particular mathematical programming model. Whether one describes the process as minimizing out-of-sample error or maximizing information gain, the focus of the discipline is on contingent reliability rather than exploratory modeling.

GP has the capacity to _tell us stories_, even in the "simple" domain of symbolic regression problems. The space under consideration is not merely a vector of numerical constants or a binary mask over a suite of input variables, but the _power-set_ of inputs, and of functions over inputs, and of higher-order functions over those. While GP can be used to explore arbitrarily close to a paradigmatic model, we argue for it most when its application can produce unexpected insights. A number of us treat it as the best candidate for "real" artificial intelligence, and rightly so.

We can do that because GP surfaces Pickering's Mangle. When it "works", it does so by offering _helpful resistance_ in our engagement with it, whether in the form of surprising answers, validation of our suspicions, or suggestions of ways to make subsequent moves. It _dances_ with us, in a way that the other mere tools of machine learning do not.

Now a great deal of the last quarter-decade of work in the field, especially in the early days, seems to treat GP as a close relative of other machine learning techniques---as a methodology for producing _unsurprising_ models from data, and within the traditional model of scientific work. That is, an implicit and idealized user is expected to proceed something like the users of any other machine learning framework:

1. frame your problem in the framework-specific language
2. "get" a GP system
3. "run GP on the data"
4. ???
5. you have solved your problem

There's strong pressure from our actual user community to enforce this stance, not least because it is exactly the stance of planning and public policy, and of the mythic science or programming project manager. That is, it frames GP as a _tool_ to be invoked in a known and well-described planning situation.

The resulting resistance from early-adopting "users" who "get" an off-the-shelf GP system and try to apply it to their problem---at least within this narrative of work---should not be unexpected. _Being surprising_ is perhaps the worst choice for any traditional project plan. It's no wonder that so much of GP research is focused on particular tweaks and staged horse-races among the unnumbered techniques: as long as there is a sentiment that GP might be "tamed" so that step (4) above _never happens_, so that the framework might join its more traditional and popular relatives, there will be strong pressure to calculate statistics on, for example "proportion of successes in 30 replicates".

But what does a "replicate" mean for somebody using GP for a real project, whether theoretical or practical, where _GP itself_ is not the focus of the work? Projects which authentically "use" GP must necessarily be searching for noteworthy answers, which is to say _surprising_ and _interesting_ answers, that they cannot otherwise obtain. To do otherwise is to somehow expect GP to be a form of "research automation", a premature expectation in light of our experience.

Therefore, any researcher who is using GP _realistically_ is one who is watching, and adjusting, and engaging and interacting with the process of search itself. Seen as working within a traditional (and deeply misleading) narrative framework, we watch as she runs a population of 100 individuals for 100 generations, peers at the results in a CSV file and finds them wanting, and then adjusts the GP parameters to run another 100 generations... and repeating as necessary. But there is _no discernible difference_ when we frame the same process as one of participating in the single application of an interactive algorithm that (remarkably, and unaccountably when you think about it) _throws away all intermediate results every 100 generations_.

Further, let's explore that traditional narrative which sees her workflow as a series of repeated attempts to use GP as a tool in a situation for which it is poorly suited, or maybe "badly tuned". She is carefully "not interfering" with any given run of 100 generations, observing our tradition in this field, and can only peer into the results file after the fact. During the course of any 100 generations, all sorts of dynamics have happened: crossover, mutation, selection, all the many random choices. Imagine for a moment we were given perfect access to the entire dynamical pedigree of the unsatisfying results she receives at the end, and were able to backtrack to any point in the run and change a single decision. Before that point, it's unclear how badly things will actually turn out at the 100-generation mark; at some point after that juncture, it's obvious to anybody watching that the whole thing's a mess.

If such miraculous insights were available, then surely the correct approach would be to intervene and adjust the situation when the crucial point was reached... and then continue. Lacking (as we do) this miraculous insight, _why then does it seem reasonable to stop any run arbitrarily at a pre-ordained time point and begin again from scratch?_

It is, I think, because the myths of artificial intelligence and vision-driven science are deeply intertwined. It is somehow "cheating" to admit in a scientific paper, even if no mistakes were made, that the original vision and plan changed over the course of the project; rather we describe research _results_ as inevitable outcomes of an ahistorical process that erases the work actually done by human beings. Similarly, it is somehow "cheating" to admit in a GP project, even if every parameter was set correctly, that the original vision and plan gave way to the inevitable surprises thrown up by GP's tremendous potential to surprise.

But insofar as GP _surprises us_---and that is its sole strength over more predictable and manageable frameworks---we will inevitably see a good fraction of the surprises as _disappointments_ rather than opportunities to adapt our selves.

# "TDD as if you meant it"

As far as I can tell, Keith Braithwaite first described his [training exercise](http://cumulative-hypotheses.org/2011/08/30/tdd-as-if-you-meant-it/) for software developers in 2009. The target of the exercise is "Pseudo-TDD": the noted habit among software developers who claim to "know" and "do" test-driven development as part of their daily work towards a sort of thoughtless approximation of the technique.

I should note that a number of agile software development practices share informative relations to genetic programming's dynamics[^agileGP], but in this work I'll focus on those of TDD. In particular, test-driven development (or more accurately "test-driven design") _when done correctly_ can break down the complex design space of a software project into a value-ordered set of incremental test cases, focus the developers' attention on those cases alone, inhibit unnecessary "code bloat" and feature creep, and produce low-complexity understandable and maintainable software.

[^agileGP]: I imagine there is an Engineering Studies thesis in this for some aspiring graduate student: Genetic programming and agile development practices arose in the same period and more or less the same culture, and both informed by the same currents in complex systems and emergent approaches to problem-solving.

TDD _as such_ is a rigorous process, to the point where it can be described as "painful" (though also "useful") by experienced programmers. The steps are deceptively easy to trivialize and misunderstand, especially for those whose habits of thinking about code are ingrained:

1. Add a little test
1. Run all tests and fail
1. Make a little change
1. Run the tests and succeed
1. Refactor to remove duplication

Each stage offers a stumbling block for an experienced programmer, but the most salient for us now is the overarching flow of implementation (or "design") that it imposes: Each cycle, and also the overarching design process, begins with the choice of _which little test should next be added_; each cycle ends with a rigorous process of refactoring, not just of the new code but of the _entire cumulative codebase_ produced over all iterations of this cycle. The middle three steps---implementing a _single_ failing test and modifying the codebase _by just enough_ so that all tests pass---feel when one is working through them as if they could be automated easily. The _mindfulness_ of the process lives in the choice of next steps and (though somewhat less so) of standard refactoring operations.

Braithwaite's exercise does an interesting thing to surface the formal rigor of this approach. In it, the participants (willing, of course, because the exercise is a _kata_ or "refresher" for experienced software developers to hone their skills) are asked to implement a nominally simple project like the game of Tic-Tac-Toe, given an _ordered_ list of features to implement and the artificial restriction that they must go farther than normal TDD practice asks. Rather than producing a suite of tests and a self-contained codebase, they are forced to use _only_ refactoring of code added to tests to produce their eventual "codebase". In other words, no code can be "produced" until _duplication in code added multiple passing tests_ provides a warrant for refactoring it out. Further, a facilitator patrols ongoing work and deletes _any and all code not called for by a pre-existing failing test_.

Words like "irritating" and "annoying" crop up in participants' accounts of  this onerous backtracking deletion the first few times it happens... as one might imagine. But as Gojko Adzik emphasizes in his descriptions of workshops he's run, the results of "design" of even simple algorithms in this artificial amplified setting seems much more _open-ended_ than it would if the software were built within the typical framing of habits and assumptions that an experienced programmer carries along with her to any project.

A number of contextually positive benefits are attributed to agile software development practices, and to TDD within that suite of practices. But the one that brings us here today is that aspect surfaced particularly in Adzik's account of Tic-tac-toe:

> By the end of the exercise, almost half the teams were coding towards something that was not a $$3\times 3$$ char/int grid. We did not have the time to finish the whole thing, but some interesting solutions in making were:
> 
> - a bag of fields that are literally taken by players---field objects start in the collection belonging to the game and move to collections belonging to players, which simply avoids edge cases such as taking an already taken field and makes checking for game end criteria very easy.
> - fields that have logic whether they are taken or not and by whom
> - game with a current state field that was recalculated as the actions were performed on it and methods that could set this externally to make it easy to test

In other words: innovative approaches to the problem at hand began to arise, though there wasn't enough time to finish them in the time alloted for the exercise. The analogy to our collective experience to date with genetic programming should start to peek through at this point---though the thoughtful reader will hopefully wonder what utility there is in an analogy drawn between two similarly unsatisfying outcomes.



# What it can mean to "use" genetic programming

# Cases for and against "usable GP": cost, features, reliability, simplicity

# "Artificial Intelligence investigated by Special Crimes Unit"; or, Even Athena needed the head of Zeus

# The mangle of practice and the mangle in practice

# The yardstick and the bamboo hand

# Leveraging "resistance": the problem of the dots and lines

# "Batch" and "interactive" styles

# Expressing "resistance": the problem of Bertrand's Paradox

# Exploration and exploitation interfaces and affordances

# The deeper question: What should it mean to _act intelligently_?