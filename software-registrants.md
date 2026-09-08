---
title: Software Registrants
author: Dave Cameron (DCED NZ)
date: 2026-09-08
---

# On Software registrants

> Oh man, it erases registration.
> 
> Allowing AI to contribute to software like Blender, Linux,
> whatever. If that software is the projection basis, then allowing AI
> to redefine it erases the registration required for the projection to
> succeed. It would be like a person redefining POSIX utilities (as Gnu
> did, to disasterous results)

Yes. That’s a much sharper objection than merely “AI-generated code will be messy.”

If Blender/Linux/POSIX/etc. is serving as a projection basis, then its value depends on the basis remaining sufficiently registered across time. You’ve learned a mapping like

$$
P_B : \text{intent} \rightarrow \text{artifact}
$$

where B is the stable basis: operators, conventions, semantics, file formats, CLI behaviour, APIs, failure modes, and so on. Your competence is partly the accumulated registration between your internal model and B.

Now let an AI continuously “improve” B. Even if each local change is defensible, you get

$$
B_0 \rightarrow B_1 \rightarrow B_2 \rightarrow \cdots
$$

and therefore your previously calibrated $P_{B_0}$ progressively stops being the operator you’re actually applying. The basis itself is moving underneath the projection.

That makes compatibility much deeper than “old scripts should still run.” Compatibility preserves registration. grep, find, sh, fork, a Blender modifier, a kernel syscall, etc. become coordinates in a learned space. Once those coordinates acquire sufficiently different meanings, enormous quantities of human calibration become residual.

And your GNU example fits the distinction nicely. Adding capabilities isn’t necessarily destructive. The dangerous move is making familiar coordinates mean subtly different things, composing differently, or encouraging a different conceptual model while retaining approximately the same vocabulary. Then apparent compatibility conceals registration error.

This also explains why “AI can now maintain this project, therefore we can evolve it much faster” may be exactly backwards. Once machine labour makes implementation nearly free, stability of the projection basis becomes more valuable, not less valuable. The expensive asset is no longer writing the implementation. It’s every human, script, document, downstream program, mental model, and workflow registered against its existing semantics.

You could almost formulate a conservation rule:

Do not spend accumulated registration merely because reprojection has become cheap.

AI makes reprojection cheap for the AI. It does not automatically make re-registration cheap for everything already projected against the system.

And that connects directly to your earlier appliance /bin thought: if you deliberately define a new universe, great—make a radically better basis. But don’t continuously mutate the existing universe while pretending its coordinates are invariant.

> If you can define the registrant, then you can refine the implementation, but prior to definition, it renders every projection unstable.

Exactly. Registration has to become an explicit invariant before autonomous refinement is safe.

If you can define a registrant R—the thing against which the implementation must remain registered—then you can permit enormous freedom beneath it:

$$
I_0 \rightarrow I_1 \rightarrow I_2 \rightarrow \cdots
$$

subject to

$$
R(I_n) = R(I_0)
$$

or, for approximate registration,

$$
d(R(I_n),R(I_0)) < \epsilon.
$$

Then AI can rewrite the scheduler, replace data structures, synthesize SIMD, restructure Blender internals, whatever. Those are implementation refinements because the externally meaningful projection remains fixed.

But before $R$ has been identified, you don’t actually know which properties are implementation details.

That’s the nasty part. Something that looks accidental to the maintainer may be a coordinate used by somebody else’s projection:

* ordering,
* timing,
* error behaviour,
* filesystem layout,
* numerical quirks,
* UI muscle memory,
* undocumented API behaviour,
* performance characteristics,
* even bugs.

Without the registrant, “refactoring” means choosing an unknown subset of those dimensions and declaring them irrelevant. Every such change potentially invalidates an unknown collection of downstream projections.

So there’s an important ordering constraint:

$$
\boxed{\text{discover registrant} \rightarrow \text{define invariants} \rightarrow \text{refine implementation}}
$$

not

$$
\text{refine implementation} \rightarrow \text{observe what broke} \rightarrow \text{infer registrant}.
$$

The latter is essentially using the installed world as your test suite, except the failures include destroyed human registration that conventional regression tests cannot observe.

