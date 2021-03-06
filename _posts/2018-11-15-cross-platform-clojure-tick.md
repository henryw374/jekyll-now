---
layout: slide
title: Cross Platform Awesomeness
description: In many domains, writing code that runs cross platform is a big win. However, there's more to this than changing file extensions to .cljc. This talk explains the why and how of cross platform Clojure and demonstrates a new date/time library 'tick' that goes a long way to enabling cross platform programs.
theme: simple
transition: slide
---

<section data-markdown>
## Cross Platform Awesomeness

### by Henry Widd

#### @henryw374

</section>

<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
![alt text]({{ site.baseurl }}/images/plumbing-brain.svg  "How apps should be")
Note:
we understand that the aim is to write pure functions and have them wired together by the plumbing

the pure functions are the decision makers, the brains of the app

pure functions accept and return data:

no async constructs: promises, core.async channels, manifold deferred's etc.
no mutable objects - e.g. atoms 
database connections

any IO is out - that stays at the edge of the system

Threading/task management

database connections etc -

questionable:
datomic db
library/framework gunk: re-frame, lacinia resolvers, interceptors

</section>


<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
### Question: Is all your brains code cross platform?
Note:
(in .cljc files)
Could it be?
</section>


<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
### Why Do that?
Note:
Clients keep asking me to build apps that involve both server and client components.
Prefer to avoid Node for the server platform if possible
Often the case you need shared brains on server and client
  - some brains need to be on the server
  - responsive app, server does the validation
  - server needs to generate similar data views to the ui - e.g. send in email
  - client needs to construct queries and the server deconstructs
Nice to have aspect, keeps me honest - not forced to do this by Clojure
Work with the same data (and specs) by default - 
avoid... oh, camel case here because come via our rest service and snake on the server, or big 2-d result set on server and different on client
ns containing decisions about some concept, another with the specs for that concept

</section>


<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
## What gets in the way? 
(IOW What forces us to include more reader conditionals?)
 
Note:
### Platform Differences
Concurrency model etc not an issue for what we're talking about here
Numbers

Need to drop to native string, date functions

Reminder: We're not doing IO for this bit 
### Clojure(Script)
Not total fidelity between the two (e.g. no equivalent of clojure.core/extend in ClojureScript)

Interop code

### Libraries

.... ah, libraries. Thankfully, there are lots of cross-platform libraries since the introduction of cljc
and quite a few that aren't yet.
 </section>

<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
## Every application .cljc file ever
```clojure
#?(:clj  [clj-time.core :as ct]
   :cljs [cljs-time.core :as ct])
```
Note:
to date... the only way to do cross platform date logic
Most applications I write involve significant date logic
this works ok for some use cases
cross platform or not, chances are you are using these either directly or indirectly
</section>


<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
### Tick 
A replacement for the clj-time and cljs-time libraries
Note:
clj-time - wrapper over Joda-Time (well loved)

Ubiquitous in clj land

joda-time improved upon by java.time - which comes with the platform (so now common ground for libraries etc)

cljs-time - partial fidelity with clj-time fns, wrapper over js/Date
          - locals not fully supported, and wrapping platform date
          - timezones not supported

need a way to convey date info on wire
aside - what boxes does the wrapper tick (functional api, clojure-y api (less verbose), add features)
</section>

<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
#### java.time (jsr310) date objects
![jsr310 domain]({{ site.baseurl }}/images/jsr310-domain.svg  "jsr310-domain" ) 
Note:
easier to grok than joda time
separation of machine and human time
</section>



<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
## Tick
* Intuitive api for jsr310 domain
* Interval Calculus
* Clocks
* Data literals
* ... for Clojure and Clojurescript!

Note:
compose, decompose, shift etc etc
see docs and Malcolm's clojure exchange talk for info on intervals
data literals handy for the repl, storing dates
drop to java.time when you need to. stackoverflow friendly. applies to cljs
</section>

<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
## Tick Origins
* brainchild of and mainly authored by Malcolm Sparks
* originally scheduling focused
* now full api for working with java.time + intervals
* I started work on cljc'ing clojure.java-time in early 2017 (then abandoned)
    * ... later I teamed up with Malcolm to make Tick cross-platform early in 2018
    * and create data literals
    

Note:
</section>

<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
### The Ecosystem
![alt text]({{ site.baseurl }}/images/tick-ecosystem.svg  "Tick ecosystem")
</section>

<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
### clojure.java-time
* API for jsr-310 domain
* JVM only
* Additionally has
    * conversions to/from joda-time and java.sql objects. 
    * extension for threeten-extra
</section>


<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
## Future Work
* tc39/proposal-temporal
* spec fns (and generators, along the lines of s/inst-in)
* scheduling
</section>

<section data-markdown="" data-separator="^\n\n\n" data-separator-vertical="^\n\n" data-separator-notes="^Note:">
## Start using it!
[github](https://github.com/juxt/tick)
</section>

