# ADR 1: Grappling with the Complexity of Our Domain

Frederik “Frikki” Krautwald <fkrautwald@sparks.network>, 
Tylor Steinberger <tsteinberger@sparks.network>, 
Jeremy Wells <jwells@sparks.network>,
Bruno Couriol <bcouriol@sparks.network>, 
Stephen “Stevo” DeBaun <sdebaun@sparks.network>, 
Johannes “Hannes” Bezuidenhout <jbez@sparks.network>, 
Andi Parker <aparker@sparks.network>

## Status

**Proposed** | ~~Accepted~~ | ~~Depreciated~~ | ~~Superceded~~

## Context

> Design is not just what it looks like and feels like.
> Design is how it works.
>
> —Steve Jobs

We strive to produce quality in the software we develop. We achieve some 
quality by using tests to help us avoid delivering software with a fatal number 
of bugs. Yet, even if we could produce completely bug-free software, that, in 
itself, does not necessarily mean that a quality software model is designed. We 
can reach higher for a well-designed software model that explicitly reflects 
the intended business objective.

The software development approach called _Domain-Driven Design_, or _DDD_, 
exists to help us more readily succeed at achieving high-quality software model 
designs. When implemented correctly, DDD helps us reach the point where _our 
design is exactly how the software works_.

### How to do DDD

Without touching heavy implementation discusions, one of the most empowering 
features of DDD is _Ubiquitous Language_. It is one of the two primary pillars 
of DDD’s strengths, the second being the **Bounded Context**. One cannot 
properly stand without the other.

> **Terms in a Context**
> 
> Think of a Bounded Context as a conceptual boundary around a whole 
> application or finite system. The reason for this boundary is to highlight 
> that every use of a given domain term, phrase, or sentence—the Ubiquitous 
> Language—inside the boundary has a specific contextual meaning. Any use of 
> the term ouside that boundary could, and probably does, mean something 
> different.

#### Ubiquitous Language

The Ubiquitous Language is a shared team language. It’s shared by domain 
experts and developers alike. In fact, it’s shared by everyone on that project 
team. No matter our roles on the team, since we are on that team, we use the 
Ubiquitous Language of the project.

This is no gimmick to get developers to be on the same page as domain experts. 
It is not just a bunch of business jargon being forced upon developers. It’s a 
real language that is created by the whole team—domain experts, developers, 
business analysts, everyone involved in producing the system. Suffice it to say 
that when multiple domain experts are involved in creating the Language, they 
often disagree ever so slightly on the terms and meanings of what they thought 
were already ubiquitous.

In the table below we not only model the provision of perks in code, but the 
team must also speak the Language openly. When the team discusses this aspect 
of the model, they literally speak phrases such as “Event organizers provide 
perks to volunteers according to each opportunity offered.”

There will be haggling and wrangling over the Language that exists in the minds 
of experts and what evolves from there. It’s all part of the natural 
progression of developing the best Language that will matter a lot for a long 
time. This happens through open discussion, looking at exisitng documents, 
business tribal knowledge that finally surfaces, as well as referencing 
standards, dictionaries, and thesauruses. There’s also a point reached where we 
come to terms with the fact that some words and phrases don’t aptly fit the 
business context as well as we might once have thought, and we realize that 
others fit it much better.

##### Analyzing the Best Model for Sparks.Network

| Possible Viewpoints                                                           | Resulting Code |
|-------------------------------------------------------------------------------|----------------|
| _Who cares? Just code it up._                                                 | `volunteer.setPerkType(PerkTypes.TYPE_DISCOUNT);`<br>`volunteer.setAmount(amount);`<br>`volunteer.setEventOrganizer(eventOrganizer);` |
| _We provide perks to volunteers._                                             | `volunteer.givePerks();` |
| _Event organizers provide perks to volunteers according to each opportunity._ | `perks<Perks> = opportunities.standardVolunteerPerks();`<br>`eventOrganizer.providePerks(volunteer, perks);` |

### The Business Value of Using DDD

The value and benefits of using DDD is summarized here:

1. Sparks.Network gains a useful model for its domain.

2. A refined, precise definition and understanding of Sparks.Network is developed.

3. Domain experts contribute to software design.

4. A better user experience is gained.

5. Clean boundaries are placed around pure models.

6. Sparks.Network architecture is better organized.

7. Agile, iterative, continuous modeling is used.

8. New tools, both strategic and tactical, are employed.

#### 1. Sparks.Network Gains a Useful Model of Its Domain

We don’t over-model. We focus on the Core Domain. Other models exist to support 
the Core Domain and are important, too. Yet, the supporting models may not be 
given the priority and effort of the Core Domain.

#### 2. A Refined, Precise Definition and Understanding of Sparks.Network Is Developed

Sparks.Network may actually come to understand itself and its mission better 
than before. As the model is refined over time, Sparks.Network develops a deep 
understanding that can serve as an analysis tool. Details surface out of the 
minds of our domain experts as we are challenged by one another and shaped by 
technical team partners. These details can help Sparks.Network analyze the 
value of the current and future direction, both strategic and tactical.

#### 3. Domain Experts Contribute to Software Design

