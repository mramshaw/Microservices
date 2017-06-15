# Microservices
## Some thoughts on Micro-Services

In the software world, the first gradual step seem to be introducing __CI__ (Continuous Integration). This is probably a software engineering _"best practice"_ aimed at catching build-breaking issues as early as possible (it's pretty much a rite of passage for newer team members to _"break the build"_ - if you commit a lot of code you will probably break the build sooner or later). 

The reason why this is a big deal is that any build-breaking issues delay the overall release - _EVERYONE_ is blocked until the build is fixed (which is why advance warning of these issues is so important - and also why Micro-Services are so critical for larger codebases {more on this further down}). If you have ever worked at a start-up where funding depended on meeting release targets you will know how critical it is to be able to release in a timely fashion.

Once __CI__ is nailed down, the next step is generally some form of automated integration testing followed by __CD__ (Continuous Deployment) - usually aimed towards a staging area (the actual roll-out to production probably still happens according to a release cycle at this point - generally because it requires full documentation).

The software is now what is known as a __monolith__ or __BBOM__ (Big Ball of Mud). Which is probably fine for smaller codebases or where the developers are well-integrated and experienced. Teams (if they exist at all at this point) are usually vertically-oriented around core competencies (web, front-end, middleware, database, back-end). There are tactics and strategies for advancing to the next step (if needed).

The next step is essentially ___cross-cutting___ - meaning it may well involve some reorganization (sometimes called "The Inverse Conway Manoeuvre") but will definitely introduce increased complexity. The usual approach is to pick a non-essential service to be used as a pilot project - and to tread very carefully. The terminology is helpful here - this is referred to as _"__strangling out a micro-service__"_. Signs that this may be necessary are when a development team is too large for the codebase (i.e. developers start stepping all over each other's code changes) or else there is a desire to be able to release different parts of the codebase independently of each other (for example, different Docker images). The front (customer-facing) end generally changes a lot faster (for cosmetic reasons) than the back-end, which might be a viable reason for separating these into two deliverables. [The eventual goal is to partition the system a lot more finely than this, but this is just a broad example of how different functional areas mutate at different speeds.]

_Coupling_ and _Cohesion_ are issues here - the acid test for a Micro-Service is that it can be released on its own. If a new Docker image requires another Docker image (or - worse - multiple Docker images) to also be released at the same time, this is an indication of tight coupling.

The reorganization (if it occurs at all) will generally be towards something like _product teams_ (perhaps with a product manager) and may involve slicing up the vertically-oriented core competency teams into horizontally-oriented product teams (like Amazon's so-called _"__two-pizza teams__"_). The general guideline is that each team should control the entire pipeline from the software to the customer.

How the slicing and dicing occurs is tricky. Eric Evans's ___Domain-Driven Design___ (the so-called _"Blue Book"_; its subtitle is _"Tackling Complexity in the Heart of Software"_) offers some guidelines on where to draw the boundaries between these _"bounded contexts"_ (which is one of the trickiest problems around). The point is that the software should be partitioned based upon _business_ (rather than _technical_) needs. ___Conway's Law___ is worth a look at this point:

  https://en.wikipedia.org/wiki/Conway%27s_law

For those who arrive at the Micro-Services stage, the business rewards are supposed to be huge; each micro-service can be rapidly iterated (speed of iteration is often cited as a competitive business advantage) and each micro-service can be independently tested (each _bounded context_ is also a _failure boundary_). Each micro-service team is free to innovate, also to use whichever language or stack they feel best suits their needs (subject to management approval of course) and the overall communications overhead is reduced as each team has its own internal scope - so that communication is simply needed to coordinate teams. The disadvantages are that the 'core competency' teams may have been broken up and the release process may have gotten more difficult (scripting and automation are generally used extensively to mitigate this last issue).

The problem with micro-services is that bugs generally show up at higher levels, which can be problematic. So integration testing gets harder and more complicated (_bulkheads_ and _circuit-breakers_ may be of use here). The general "fullstack" Kool-aid is that developers should do their own testing and releasing, however this particular issue may well argue for the continued existence of QA/Testing/Release teams. On the plus side, problems in production can be quickly rolled back small-scale; a more granular release means finer control over the actual components. This makes for a much more _"__agile__"_ experience.
