# Research review

In this review I focused on representational language used for AI planning problems.
3 major developments in this field are **STRIPS**, **ADL** and **PDDL** languages.

First major planning system, STRIPS [1] used world (state) models defined by two clause lists.
The first list, DELETIONS, names all those clauses from the initial world model which are no longer present
in the model being defined.
The second list, ADDITIONS, names all those clauses in the model being define which are not also in the
initial model.
An example below shows the transition between initial state `s0` and following `s1` by using DELETIONS
and ADDITIONS lists:

```
s0: ATR(a, s)
    AT(B,b,s)
    AT(C,c,s)
 
s1: DELETIONS: ATR(a,s)
    ADDITIONS: ATR(c,goto`(a,c,s0))
```

STRIPS language had big influence on representation approach and its first modification called Action
Description Language (ADL) with several relaxations appeared in 1986.
ADL integrates the semantics and much of the expressive power of the situation calculus with the notational
and computational benefits of the STRIPS language [2].
By combining aspects of both, ADL overcomes the limited expressiveness and semantic difficulties of the
STRIPS language without encountering the computational barriers of the situation calculus.

Problem Domain Description Language (PDDL) introduced in [3] was intended to express the "physics" of a domain,
that is, what predicates there are, what actions are possible, what the structure of compound actions is, and
what the effects of actions are.
Noteworthy is that first feature highlighted by authors is *STRIPS-style actions* which another time
emphasizes the influence of STRIPS for all the field.

PDDL became a standard language for many planning problems and it has several extensions [4]:
* PDDL2.1 introduced **numeric fluents** (e.g. time, energy, distance), **plan-metrics** (to allow quantitanive
evaluation of plans), **durative/continuous actions** (with variable, non-discrete length, conditions and effects)
* PDDL2.2 introduced **derived predicates** (to model dependeny of given facts from other facts) and 
**timed initial litersls** (to model exogenous events occuring at given time independently from plan-execution)
* PDDL3.0 introduced **state-trajectory constraints** and **preferences**
* PDDL3.1 is the latest version from 2011 which introduced **object-fluents** and adapted the language to modern
expectations.

## References

1. Fikes, R. E., & Nilsson, N. J. (1971). STRIPS: A new approach to the application of theorem proving to problem solving. Artificial intelligence, 2(3-4), 189-208.
2. Pednault, E. P. (1989). ADL: Exploring the Middle Ground Between STRIPS and the Situation Calculus. Kr, 89, 324-332.
3. McDermott, D., Ghallab, M., Howe, A., Knoblock, C., Ram, A., Veloso, M., ... & Wilkins, D. (1998). PDDL-the planning domain definition language.
4. Planning Domain Definition Language (URL: https://en.wikipedia.org/wiki/Planning_Domain_Definition_Language)