There is business value when Sparks.Network grows a deeper understanding of the 
core business. Domain experts don’t always agree on concepts and terminology. 
Sometimes the differences are fostered by different experiences from outside 
before joining Sparks.Network. Sometimes it happens because of the divergent 
paths taken by each expert within Sparks.Network. Yet, when brought together to 
a DDD effort, the domain experts gain concensus among themselves. This 
fortifies the effort and Sparks.Network as a whole.

Developers now share a common Language as a unified team along with domain 
experts. They benefit further from the domain experts they work with. As 
developers inevitably move on, either to a new Core Domain or out of 
Sparks.Network, training and handoffs are easier. The chances of developing 
"tribal knowledge" where only a select few understand the model, are reduced. 
The experts, remaining developers, and new ones continue to share a common 
knowledge that is available to anyone in Sparks.Network who requires it. This 
advantage exists because there remains an expressed goal to adhere to the 
Language of the domain.

#### 4. A Better User Experience Is Gained

When software leaves too much to the understanding of its users, users must be 
trained to make a greater number of decisions. In essence, the users are only 
transfering the understanding in their minds into data that they enter into 
forms. The data is then saved to a data store. If users don’t understand 
exactly what is needed, the results are incorrect. Often this leads to 
guesswork with related lowered productivity until user can figure out the 
software.

When the user experience is designed to follow the contours of the underlying 
expert model, users are led to correct conclusions. The software actually 
trains the users, which reduces the training overhead to Sparks.Network. 
Quicker to productivity with less training—that’s business value.

#### 5. Clean Boundaries Are Placed around Pure Models

The technical team is discouraged from doing what might appeal to their 
programming and algorithmic interests by aligning expectations with business 
advantage. Purity in direction allows for focus on the potency of the solution, 
with efforts directed to where they matter the most.

#### 6. Sparks.Network Architecture Is Better Organized

When Bounded Contexts are well understood and carefully partitioned, all teams 
in Sparks.Network develop an acute understanding of where and why integrations 
are necessary. The boundaries are explicit, and the relationships between them 
are as well. The teams that have models that intersect by usage dependency 
employ Context Maps to establish formal relationships and ways to integrate. 
This usually leads to a very thorough understanding of the entire enterprise 
architecture.

#### 7. Agile, Iterative, Continuous Modeling Is Used

The word _design_ can evoke negative thoughts in the minds of business 
management. However, DDD is not a heavyweight, high-ceremony design and 
development process. DDD is not about drawing diagrams. It’s about carefully 
refining the mental model of domain experts into a useful model for 
Sparks.Network. It is not about creating a real-world model, as in trying to 
mimic reality.

The team’s efforts follow an agile approach, which is iterative and 
incremental. Any agile process that the team feels comfortable with can be used 
successfully in a DDD project. The model that is produced is the working 
software. It is refined continuously until it is no longer needed by 
Sparks.Network.

#### 8. New Tools, Both Strategic and Tactical, Are Employed

A Bounded Context gives the team a modeling boundary in which to create a 
solution to a specific business problem domain. Inside a single Bounded Context 
is a Ubiquitous Language formulated by the team. It is spoken among the team 
and in the software model. Disparate teams, sometimes each responsible for a 
given Bounded Context, use Context Maps to strategically segregate Bounded 
Contexts and understand their integrations. Within a single modeling boundary, 
the team may employ any number of useful tactical modeling tools: 
**Aggregates**, **Entities**, **Value Objects**, **Services**, 
**Domain Events**, and others. 

### Determing the Complexity of Our Project

Primarily we want to use Domain-Driven Design (DDD) in the areas that are most 
important to our business. We don’t invest in what can be easily replaced. _We 
invest in the nontrivial, the more complex things, the most valuable and 
important things that promises to return the greatest dividends._ Rightly, 
then, we need to grasp what _complex_ means.

Rather than determining what is _complex_, it may be easier to determine what 
is _nontrivial_.

We will use this scorecard to determine whether our project qualifies for an 
investment in DDD. When a row in the scorecard describes our project, we place 
the corresponding number of points in the right-hand column. We then tally all 
the points for our project. If it’s 7 or higher, we should seriously consider 
using DDD.

| If Our Project ...                                                                                                                | Points | Supporting Thoughts                                                                                                                                   | Our Score |
|-----------------------------------------------------------------------------------------------------------------------------------|-------:|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------:|
| ... is completely data-centric and truly qualifies for a pure CRUD solution.                                                      |      0 |                                                                                                                                                       |         0 |
| ... requires just 30 or fewer business operations, i.e., no more than 30 total user stories or use case flows with minimal logic. |      1 | We are talking about 25 to 30 single business methods, not 25 to 30 whole service interfaces, each with multiple methods.                             |           |
| ... has user stories or use case flows in the range 30 to 40?                                                                     |      2 | Complexity is often not recognized soon enough.                                                                                                       |           |
| ... will grow in complexity.                                                                                                      |      3 | Are domain experts already asking more complex features? It is likely to become complex. Or are they bored with features? It is probably not complex. |           |
| ... features are going to change often over a number of years, and we can’t anticipate that the kinds of changes will be simple.  |      4 | DDD can help to manage the complexity of refactoring our model over time.                                                                             |           |
| ... is new and we don’t understand the Domain. I.e., nobody has done this before.                                                 |      5 | Talk to domain experts and experiment with models to get it right.                                                                                    |           |

## Decision

## Consequences