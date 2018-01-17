# Microservices
## Some thoughts on Micro-Services

In the software world, the first gradual step seem to be introducing __CI__ (Continuous Integration). This is probably a software engineering _"best practice"_ aimed at catching build-breaking issues as early as possible (it's pretty much a rite of passage for newer team members to _"break the build"_ - any developer who commits a lot of code will probably break the build sooner or later). 

The reason why this is a big deal is that any build-breaking issues delay the overall release - _EVERYONE_ is blocked until the build is fixed (which is why advance warning of these issues is so important - and also why Micro-Services are so critical for larger codebases {more on this further down}). Anyone who has ever worked at a start-up where funding depended on meeting release targets will know how critical it is to be able to release in a timely fashion.

Once __CI__ is nailed down, the next step is generally some form of automated integration testing followed by __CD__ (Continuous Deployment) - usually aimed towards a staging area (the actual roll-out to production probably still happens according to a release cycle at this point - generally because it requires full documentation).

The software is now what is known as a __monolith__ or __BBoM__ (Big Ball of Mud). Which is probably fine for smaller codebases or where the developers are well-integrated and experienced. Teams (if they exist at all at this point) are usually vertically-oriented around core competencies (web, front-end, middleware, database, back-end, etc). There are tactics and strategies for advancing to the next step (if needed).

The next step is essentially ___cross-cutting___ - meaning it may well involve some reorganization (sometimes called _"The Inverse Conway Manoeuvre"_) but will definitely introduce increased complexity. The usual approach is to pick a non-critical service to be used as a pilot project - and to tread very carefully. The terminology is helpful here - this is referred to as _"__strangling out a micro-service__"_. Signs that this may be necessary are when a development team is too large for the codebase (i.e. developers start stepping all over each other's code changes) or else there is a desire to be able to release different parts of the codebase independently of each other (for example, different __Docker__ or __rkt__ images). The front (customer-facing) end generally changes a lot faster (for cosmetic reasons) than the back-end, which might be a viable reason for separating these into two deliverables. [The eventual goal is to partition the system a lot more finely than this, but this is just a broad example of how different functional areas mutate at different speeds.]

_Coupling_ and _Cohesion_ are issues here - the acid test for a Micro-Service is that it can be released on its own. If a new Docker image requires another Docker image (or - worse - multiple Docker images) to also be released at the same time, this is an indication of tight coupling. While this is very probably a good start in terms of migrating to the cloud, the eventual goal should be to have independent micro-services.

The reorganization (if it occurs at all) will generally be towards something like _product teams_ (perhaps with product managers) and may involve slicing up the vertically-oriented core competency teams into horizontally-oriented product teams (like Amazon's so-called _"__two-pizza teams__"_). The general guideline is that each team should control the entire pipeline from the software to the customer.

How the slicing and dicing occurs is tricky. Eric Evans's ___Domain-Driven Design___ (the so-called _"Blue Book"_; its subtitle is _"Tackling Complexity in the Heart of Software"_) offers some guidelines on where to draw the boundaries between these _"bounded contexts"_ (which is one of the trickiest problems around).

Here are some further thoughts from Eric Evans, discussing
[Domain-Driven Design at 10 Years](http://www.se-radio.net/2015/05/se-radio-episode-226-eric-evans-on-domain-driven-design-at-10-years/):

> The bounded context is a concept which in more traditional architectures, there
> weren't very good ways to implement that concept - you know, to really establish
> the boundary - so it seems to me that microservices has delivered us a practical
> and popular way of defining - and sticking to - those boundaries. And that's a
> big deal. And the emphasis on, you know, the micro: someone once asked me, what's
> the difference between microservices and the old SOA services? And I said: Well,
> I think part of it is the micro. These services are ... smaller. That's just a
> convention, of course, but it's an important convention: The idea that a very
> small piece of software could be very isolated, and uh, you know, do a narrow
> thing.

The point is that the software should be partitioned based upon _business_ (rather than _technical_) needs. ___Conway's Law___ is worth a look at this point:

  https://en.wikipedia.org/wiki/Conway%27s_law

For those who arrive at the Micro-Services stage, the business rewards are supposed to be huge; each micro-service can be rapidly iterated (speed of iteration is often cited as a competitive business advantage) and each micro-service can be independently tested (each _bounded context_ is also a _failure boundary_). Each micro-service team is free to innovate, also to use whichever language or stack they feel best suits their needs (subject to management approval of course) and the overall communications overhead is reduced as each team has its own internal scope - so that communication is simply needed to coordinate teams. The disadvantages are that the 'core competency' teams may have been broken up and the release process may have gotten more difficult (scripting and automation are generally used extensively to mitigate this last issue).

The problem with micro-services is that bugs generally show up at higher levels, which can be problematic. So integration testing gets harder and more complicated (_bulkheads_ and _circuit-breakers_ may be of use here). The general "fullstack" Kool-aid is that developers should do their own testing and releasing:

  ___“You build it, you run it.”___ - Werner Vogels, Amazon CTO

However, the issue of the increased complexity of integration testing of micro-services may well argue for the continued existence of QA/Testing/Release teams. On the plus side, problems in production can be quickly rolled back small-scale; a more granular release means finer control over the actual components. This makes for a much more _"__agile__"_ experience.

This new agility manifests in partial or rolling deployments such as _canary deployments_, _blue-green deployments_, _red-black deployments_, _A/B deployments_ and the like. The key value of these is that performance impacts can be evaluated __in production__ and - if necessary - tuned or rolled-back. [A traditional approach has been a phased regional deployment - which assumes that all regions have equivalent performance characteristics. As this is usually __not__ the case, being able to monitor trial deployments of micro-services represents an attractive proposition.]

Partial or rolling deployments are really a feature of component-based services - and are not specific to micro-services. But, as discussed earlier, micro-services probably represent the desired end-goal of component-based services.

One particularly attractive feature of this approach is that it is possible to make __micro-releases__ - as in release individual changes (which could be either bug fixes or requested new features) instead of a bundle of them - making it much easier to evaluate impact. Obviously the smaller the change to production, the easier it is to debug or diagnose. Conversely, larger change bundles may be much harder to evaluate or triage. In terms of testing, there are two types of errors: ___Type I___ is when the change is not implemented correctly and ___Type II___ represents an unintended breakage - meaning that something that wasn't supposed to be affected actually _was_ affected. Either of these errors may be a result of differences between the production and development environments so being able to test in production (as in a _canary deployment_) can obviously be helpful. Type I (which is usually less common) is normally caught by _unit testing_ (as in __TDD__ or Test-Driven Development) whereas Type II is normally caught by _integration testing_ or _QA_ (Quality Assurance). The general point is that all of these efforts are reduced by smaller deployments (it may be necessary to release deployments more frequently to compensate for their smaller size and scope).
