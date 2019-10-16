# Split technical and business code

It's not because the code is written by the same people, with the same technology and for the same project that it is all equal.

There is a huge difference between technical and business code in term of goals and life-cycle, so we should identify them clearly and isolate them as much as possible.
Not doing that will produce maintenance nightmares.

**Technical code** is the part with implementation details and very technical jargon.
Most of the time it is understandable only by developers with (more or less) deep knowledge of the used technologies (language, libraries, databases, concepts...).
Generally it evolves for technical reasons such as library upgrade, performance issues, architecture change ... but product changes should not impact it.
It can be a good idea to make it as generic as possible and package it as a dedicated project/module to isolate it and maybe reuse in other projects.
This can lead to advanced feature use and sometimes hacky implementation for performance reasons but it should expose a simple interface to be used in business code.

**Business code** is where we implement application requirements.
It should be easily readable by any developer (even beginners or ones with no experience in this technology) and also (ideally) product owners, managers, support people, designers or marketing people.
No technical concerns should surface in it and functions and values should have business names (see DDD).
This is where most of the changes will be done when adding or changing features. All the efforts must be done to improve its readability and coherence.
No technical upgrade should impact it and it can be a good idea to hide every technical detail inside a wrapper to have a good isolation.
Generally, it does not have direct dependencies on external libraries.