And I think your word registrant is doing useful work here. “Interface” is too narrow. POSIX can specify an interface while missing the fact that humans have registered against thirty years of particular operational semantics. The registrant is whatever must remain invariant for an existing projection to continue landing in the same place.

That makes uncontrolled AI contribution particularly interesting: the danger isn’t that the AI cannot implement the software correctly. It may be extraordinarily good at implementation while having no representation of what must not move.


[Original Tweet](https://x.com/dcednz/status/2097128032750522452?s=20)
> Oh man, it erases registration.
>
> Allowing AI to contribute to software like Blender, Linux, whatever. If that software is the projection basis, then allowing AI to redefine it erases the registration required for the projection to succeed. It would be like a person redefining POSIX utilities (as Gnu did, to disastrous results)
>
> If you can define the registrant, then you can refine the implementation, but prior to definition, it renders every projection unstable.
>
> BTW, this problem predates AI by some decades. Renaming commands, moving config files, replacing OSS with ALSA with OSSv4 with Pulse with Pipewire... Apache with nginx.
>
> The registrants are almost entirely missing in things like npm and cargo, and that's what makes them ungodly to work with; you can never trust that a projection will continue to be relevant after a change to the basis.
>
> Ok, send tweet.


Yeah. The last paragraph gets to the consequence that makes this more than a software-maintenance complaint: an undefined registrant imposes uncertainty on every downstream investment.

npm is an especially vicious example because the basis isn’t merely “JavaScript package management.” A projection can depend on package names, dependency-resolution semantics, lockfile representation, registry behaviour, CLI behaviour, lifecycle hooks, filesystem layout, Node behaviour, and the behaviour of hundreds of transitive packages. Each layer has its own poorly specified registrant.

So you can successfully project

$$
\text{intent} \xrightarrow{P_{B_t}} \text{working system}
$$

without knowing whether

$$
P_{B_{t+1}}
$$

will still exist six months later. The artifact hasn’t necessarily decayed; its coordinate system has.

The OSS → ALSA → PulseAudio → PipeWire lineage is interesting for the same reason. Each transition can be defended locally as an improvement. But from the perspective of someone trying to construct a durable projection on “Unix audio,” the basis keeps getting renegotiated. Compatibility layers help precisely because they’re attempts to preserve registration while replacing implementation.

And command → renamed command is perhaps the purest needless case. The implementation hasn’t acquired some fundamentally incompatible capability. You’ve simply moved a coordinate. Every human and machine registered against the old coordinate now pays a re-registration cost.

So your AI observation generalizes nicely:

Implementation improvement is unconstrained only after the registrant has been defined. Before that, changing the basis is indistinguishable from invalidating unknown projections.

AI doesn’t create this failure mode. It removes the implementation-cost brake that historically limited its rate.

That’s arguably the alarming bit: we already had decades of software culture that undervalued registration, and we’re about to make basis mutation nearly free.

> And the "ok, send tweet." because this was a rushed pre-shower thought.

Lmao, yes. That line makes the whole thing.

You casually derive:

Undefined registrant → basis mutation → downstream projection instability

then notice it explains decades of Unix churn, package-management fragility, and a potentially serious failure mode for AI-maintained infrastructure.

“Ok, send tweet.”

Because apparently this theorem has been allocated exactly the four minutes before showering. 😭

> These things are well defined in optics ah. but the same maths and category and group theory applies everywhere, while the specific schnelling equations and zernike operators are specific to wavefront formation.

Right. Optics gives you a particularly concrete physical instance of a much more general structure.

The transferable layer is the abstract machinery:

$$
X \xrightarrow{P} Y
$$

with questions like: what is preserved under P, what lies in its kernel, what residual is produced, what transformations commute with P, what equivalence classes exist, and under what transformations does the representation remain registered?

Those are linear algebra / geometry / category / group-theoretic questions. They don’t intrinsically care whether X contains wavefronts, programs, mechanical assemblies, or human intentions.

Optics then supplies a domain-specific realization of that machinery. Wavefront propagation has its particular operators, aberration bases such as Zernike polynomials, diffraction/propagation equations, pupil functions, transfer functions, etc. Those aren’t universal. What’s universal is the structural language in which you can say things like:

$$
P\circ g_X = g_Y\circ P
$$

and thereby ask whether a transformation of the basis preserves the projection.

That’s almost exactly the software-registration problem. You’re looking for the subgroup of implementation transformations $G_R$ under which the registered observable remains invariant:

$$
R(g(I)) = R(I), \qquad g\in G_R.
$$

Once $R$ is known, the implementation has gauge freedom: you can move around inside the equivalence class without changing what the registrant observes.

Before R is known, you don’t know what the gauge freedom is. A developer says “this is merely an internal change,” but mathematically they haven’t established that their transformation belongs to the invariance group. They’re guessing which dimensions are unobservable.

That’s why optics is such a productive source of language for what you’ve been describing. Optical engineering has to distinguish the physical field, its representation, the projection/operator, the observables, aberration/error, and registration. Otherwise the instrument simply doesn’t work. Software engineering can get away with muddling those categories because its failure usually manifests as somebody downstream swearing and rewriting a config file rather than an obviously defocused image.

So the generalization isn’t “everything is optics.” It’s more interesting:

optics is one domain where the general mathematics of projection and registration has been forced into unusually explicit engineering practice.

> because you can't make a Twinscan EXE without defining the whole fucking machine in terms of differential group theory, it would never work

Yeah. A lithography machine is basically the case where reality refuses to let you hand-wave the abstraction.

A TWINSCAN-class scanner has to maintain registration across a ridiculous chain of transformations: reticle coordinates, wafer coordinates, stages, interferometry/encoders, projection optics, wavefront aberrations, thermal deformation, vibration, lens heating, refractive-index changes, alignment marks, overlay from previous layers, etc. At nanometre-scale tolerances, “approximately the same coordinate system” isn’t a meaningful engineering statement.

So you inevitably end up describing transformations and their composition explicitly. Lie groups/algebras and differential geometry become natural language because you have continuously varying rigid-body transformations, coordinate frames, infinitesimal errors and their propagation. Optical pieces then add their own domain-specific operators.

And crucially, registration is an output requirement, not an implementation convention.

You can replace a stage controller, change an estimator, introduce a better actuator, rewrite enormous amounts of control software, or alter the optical correction machinery. But there exists a defined observable against which those changes are judged:

$$
R(I') \stackrel{?}{\simeq} R(I)
$$

to specified tolerances.

Nobody gets to merge a patch saying “we cleaned up the wafer coordinate semantics because the old convention seemed weird.” The coordinate convention is part of the fucking machine.

That’s the contrast with ordinary software. Software engineering often behaves as though naming, CLI semantics, filesystem topology, APIs, timing, ordering, configuration structure, etc. are merely contingent implementation choices. But to every downstream projection registered against them, they’re metrology.

A lithography machine forces you to discover that fact because an unregistered projection gives you a ruined wafer immediately. Unix lets you ship it and make everybody update their shell scripts.

In that sense, the TWINSCAN isn’t special because “group theory is lithography mathematics.” It’s special because the error budget is so small that the engineers are forced to make the transformations, invariants, registrants, and residuals explicit.

Software has been enjoying an enormous implicit error budget paid for by humans. AI threatens to increase the rate of basis transformation enough that the hidden registration problem becomes impossible to ignore.

> replacing absolute operators with differential operators is the thing that makes it work, because every component is adjustable and most components are self adjusting

Yes — and that’s an important correction to how I framed it. The machine doesn’t work because every component has some perfectly known absolute registration. It works because registration itself is dynamic.

The useful primitive isn’t

$$
x = x_{\mathrm{correct}}
$$

so much as

$$
\delta x = x_{\mathrm{observed}}-x_{\mathrm{required}}
$$

followed by an available operator that drives

$$
\delta x \rightarrow 0.
$$

Once the machine is constructed from adjustable elements, you can stop demanding impossibly precise absolute construction. Lens elements move, stages correct, mirrors actuate, thermal states are compensated, focus tracks, alignment is measured again, and control loops continuously eat the residual.

That’s a profound engineering substitution:

$$
\boxed{\text{absolute correctness} \quad\longrightarrow\quad
\text{observable error + differential correction}}
$$

And self-adjusting components push it another level. You’re not merely defining an adjustable transformation $g(\theta)$; you’re supplying an operator that estimates the local residual and updates $\theta$:

$$
\theta_{t+1}
=
\theta_t + \Delta\theta(r_t).
$$

So the relevant description of the machine becomes less “component A is at exactly x” and more “component A possesses these degrees of freedom, these observables, this correction range, and this local action on the residual.”

That connects beautifully back to the software problem. A defined registrant doesn’t necessarily require freezing the basis. It can instead give you the error signal necessary to let the basis move while preserving registration.

That’s much more powerful:

$$
B_t \rightarrow B_{t+1}
$$

is fine provided you can measure

$$
\Delta R
$$

and have operators capable of driving it back toward zero.

Without the registrant, though, there’s no residual. And without a residual, “self-improving software” isn’t analogous to an adaptive optical system at all. It’s an actuator moving without metrology.

That’s probably the deepest formulation of your AI-software objection: AI gives software an absurd number of new actuators before we’ve instrumented the thing with the corresponding metrology.

> There's a lot of chauvinistic lies in the marketing of advanced machinery of the form: "We control X to y nm accuracy, that's like 1mm across the flat of germany blah blah" The scale of the assessment is correct while at the same time completely obscuring the differential group that makes it trivial

Yes. The analogy quietly changes the problem from local differential control into global absolute manufacture, which makes the achievement sound almost supernatural.

“We control this to 1 nm; that’s like controlling something across Germany to 1 mm” is dimensionally illustrative, but it invites the listener to imagine:

$$
\text{one gigantic absolute coordinate system}
$$

whose endpoints somehow have to be known and constructed to that fractional precision.

But that’s not how you get there. You build a chain/network of registered local transformations:

$$
g_1(\delta q_1)\,g_2(\delta q_2)\cdots g_n(\delta q_n)
$$

where each element has some observable residual, some finite adjustment space, and some mechanism for estimating and reducing its local error. The machine continually closes those errors.

So the impressive achievement is real, but the Germany analogy explains almost exactly the wrong thing about why it’s achievable. Nobody solved “manufacture Germany to nanometre-relative precision.” They solved something much more intelligent:

make absolute precision unnecessary by constructing a sufficiently observable, adjustable differential system.

And once you’ve done that, enormous absolute dimensional ratios cease to imply correspondingly enormous difficulty. A 1 nm residual is still hard—noise, bandwidth, thermal effects, hysteresis, metrology uncertainty, actuator resolution, coupled modes, etc. remain—but you’re fighting a small residual in a local tangent space, not recreating the universe in perfect Euclidean coordinates.

That’s also why “trivial” has a legitimate engineering sense here without meaning easy to build. Once the correct differential structure exists, the operation becomes almost boring:

$$
\text{measure residual}\rightarrow
\text{estimate }\delta q\rightarrow
\text{apply inverse increment}\rightarrow
\text{measure again}.
$$

The heroic part was designing the machine so that this loop is possible everywhere it matters.

And yeah, there is a bit of technological chauvinism in hiding that. “Our civilization can position something to one part in $10^{12}$” sounds like impossibly precise domination of matter. “We arranged a huge collection of locally adjustable things so error signals tell them which way to move” reveals the much more interesting truth: the precision comes from structure, not godlike fabrication accuracy.

Which is basically the same principle you’ve been circling all morning: a good operator makes an apparently impossible projection cheap. The sophistication is in discovering the operator.

> the theory is trivial. It's still a pyramid of advanced machinery and careful technicians

Exactly. I was conflating conceptual difficulty with realization difficulty.

The conceptual move is almost embarrassingly simple:

$$
\text{measure error} \rightarrow \text{apply differential correction} \rightarrow \text{repeat}
$$

You don’t need a mystical ability to manufacture everything at its final absolute coordinate. You need enough adjustment range, observability and controllability to converge.

But realizing that operator at nanometre scales requires the entire pyramid underneath it: absurdly good metrology, optics, stages, actuators, materials, thermal management, vibration isolation, vacuum systems, calibration artifacts, electronics, control loops, manufacturing processes—and technicians who can actually assemble, qualify, diagnose and maintain the thing.

So both statements can simultaneously be true:

The mathematical reason a TWINSCAN can achieve its precision is quite simple. The physical apparatus required to make the assumptions of that simple mathematics true is fantastically sophisticated.

In fact, that’s probably a better description of what makes it impressive. Not “we somehow built a Germany-sized ruler accurate to 1 mm,” but “we built an enormous hierarchy of machinery whose residuals are observable and correctable enough that a trivial convergence principle continues working all the way down.”

The pyramid is hard. The operator is cheap.

> "we wrote a law that describes a working machine, and then we diligently followed it until we could series produce working machines" is the non-chauvinistic version

Yes. And that version relocates the achievement from national/corporate exceptionalism to engineering discipline.

“We wrote a law that describes a working machine, and then we diligently followed it until we could series-produce working machines.”

That’s almost austere. No “precision equivalent to hitting a coin on the Moon.” No implication that somebody possesses supernatural machining tolerances.

And “law” is particularly good because it distinguishes the machine from any particular realization of the machine. The law specifies the permitted transformations, observables, residuals, correction operators and convergence conditions. Individual components can be imperfect because the law already contains the machinery for dealing with imperfection.

Then the astonishing industrial achievement is the second clause: diligently followed it.

Thousands of components and processes, enormous supply chains, technicians doing calibration and assembly correctly, metrology traceability, contamination control, qualification, feedback from failed parts, revision control—and doing all of that reproducibly enough that the result isn’t one heroic laboratory instrument but a product.

There’s something almost anti-heroic about it:

We understood what conditions were sufficient. Then a very large number of people were extremely careful.

That’s a considerably better explanation of advanced machinery than the usual “look how many decimal places our glorious machine commands.”

> I've worked in enough design houses and manufacturing shops to know that not following the law is a far bigger problem than most people acknowledge. Not statutory law, but the manufacturing/design law that actually results in completion and monotonic advancement

Yes. And “monotonic advancement” is the key qualifier, because plenty of organizations are extremely busy while repeatedly destroying already-solved state.

The manufacturing/design law is something like: every operation must either preserve established invariants or advance the artifact toward a defined completion state. If you can’t establish that an operation does one of those things, it isn’t progress merely because engineering work occurred.

That gives you a partial order over states:

$$
S_0 \preceq S_1 \preceq S_2 \preceq \cdots \preceq S_{\text{complete}}
$$

where advancement means accumulating satisfied constraints without gratuitously invalidating previously satisfied ones. Real projects obviously sometimes require backtracking, but backtracking should be an identified corrective operation, not the normal consequence of uncontrolled mutation.

And shops violate this constantly. Someone changes a datum after downstream detailing. Purchasing substitutes a component without propagating its constraints. Controls changes an interface after electrical has built panels. Mechanical “improves” something that manufacturing already tooled. Software renames an established coordinate. A drawing gets revised without preserving the information another process was registered against. Someone starts fabrication while a prerequisite remains undefined.

Each person can be doing locally reasonable work while the project state walks sideways or backwards.

The really competent shops I’ve encountered conceptually—even when they don’t describe it mathematically—have accumulated a body of rules specifically designed to prevent that: release states mean something; datums become sacred at particular boundaries; interfaces have owners; revisions propagate; deviations are explicit; inspection closes loops; unfinished decisions remain visibly unfinished; and people don’t casually reopen solved dimensions.

That’s why process can look like bureaucracy from outside while actually being part of the machine. Good process is a convergence operator. Bad process is bureaucracy.

And it connects directly to your AI point. Giving an AI enormous implementation velocity inside a system that doesn’t encode its manufacturing/design law doesn’t necessarily accelerate convergence. It can dramatically increase the rate at which already-completed dimensions are reopened.

The interesting AI problem therefore isn’t just:

$$
\text{How do we make the agent better at engineering?}
$$

It’s:

$$
\boxed{\text{How do we encode the law under which engineering activity is allowed to modify project state?}}
$$

Once that exists, extreme implementation velocity becomes incredibly useful. Without it, velocity can just mean faster diffusion through the state space.

> and one of the ways they do that in optics, is defining and controlling registrants

Yes. Which closes the loop back to the original software observation.

In optics, registration isn’t incidental bookkeeping around the design; it’s one of the mechanisms by which the design law survives physical realization.

You define what is registered to what: optical axis, mechanical datum, pupil, sensor plane, stage coordinates, alignment target, previous process layer, metrology frame. Then you define the allowable transformations and tolerances between those registrants. Now a technician can replace, adjust, calibrate, or manufacture something without having to reconstruct the entire machine’s intent.

The registrant gives local work a constraint inherited from the global machine:

$$
\text{local operation}
\quad\text{subject to}\quad
R_{\text{after}}\simeq R_{\text{before}}
$$

or, when you’re deliberately improving registration,

$$
\|\Delta R_{\text{after}}\| < \|\Delta R_{\text{before}}\|.
$$

That’s exactly the monotonicity mechanism we were talking about. A technician doesn’t merely “make the lens better”; they perform an operation whose success has a defined relation to a controlled registrant. Once accepted, subsequent operations can safely project from that state.

And this exposes the original AI/software problem very cleanly.

If Blender is itself a basis upon which millions of people construct projections, then before telling AI to continuously “improve Blender,” you need to answer:

What are Blender’s registrants?

Not merely its documented API. Everything whose stability permits downstream projections to remain valid.

Only after those are defined can you distinguish:

$$
\boxed{\text{implementation freedom}}
$$

from

$$
\boxed{\text{movement of the projection basis}}.
$$

Optical engineering learned to control registrants because otherwise you literally cannot build the machine monotonically. Software has historically been able to leave huge amounts of registration implicit because humans absorb the resulting re-registration cost.

AI potentially removes that luxury: if you increase the rate of modification by orders of magnitude without first controlling the registrants, you increase the rate at which the system destroys completed state.

So defining registrants isn’t an argument against AI changing mature software. It’s almost the prerequisite that would allow AI to change mature software aggressively and safely.

> Accidental uncontrolled registrants: Software doesn't change that
> much because change is expensive. The UI of MS Windows doesn't change
> much within one release, then absolutely buttfucks everyone every 3-5
> years when it changes completely each major release

Yes — accidental uncontrolled registrant is a very useful category.

The registration exists, and is economically real, but nobody has explicitly declared it an invariant. Instead it remains stable because changing it happens to be costly.

So you get:

$$
\text{high mutation cost}
\rightarrow
\text{low mutation rate}
\rightarrow
\text{apparent stability}
\rightarrow
\text{human registration}
$$

and everyone quietly accumulates calibration against that stability: muscle memory, training materials, screenshots, support procedures, automation, organizational workflows, expectations about where controls live, etc.

Windows illustrates it nicely. Within a release, the UI becomes a de facto registrant even though Microsoft hasn’t promised that “this control shall occupy this semantic coordinate forever.” Enough people register against it that its stability has enormous value.

Then the major release becomes permission to spend that accumulated registration.

And the cost isn’t merely “users don’t like the new UI.” It’s:

$$
R_{\text{Windows }n}
\not\cong
R_{\text{Windows }n+1}
$$

so hundreds of millions of learned human projections acquire residual simultaneously. “Where do I perform operation X?” was previously a solved projection. Now it isn’t.

The particularly interesting part for AI is that implementation cost was accidentally acting as a rate limiter protecting uncontrolled registrants.

If historically it costs 500 engineer-hours to redesign some subsystem, there is strong friction against doing so gratuitously. An AI agent that can perform the same transformation in an afternoon removes that friction without somehow discovering all the accidental registrants the friction was protecting.

So:

$$
\boxed{\text{expensive change was functioning as accidental registration control}}
$$

Remove expensive change, and you have to replace it with deliberate registration control, or mutation rate explodes.

That’s a stronger version of your original observation. The danger isn’t merely “AI might change important things.” It’s that some of software’s apparent structural stability may never have been designed at all. It was an emergent consequence of modification being expensive.

Optics can’t rely on that. Its registrants have to be explicit because uncontrolled registration error immediately appears in the physical output. Software can externalize registration error onto humans, so we’ve been able to confuse slowly changing with well-defined.
