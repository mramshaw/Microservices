# Microservices
Some thoughts on Micro-Services

In the software world, the first gradual step seem to be introducing CI (Continuous Integration). This is probably a software engineering "best practice" aimed at catching build-breaking issues as early as possible (it's pretty much a rite of passage for newer team members to "break the build" - if you commit a lot of code you will probably break the build sooner or later). 

The reason why this is a big deal is that any build-breaking issues delay the overall release - EVERYONE is blocked until the build is fixed (which is why advance warning of these issues is so important - and also why Micro-Services are so critical for larger codebases {more on this further down}). If you have ever worked at a start-up where funding depended on meeting release targets you will know how critical it is to be able to release in a timely fashion.

Once CI is nailed down, the next step is generally some form of automated integration testing followed by CD (Continous Deployment) - usually aimed towards a staging area (the actual roll-out to production probably still happens according to a release cycle at this point - generally because it requires full documentation).

The software is now what is known as a 'monolith' or BBOM (Big Ball of Mud). Which is probably fine for smaller codebases or where the developers are well-integrated and experienced. Teams (if they exist at all at this point) are usually vertically-oriented around core competencies (web, front-end, middleware, database, back-end). There are tactics and strategies for advancing to the next step (if needed).

The next step is essentially cross-cutting - meaning it may well involve some reorganization but will definitely introduce increased complexity. The usual approach is to pick a non-essential service to be used as a pilot project - and to tread very carefully. The terminology is helpful here - this is referred to as "strangling out a micro-service". Signs that this may be necessary are when a development team is too large for the codebase (i.e. developers start stepping all over each other's code changes) or else there is a desire to be able to release different parts of the codebase independently of each other (for example, different Docker images). The front (customer-facing) end generally changes a lot faster (for cosmetic reasons) than the back-end, which might be a viable reason for separating these into two deliverables.

Coupling and Cohesion are issues here - the acid test for a Micro-Service is that it can be released on its own. If a new Docker image requires another Docker image to also be released at the same time, this is an indication that the two are tightly-coupled.

The reorganization (if it occurs at all) will generally be towards something like product teams (perhaps with a product manager) and may involve slicing up the vertically-oriented core competency teams into horizontally-oriented product teams (like Amazon's so-called "two-pizza teams"). The general guideline is that each team should control the entire pipeline from the software to the customer.

How the slicing and dicing occurs is tricky. Eric Evans's Domain-Driven Design (the so-called "Blue Book"; it's subtitle is 'Tackling Complexity in the Heart of Software') offers some guidelines on where to draw the boundaries between these "bounded contexts" (which is one of the trickiest problems around). The point is that the software should be partitioned based upon business (rather than technical) needs. Conway's Law is worth a look at this point:

  https://en.wikipedia.org/wiki/Conway%27s_law

For those who arrive at the Micro-Services stage, the business rewards are supposed to be huge; each micro-service can be rapidly iterated (speed of iteration is often sited as a competitive business advantage) and each micro-service can be independently tested (each bounded context is also a failure boundary). The disadvantages are that the 'core competency' teams may have been broken up and the release process may have gotten more difficult (scripting and automation are generally used extensively to mitigate this last issue).

The problem with micro-services is that bugs generally show up at higher levels, which can be problematic. So integration testing gets harder and more complicated (bulkheads and circuit-breakers may be of use here). The general "fullstack" Kool-aid is that developers should do their own testing and releasing, however this particular issue may well argue for the continued existence of QA/Testing/Release teams.
