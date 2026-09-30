# Claims about generator distributions in eleven PBT papers

A survey of every claim about *distributions* — formal or informal — made in the
following papers, including places where a generator had to be modified in a way
that changes its distribution.

Papers covered (chronological):

| Paper | Venue |
|---|---|
| Pałka, Claessen, Russo, Hughes — *Testing an Optimising Compiler by Generating Random Lambda Terms* | AST 2011 |
| Fetscher, Claessen, Pałka, Hughes, Findler — *Making Random Judgments* | ESOP 2015 |
| Claessen, Duregård, Pałka — *Generating Constrained Random Data with Uniform Distribution* | JFP 2015 |
| Bendkowski, Grygiel, Tarau — *Boltzmann Samplers for Closed Simply-Typed Lambda Terms* | PADL 2017 |
| Midtgaard, Justesen, Kasting, Nielson, Nielson — *Effect-Driven QuickChecking of Compilers* | ICFP 2017 |
| Lampropoulos, Paraskevopoulou, Pierce — *Generating Good Generators for Inductive Relations* | POPL 2018 |
| Foner, Zhang, Lampropoulos — *Keep Your Laziness in Check* | ICFP 2018 |
| Giacometti Rocha — *Testing of OCaml Exceptions by Effect-Driven Generation of Programs* | MSc thesis, Edinburgh 2019 |
| Zhang, Roth, Pierce, Roth, Haeberlen — *Testing Differential Privacy with Dual Interpreters* | OOPSLA 2020 |
| Frank, Quiring, Lampropoulos — *Generating Well-Typed Terms that are not "Useless"* | POPL 2024 |
| Tjoa, Garg, Goldstein, Millstein, Pierce, Van den Broeck — *Tuning Random Generators* | OOPSLA 2025 |

---

## Pałka, Claessen, Russo, Hughes — *Testing an Optimising Compiler by Generating Random Lambda Terms* (AST 2011)

Has an explicit §5.2 titled **"Distribution"**:

- "The distribution of generated terms is **ad-hoc**, but produces an acceptable
  rate of terms that trigger failures—**we have tweaked it to achieve good
  results in our own testing**." Weights: 4 for locally-bound-variable rules, 2
  for constants, 8 for app/λ, 6 for `seq`.
- "It is unclear what the **'best' distribution** would be. There is no reason to
  believe, for example, that a **uniform distribution over terms of a specific
  size would be more effective at revealing bugs, and it might well be less
  so**." They disclaim approximating "programs a real programmer could write,"
  and conclude: "we are pragmatic, and consider that **success in finding bugs is
  the most important measure of a good distribution**."

### Generator modifications that change the distribution (§5, §5.1)

- **Polymorphic instantiation.** "Instead of instantiating undetermined type
  variables with any possible type, we use a method for **randomly generating
  types that avoids those for which it is impossible to construct a term**" —
  described as "crude, but reasonably effective."
- **Capping `(Indir)` arity.** `(Indir)` on a result-type variable (e.g.
  `id : ∀α.α→α`) admits infinitely many applications, so "**we only allow the
  (Indir) rule to consider at most three extra parameters. This trade-off
  prevents the (Indir) rule from generating some terms**" (mitigated by `(App)`
  still reaching them).
- **Grouping by type.** One `(Indir)` instance per *unique type* in the
  environment, with the concrete symbol chosen at random afterwards — an explicit
  reweighting away from per-symbol uniformity.
- **Limiting recursion of "dangerous" rules.** Rule prioritisation by weight,
  plus: "we also limit the number of 'dangerous' rules—the ones involving
  guessing types—that can be applied recursively. **Undoubtedly, this rules out
  the generation of some complex terms**, but we still generate enough
  interesting terms to find compiler failures."
- Related work notes Moczurad et al./others "employ counting of possible subterms
  to achieve **uniform generation distribution**."

---

## Fetscher, Claessen, Pałka, Hughes, Findler — *Making Random Judgments* (ESOP 2015)

Two rule-ordering strategies, and the paper argues neither distribution is
unambiguously right:

- **Strategy 1** (uniform over permutations of candidate rules): "we have a 1 in
  4 chance of choosing the type rule for numbers, so **one quarter of all
  expressions generated will just be a number**. This **bias towards numbers**
  also occurs when trying to satisfy premises of the other, more recursive
  clauses, so the **distribution is skewed toward smaller derivations**, which
  contradicts commonly held wisdom that bug finding is more effective when using
  larger terms."
- **Strategy 2** (depth-dependent): samples a permutation from a **binomial
  distribution** whose probability is proportional to current-depth / max-depth,
  thereby "**biasing the generation towards rules with more premises early on in
  the search** and thus tending to produce larger terms." Drawback: rules with
  many premises are often unsatisfiable as the first rule, so the uniform
  strategy "still is less than ideal, but is overall more likely to produce terms
  at all." Figure 8 plots the density functions.
- Past the depth bound the generator stops permuting and sorts by fewest premises
  — another deliberate distribution change to terminate quickly.
- Termination heuristics "make the assumptions that the cost of completing the
  derivation is proportional to the size of the goal stack, and that **terminal
  nodes in the search space are uniformly distributed**. Typically these are safe
  assumptions, but not always" (the CPS-transformed `let-poly` model breaks it).

### Generator modified in a way that changes the distribution (§4.2, GHC comparison)

- "Redex was producing **significantly smaller terms** than the hand-written
  generator"; excessive backtracking "**skews the distribution toward smaller
  terms** because these failures become more likely as the size of the search
  space expands."
- Fix: "we built a **new Redex model identical to the first except with a
  pre-instantiated set of constants, removing polymorphism**. We picked the 40
  most common instantiations from a set of counterexamples…" Result: "we get a
  **much better size distribution** with the non-polymorphic model, comparable to
  the hand-written generator's distribution," with counterexample rates from
  1-in-500K at depth 6 to 1-in-320 at depth 8.
- On Pałka's generator: "**Significant effort was spent on adjusting the
  distribution of terms and optimization, even adjusting the type system in
  clever ways.**"
- On Feat: "The enumeration also weights all terms equally, so a random sample of
  values can in some sense be said to have a **more uniform distribution**." On
  Grygiel & Lescanne: filtering closed terms with a typechecker "is somewhat
  inefficient… but it does provide a **uniform distribution**."

---

## Claessen, Duregård, Pałka — *Generating Constrained Random Data with Uniform Distribution* (JFP 2015)

The whole paper is a distribution claim. Key ones:

- **Abstract.** "The distribution of these generators is **uniform over values of
  a given size**… **handwritten generators often have an unpredictable
  distribution of values, risking that some values are arbitrarily
  underrepresented**. We also present a variation of the technique that has
  better performance, but where the **distribution is skewed in a limited, albeit
  predictable way**."
- **Motivating claim about the prior Pałka work.** "it was shown to be possible
  but very tedious to manually construct a generator that (a) could generate
  random well-typed programs… and at the same time (b) **maintain a reasonable
  distribution such that no programs were arbitrarily excluded from
  generation**." Generators "mix concerns we would like to separate: (1)
  structure, (2) properties, (3) **what distribution do we want**."
- **Why not depth.** "useful distributions for sets of trees of depth d are hard
  to find, because there are many more complete trees of depth d than there are
  sparse trees. This may lead to an **overrepresentation of almost full trees**."
- **Why uniform.** "The simplest useful and predictable distribution that **does
  not arbitrarily exclude values** from a set is the uniform distribution… We
  **acknowledge the need for other distributions than uniform** in certain
  applications… We anticipate methods for controlling the distribution of our
  generators in multiple ways, but that remains future work."
- **Uniformity is over *multiset occurrences*, not values.** "whenever we speak of
  uniform sampling procedures it is understood to be uniform over the set of
  occurrences of values, not over the set of values themselves. **Repeated values
  are overrepresented** exactly as one might expect from a uniform sampler of a
  multiset."
- **`Pay` cost assignment.** "the user may choose to assign costs differently,
  which would **change the sizes of individual values and consequently the
  distribution of size-limited generators**."
- **Explicit trade of uniformity for speed (§5.1, "Relaxed uniformity
  constraint").** "We have implemented two alternative algorithms that **violate
  this restriction, compromising uniformity, in favour of better performance**."
  - *Unbounded backtracking*: "no longer uniform… since an arbitrary number of
    backtracking steps is allowed the **distribution of generated values may be
    arbitrarily skewed**. In particular, values satisfying the predicate that are
    'surrounded' by many values for which it does not hold may be much more
    likely to be generated."
  - *Bounded backtracking* (bound *b*): "**'almost uniform' in a precise way: the
    probabilities of generating any two values differ at most by a factor
    b + 1**. So, if we pick b = 1000, generating the most likely value is at most
    1001 times more likely than the least likely value." Generalizes uniform
    (b=0) and unbounded (b=∞). Motivation: "trading the uniformity of the
    distribution for higher performance may lead to a higher rate of finding
    bugs."
- **Nondeterministic predicates can bias the distribution.** The `nonDetB` /
  racing-`oracle` example where "`(False, True)` will never be returned, leading
  to a **biased distribution**" — motivating the assumption of deterministic
  evaluation order.
- §4 proves `uniformFilter` and `uniform` are uniform over
  `{x ∈ sized s k | p x}`.
- §6.2: for handwritten STLC generators, "**Achieving satisfactory distribution
  and performance requires careful tuning, and it is difficult to assess if any
  important values are severely underrepresented** (Palka 2012)."

---

## Bendkowski, Grygiel, Tarau — *Boltzmann Samplers for Closed Simply-Typed Lambda Terms* (PADL 2017)

Quotes are from the extended version,
[arXiv:1612.07682v2](https://arxiv.org/abs/1612.07682), which adds the full
Boltzmann-sampler development and the parallel execution model to the PADL paper.

This is the paper that states non-uniform STLC generation as a *problem*, and it
is the uniform baseline Frank et al. (POPL 2024) measure themselves against.

- **Abstract, framing uniformity as the open problem.** "due to the intrinsically
  difficult combinatorial structure of typable λ-terms **no effective uniform
  sampling method is known, setting it as a fundamental open problem in the random
  software testing approach**." The contribution: "**uniformly random** closed
  simply-typed λ-terms of up size 120," extended to "**uniformly random** closed
  simply-typed normal forms" and to size 140 in parallel.
- **The direct critique of Pałka et al. (2011).** "Though successful for the
  purpose of finding optimisation bugs in GHC, **their random terms were not
  uniformly random with respect to size. In other words, some kinds of typable
  λ-terms were favoured over other kinds of equal size terms.** Uniform
  generation, on the other hand, **assigns equal probability to terms of equal
  size** and hence produces '**typical**' typable λ-terms, **without introducing
  an unintended nor explicit bias in the sampling process**."
- **What Boltzmann sampling actually guarantees.** Uniformity is *conditional on
  size*, and size is a random variable: "we want the probability P(α) that α ∈ A
  of size n is the sampler's outcome to be equal to 1/aₙ"; "Suppose we **relax our
  restriction that the sampler's outcome size is deterministic**." With
  `Px(α) = x^|α| / A(x)` and `Px(N = n) = aₙxⁿ / A(x)`, the explicit caveat is:
  "**in this model we do not control the exact size of the sample, although we can
  calibrate its expected size and standard deviation by choosing a suitable
  parameter x**" (Eq. 2 gives `Ex(N)` and `σx(N)`).
- **Calibration is empirical.** "Following our empirical experiments, **we
  calibrated the branching probabilities so the expected outcome size to 120** –
  the currently biggest practical size achievable," solving `Ex(N) = 120`
  numerically for `x ≈ 0.29558095907`, yielding branching probabilities
  0.35700035696434995 (de Bruijn index), 0.6525813160382378 (abstraction),
  0.7044190409261122 (leaf). The generated code hard-codes these alongside
  `min_size(120)`, `max_size(150)`, `max_steps(10000000)`, and "The Boltzmann
  sampler **can be fine-tuned via `min_size` and `max_size`** to search for terms
  in an interval **for which the probabilities of the sampler have been
  calibrated**."
- **Uniformity is preserved by rejection, but at unbounded cost.** Because
  closed simply-typed terms are asymptotically negligible among plain terms, "a
  naive generate-test-reject sampling scheme becomes inevitably infeasible for
  sufficiently large term sizes," so they interleave sampling with "an optimised
  **anticipated rejection** phase… where undesired terms are discarded as soon as
  it is possible to determine that the (partially) constructed term cannot be
  closed nor typeable. At this point, the whole process is interrupted and
  restarted." The claim and its price: "**Although in effect we obtain a uniform
  sampler for closed simply-typed λ-terms, the power of Boltzmann samplers is
  significantly constrained** – due to the fact that closed simply-typed λ-terms
  are asymptotically negligible in the set of plain λ-terms, **the number of
  expected retrials tends to infinity as the target term size increases**."
- **The size measure itself is a distribution-shaping choice.** They adopt "a
  slight variation of the '**natural size**'… **assigning to each constructor a
  size given by its arity**" (0 for `0`, 2 for application). And on unary de Bruijn
  indices: "`nth_elem/3` **consumes progressively larger size-units for variables
  of a higher de Bruijn index**, a property that **conveniently mimics the fact
  that, in practical programs, variables located farther from their binders are
  likely to occur less frequently** than those closer." (Compare Claessen et al.'s
  `Pay`, where cost assignment likewise changes the size-limited distribution.)
- **A measured sparsity result explaining a distributional limit.** Fig. 1
  tabulates densities: at size 20 there are 16,019,330 closed simply-typed terms
  (1 per ~15.8 plain terms) versus 473,628 simply-typed normal forms (1 per ~60
  normal forms), density ratio 0.263 and falling. Hence "closed simply-typed
  normal forms are becoming very sparse much earlier than their plain
  counterparts," and the conclusion reports "an **intriguing discrepancy** between
  the case of simply-typed terms and simply-typed normal forms. While these two
  classes of terms are both known to asymptotically vanish, the **significantly
  faster sparsity growth of the latter has limited our Boltzmann sampler to sizes
  of order 70**." Left open: whether that density ratio tends to 0, and note
  "**this behaviour could be dependent on the size definition we are using**."
- **Related work draws the uniform / non-uniform line explicitly.** "Other,
  **non-uniform generation**, approaches are also studied in the context of
  automated software verification. Prominent examples include Quickcheck… and
  GAST – two frameworks offering facilities for random (**yet not necessarily
  uniform**) and exhaustive test generation." Pałka et al.'s type-directed
  mechanism is credited with "resulting in **more realistic** (from the particular
  use case point of view) terms." They also cite Tarau's statistical study as
  "indications that some types **frequent in human-written programs** are among
  the **most frequently inferred ones** for terms of a given size."
- **Conclusion.** "We have derived from logic programs for exhaustive generation of
  λ-terms programs that generated **uniformly distributed** simply-typed λ-terms
  via Boltzmann samplers."

*Observation, not a claim of the paper:* §7 parallelises by running identical
`ranTypable` goals in `first_solution` across threads and returning whichever
finishes first. The paper reports only the size/time gains (size 180 in under a
second on 44 cores) and does not discuss whether racing independent samplers and
taking the first winner preserves the uniformity claimed for a single sampler.

---

## Midtgaard, Justesen, Kasting, Nielson, Nielson — *Effect-Driven QuickChecking of Compilers* (ICFP 2017)

Has §6.2 titled **"Distribution"**:

- "**Rather than attempt to generate programs uniformly we skew the
  distribution** in an attempt to generate programs that will stress the compiler
  backends. As traditional we do so by assigning integer weights." Weights:
  `EConst` 6, `EVar` 1, `ELam` 8, `EApp` 8 (4/4 split on which side receives the
  goal effect), `EIndir` 4, `ELet` 6, `EIf` 3, with the caveat that "these
  frequencies do not represent the chance that a particular rule is chosen as a
  whole."
- "Our frequencies **started out according to Pałka et al. [2011] and were later
  adjusted based on experiments. They will most likely be tweaked further in the
  future and clearly do not result in a uniform distribution**."
- They then *measure* the resulting distribution: mean size 64.4, sd 181.0,
  median 12, min 1, max 2672 over 1000 terms — "a **distribution centered around
  smaller programs** (small average and median) albeit with occasional large
  ones," confirmed by a histogram (truncated at 200; 920/1000 programs had size
  1–199).
- **Distribution-changing generator mechanics.** Variables with identical
  type-and-effect signatures are **grouped**, adding `(EIndir)` once per
  signature group rather than once per variable (following Pałka); the goal
  effect is passed to exactly one argument "chosen **uniformly** in [1,…,i]"; and
  non-termination is remedied "by **imposing a size limit** on the generated
  terms, as is standard within QuickChecking," with only axioms
  (`EVar`/`EConst`) applicable at size zero.
- **Explicit non-claim vs. Claessen et al.** They "developed an approach to
  automatically construct generators… that have a **provably uniform
  distribution**. The focus of the present work is different… **We make no formal
  claims of uniformity** but in future work we would like to investigate and
  integrate the approaches of Claessen et al. [2015] in order to do so."

---

## Lampropoulos, Paraskevopoulou, Pierce — *Generating Good Generators for Inductive Relations* (POPL 2018)

- **Motivation for not using generate-and-test.** "worse, the **distribution of
  the lists that we do not discard will be strongly skewed toward short ones**,
  which might fail to expose bugs that only show up for larger inputs."
- On QuickCheck combinators: they exist "for writing custom generators for
  **well-distributed** random values," but writing them "can be both complex and
  time consuming, sometimes to the point of being a research contribution in its
  own right."
- `freq` "gives the user a degree of **local distribution control** that can be
  used to **fine-tune the distribution** of generated data, **a crucial feature
  in practice**." Worked out concretely: `gen_tree_sized` yields a `Leaf`
  `1/(size+1)` of the time and a `Node` `size/(size+1)` of the time.
- **`backtrack` replaces `frequency`** in derived generators: it makes the first
  choice "based on the induced discrete distribution. **However, should the
  chosen generator fail, it backtracks and chooses a different generator** until
  it either exhausts all options or the backtracking limit" — i.e.
  resampling-without-replacement, so it does not induce `frequency`'s
  distribution.
- Derived-generator weights "**can be chosen by the user via lightweight
  annotations**, similar to the local distribution control of Luck."
- **Their correctness results are explicitly distribution-free.** The
  verification framework "assigns semantics to each generator by mapping it to
  its **set of outcomes**, i.e. the elements that have **non-zero probability** of
  being generated," supporting only soundness and completeness proofs — nothing
  about probabilities.
- **IFC case study.** Derived generators were "1.75× slower than the
  corresponding handwritten ones, **while producing the same distribution and
  bugfinding performance**." To reproduce the handwritten generator's
  distribution they had to add weights: "**we can achieve the same distribution
  with a weight annotation before deriving generators**"
  (`QuickChickWeights [(GoodStackCons, 10); (GoodStackRet, 4)]`), and verified
  it: "To ensure both generators yield similar distributions of inputs, we used
  QuickChick's `collect` to determine the number of times each instruction was
  generated during those 10000 tests (**as this was the metric that was used to
  fine-tune the handwritten generators in the first place**)."
- Also: an unbounded `genTree` is rejected because "**the expected size of
  generated trees is infinite**," motivating the size parameter.
- Against SMT-based approaches (Target): "the complexity of the translation
  **leaves little room, if any, for controlling the distribution** of generated
  inputs, unlike in QuickChick-derived generators."

---

## Foner, Zhang, Lampropoulos — *Keep Your Laziness in Check* (ICFP 2018)

The distribution being manipulated here is over the **strictness of generated
functions**:

- QuickCheck's `CoArbitrary` "is capable of generating **only fully strict
  functions**. This **uniform strictness** is fine for testing ordinary
  functional correctness properties, but it's a **deal-breaker for
  StrictCheck**": a buggy `map'` that forces every element is indistinguishable
  from `map` under fully strict function arguments. "If we want to use
  StrictCheck to test higher-order functions, we will need to **generate
  functions with randomly varied strictness**."
- So they write their own `draws`: "We could implement `draws` in many ways, each
  of which would give a **different statistical distribution to the strictnesses
  of our generated functions**. In StrictCheck, `draws` uses a **geometrically
  bounded depth first random traversal, which biases generated functions so that
  demanding different pieces of output tends to evaluate different pieces of
  input**."
- Appendix B makes it precise: "The number of nodes of `Input` consumed by a call
  to `draws` is given by a **geometric distribution with expectation 1**,"
  implemented via `oneof` ("**uniform choice** between" stopping and traversing
  further) at each level.

---

## Giacometti Rocha — *Testing of OCaml Exceptions by Effect-Driven Generation of Programs* (MSc thesis, Edinburgh 2019)

Extends Midtgaard et al.'s Efftester with exception effects. Fewer distribution
claims than the papers above, but the ones it makes are unusually blunt, and the
thesis is a clean case of **a generator modified in ways that change its
distribution for reasons unrelated to distribution**.

- **Rule probabilities, stated as the method.** "First, we choose any of the
  typing rules (Section 3.4) **with different probabilities**." Generation is
  top-down over a type-and-effect goal `(Γ; τ; φ)`, and "**Leaves are always
  pure**."
- **The central modification: exact effect matches instead of sub-effecting.**
  The original Efftester relied on an invariant — "if the sub-expressions had an
  effect smaller than or equal to the expected, the enclosing expression would
  also have an effect smaller than or equal to the expected" — which lets the
  generator terminate by dropping to `pure` leaves. "**But this property no longer
  holds in our system.** Replacing `exception` by `pure`, for example, might make
  the enclosing expression larger than expected because exceptions change the
  computation and the way effects propagate" (worked example:
  `let(pure, mixed) = mixed ⊀ exception`). Consequence: "**So, in our extended
  system, we have to use exact matches at the cost of more failures and a
  performance downgrade – trees are longer on average because it takes more steps
  in average to have `pure` in all leaves.**"
- **How they compensated — and what it does to the distribution.** "A machine
  learning or manual tuning can improve this number by **adjusting the
  probabilities of each rule** to increase the success rate. Another approach is
  to **increase the probability of terminal symbols (constants) as we go down the
  tree**. In our case, **we tuned the maximum height of the trees in a way to get
  a relatively low number of failures, tried to generate more programs than
  necessary and simply ignored the ones that failed.**" (I.e. the shipped fix is a
  height cap plus discarding failures — both of which reshape the distribution —
  rather than reweighting the rules.)
- **Three "concessions" listed in §6.1, each narrowing or reshaping the output
  space.**
  1. Exact effect matches cause "a performance downgrade… because **we have more
     constraints on where we can place terminal symbols in the tree**."
  2. "A similar problem arises in shrinking: the reduced expression must have the
     same effect as the original one, or we might introduce non-determinism…
     **So we disabled many shrinking strategies.**"
  3. "**We use an application rule instead of `Indir` rule** as proposed in
     [Midtgaard et al., Pałka et al.], a generalisation that handles functions with
     multiple parameters. This rule avoids expensive backtracking in the
     generation. **We did not consider it to simplify our induction proofs and our
     implementation.** Many functions given in the environment have a single
     parameter."
- **A distribution-shaped support limitation.** "there is a limitation inherent to
  this technique, and it is valid for other goal-directed generators: **we do not
  create invalid programs, so we do not have negative test cases.** The generated
  programs always type-check and are syntactically correct, so **we might miss bugs
  where the compiler accepts invalid input**."
- **The evaluation attributes a coverage failure directly to rule
  probabilities.** The extended tool found 3 exception-related bugs the original
  missed, but "**the original tool also detected bugs not found by us. In theory,
  the extended version should have found all of them. The reason it did not is the
  lack of variability in the probability of the rules. This again demonstrates the
  importance of selecting the likelihood of each rule.**"
- **The general critique, twice.** §6.1: "a weakness in many generators
  [Midtgaard et al., Pałka et al., Csmith] is the **manual selection of
  probabilities for each rule**. Ideally, we should have **uniform testing of rule
  combinations** or **some iterative process to adjust the probabilities according
  to some attribute of the tree or the rules, such as the capacity of uncovering
  bugs. Otherwise, the process is biased.**" §6.2: "Efftester and many generators
  use a production grammar with **fixed, user-defined probabilities, so the manual
  choice of values can negatively affect the ability to find bugs. If they ever
  change it during the process, it is only to guarantee termination.**" §6.3
  (future work): "Efftester and many other generators would benefit from an
  **automated mechanism to choose probabilities for the rules in the grammar**."
  It cites Claessen–Duregård–Pałka among approaches where probabilities "can be
  generated automatically to **guarantee a distribution of the input**."
- **Bias claims about rival input-generation techniques.** Hand-written compiler
  tests: "The tested scenarios are also **biased by their experience**."
  DeepSmith-style learned models: "The code generated by this method is **biased to
  use popular features and might dismiss new or unconventional ones**." EMI-style
  mutation: "the result **might be biased**… it might produce **less variation**
  because it does not combine the characteristics of multiple programs."
- **The only formal probabilistic claim** is about the *experiment*, not the
  generator: because "the parameters of the generator are constant during the
  execution and all generations are independent, every generated program is a
  **Bernoulli trial**," so bug counts "follow a **binomial distribution**,"
  estimated by MLE with **Clopper–Pearson** confidence intervals, since "the
  standard method of using the normal method as a confidence interval estimator is
  inappropriate when p is too small, as in our case."

---

## Zhang, Roth, Pierce, Roth, Haeberlen — *Testing Differential Privacy with Dual Interpreters* (OOPSLA 2020)

Most "distribution" talk here is about output/probability distributions of DP
mechanisms, but there are real generator-distribution claims:

- **The testing guarantee is stated relative to the generator's input
  distribution.** Random differential privacy (Def. 10, from Hall et al.):
  "**Assume a fixed distribution over similar inputs I**… `f` is (ε, δ, α)-random
  differentially private if, with probability at least 1 − α, **sampling similar
  inputs (x₁, x₂) from I** leads to (ε, δ)-pointwise indistinguishable
  distributions." "Ill-behaved" is likewise defined "assume a fixed distribution
  I of similar inputs," with α bounding "the probability of **draws from I**
  yielding 'bad' similar inputs." So the guarantee (Theorem 12: false-accept
  probability ≤ e^(−d(θ+α))) is only as meaningful as the generator's
  distribution — no uniformity or coverage claim is made about it.
- **The generators themselves are hand-written and per-relation.** "We used the
  QuickCheck randomized testing library to build test input generators… **We
  manually wrote one generator for each type of similarity relation** introduced
  in Section 2." For ReportNoisyMax: "we need to generate inputs whose
  coordinate-wise distance is bounded by 1. **We implement such a generator
  manually** using QuickCheck." A single generator is then **reused** across
  several benchmarks (ReportNoisyMax's is reused for the WithGap variant and its
  buggy variants; a separate manually written generator for L1-distance-bounded
  pairs of lists is reused for PrefixSum/SparseVector variants).
- **Explicit gap between theory and practice, driven by sample counts.** "Due to
  scaling issues, our current testing framework **does not allow us to run tests
  large enough to give meaningful (ε, δ, α)-random differential privacy
  guarantees** through Theorem 12. For example, to achieve guarantees with
  δ = 10⁻⁵ … we need at least 10⁵ samples. Our current evaluation uses between
  500 to 5000 sampled traces per test iteration." Conclusion: "DPCheck's testing
  framework is **more useful for catching differential privacy bugs than for
  validation**."
- **A distribution claim used as the *oracle*** in the DAS case study: "If our
  null hypothesis—that both versions of DAS behave identically—is true, then we
  should observe that the recorded *p*-values **follow a uniform distribution on
  the interval [0, 1]**." A Kolmogorov–Smirnov test on the collected *p*-values
  gives *p* = 0.68, "signaling a lack of evidence to reject the hypothesis that
  the recorded *p*-values are sampled from a uniform distribution."

---

## Frank, Quiring, Lampropoulos — *Generating Well-Typed Terms that are not "Useless"* (POPL 2024)

The paper's core thesis is a distribution bug in top-down generation:

- "decoupling the generation of types and expressions **significantly biases
  generation towards functions that do not use their arguments**. Such 'use-less'
  function arguments lead to wasted computation, both in generation time (the
  code that generates such arguments is essentially dead) and compilation time."
- Their λ▷ small-step relation "allow[s] for **more fine-grained control of the
  resulting distribution**."
- Backwards compatibility with top-down rules is weighted "**by annotations under
  user control**. Although fully replacing function and application generation
  with their λ▷ variants would **restrict the space of generated programs**, this
  is not a problem in practice as these rules only give people crafting
  generators **the flexibility to bias generation towards creating functions that
  use their arguments, a bias that cannot be expressed so straightforwardly in a
  top-down setting**."

### Generator modifications explicitly made to fix the distribution (§3.4)

- Introduced under the heading "**low-level choices that impact the posterior
  distribution of generated terms**."
- **Functions with empty domains.** In the multary calculus, functions can end up
  with no parameters, and "if too many functions turn out this way generated
  programs will primarily be thunking and forcing expressions rather than passing
  data around in complex ways. In practice, however, we found that **weighting
  the insertion of arguments into functions with fewer parameters more heavily**
  was sufficient to ensure that almost all functions have parameters."
- **No direct control over function domains.** Crude measures like blocking
  `GenParam▷` on complex types "fall short"; the generator runs out of fuel and
  produces "many complex function types that just throw out all their values —
  **exactly what we were trying to avoid!** In practice, the same **distribution
  tuning** as before—**deprioritizing insertions into functions with too many
  parameters**—greatly lessens this tendency."
- Rule selection uses **urns** "to efficiently sample from each valid distinct
  generation step."

### Quantified distribution outcome (§4.2)

- Real programs: 94.9% average parameter usage. Pałka et al.'s generator over
  10000 tests: **30% parameter usage**. Theirs: 30% for plain λ parameters,
  **66% for extensible λ parameters**, 100% for let-bindings; 35% across all
  function parameters, 55% across all variables.
- "However, in our generator framework **all of these statistics are highly
  tunable**: different choices for the weights of rules such as `GenApp▷` or
  `GenParam▷` **directly influences the percentage of variables used** in
  generated terms, enabling a wide range of previously unattainable behaviors. In
  our experimenting the usage of extensible lambda parameters has **ranged from
  ∼60% to ∼99%**."

### Skepticism about uniformity (§5)

- "It still, however, remains an **open question whether testing with a uniform
  generation of inputs (and the associated overhead that comes with) is an
  effective approach**. For example, Grygiel and Lescanne observed that 'almost
  all typeable terms start with an abstraction.' **Unfortunately, a generator
  that almost always produces such abstractions is completely ineffective at
  finding bugs in properties that involve reduction**, such as progress,
  preservation, or most specifications of optimization correctness, as no
  reductions can actually take place."
- "At the same time, **ensuring uniformity comes at a significant efficiency
  cost**: Bendkowski et al. can generate terms of a given moderate size at a rate
  of a few lambda terms per second; in contrast, our implementation generates
  many thousands… this kind of difference in efficiency is **almost a
  non-starter** for testing a complex software system such as GHC."
- On Luck: "lightweight annotations to control both **the resulting probability
  distribution** and the amount of constraint solving."

---

## Tjoa, Garg, Goldstein, Millstein, Pierce, Van den Broeck — *Tuning Random Generators* (OOPSLA 2025)

Entirely about generator distributions.

- **Framing.** "**Since testing performance is entirely dependent on the
  distribution of test inputs**, a great deal of PBT research has focused on how
  to quickly generate inputs that find more bugs, faster." "To achieve a good
  distribution over test inputs, users must **tune** their generators… **it is
  very difficult to understand how to choose individual generator weights in
  order to achieve a desired distribution**, so today this process is tedious and
  **limits the distributions that can be practically achieved**."
- The `oneOf` list generator: "**50% of the generated test cases will be empty
  lists!** … not only are half the lists empty, but half of the rest are length
  1, and so on." With `freq [1 ⇒ Nil, 2 ⇒ Cons …]` "the distribution of list
  lengths is roughly the **geometric distribution with success probability
  two-thirds**" (footnote: "It differs from the geometric distribution because it
  is **truncated at the initial sz**").
- "it can be a significant challenge to understand how changing weights changes
  the final distribution. Indeed, recent work on PBT usability cited tuning as a
  source of '**mental strain**' for developers who felt like they needed to
  '**study probability and statistics**.'"
- **Convention throughout.** "an '**untuned**' generator is one in which **each
  random choice is uniformly distributed**"; "we initialize weights to have
  uniform values."
- **Objective functions.** **target** (negative KL divergence to a user-specified
  target distribution, pushed forward through a feature function `g`); **entropy**
  ("a uniform distribution over all possible test cases has the maximum entropy,
  thus maximizing the entropy objective function **takes the generator
  distribution closer to a uniform distribution**"); **specification**;
  **specification entropy**; **feature specification entropy**.
- **The two-objective tension.** "**these two objectives inherently conflict.
  Tuning for diversity incentivizes large terms, which are more likely to be
  diverse but less likely to be valid. Tuning for validity incentivizes trivially
  valid terms such as empty trees or lists.**"
- **Honest partial-success claim** (footnote 4): "The tuned distributions of the
  AST heights in the STLC bespoke generator **do not exactly match the target
  distribution**… but they are closer to their objectives: tuning improves KL
  divergence **from 0.44 to 0.22** for the uniform target and **from 0.92 to
  0.27** for the linear target."
- **STLC type diversity.** "At the beginning of tuning, ill-typed terms and terms
  of type `bool` comprise **66% and 25%** of samples. After tuning, **neither any
  single type nor ill-typed terms comprise more than 0.5%** of samples."

### Most explicit of the eleven about modifying a generator to change its achievable distributions (§5, "Constructing Tunable Generators")

- "the **space of distributions that are possible to achieve by tuning the weights
  of the generator is limited by the generator's structure**. This, in turn,
  limits the extent to which Loaded Dice can optimize for an objective function"
  (footnote 10: "This is the classic problem of **underfitting**"). Figure 9's
  caption: "**Modifications to a generator for RBT trees… to increase the space
  of distributions it can exhibit.**"
- (a) → (b): parameterize weights by the `size` argument — "**this generator now
  uses m + 1 symbolic weights instead of only two**."
- (b) → (c): "the user does not have to be limited by the preexisting structure of
  their generator. They can **add additional function arguments** to their
  generators for more symbolic weights" — passing the parent color down gives
  `2m + 1` tunable weights, and "**correlates random choices in the generator
  that were previously independent**… Depending on the parent color, the
  probability of choosing `Leaf` changes."
- (c) → (d): "**frontloading** random choices, allowing them to be made in
  tandem" — the two children's constructors are chosen jointly and passed down,
  so they can be correlated.
- `derive_generator` automates this by "**introducing new symbolic weights for
  each possible value of the dependencies**," namely current size and a
  **call-stack suffix** of a given lookback length (2 in the evaluation), and
  "also **frontloads choices so that they can be correlated**."
- **Evaluation adaptation.** "We first **adapted** Etna's bespoke STLC generator…
  **We fixed initial sizes and parameterized weights by the size argument** of
  the current recursive call." Then tuned it toward a **deliberately small-term**
  target distribution `{0→40%, 1→30%, 2→20%, 3→10%}` over `App` constructors,
  following Etna's observation that "**larger generations can be empirically
  detrimental for bug-finding (in the face of conventional PBT wisdom)**."
- **Results.** 3.1–7.4× bug-finding speedup for specification entropy; ≥1.9× for
  the STLC target distribution, and "**Figure 15b validates that the distribution
  of applications did change as intended by tuning**." Cause analysis: speedup
  comes from more valid/unique samples (e.g. BST 2,592 → 14,387), not generation
  speed — while "**tuning the STLC Bespoke generator for smaller generations
  decreases the number of valid and unique samples. This highlights the tradeoff…
  the virtue of tuning is that it allows one to choose that tradeoff.**"
- **Expressiveness caveats.** Loaded Dice supports "only **first-order (flat)
  generators that are statically bounded**," so `backtrack` semantics are spelled
  out ("samples from a list of optional values, **resampling without
  replacement** upon sampling `None`") and adaptive sizing must be approximated —
  "Loaded Dice also supports **modeling adaptive initial sizes as a fixed
  distribution**, resulting in a generator that can be tuned as a proxy for the
  adaptively-sized version."

---

## Cross-cutting threads

1. **Uniformity is contested, not assumed good.** Pałka 2011 ("no reason to
   believe a uniform distribution… would be more effective, and it might well be
   less so"), Frank 2024 ("open question whether testing with a uniform
   generation of inputs is an effective approach"; the "almost all typeable terms
   start with an abstraction" argument), and Tjoa 2025 (Etna's finding that
   smaller terms find bugs faster) all push back on Claessen 2015's premise that
   uniform is the right default — which Claessen 2015 itself hedges ("we
   acknowledge the need for other distributions than uniform"). Bendkowski 2017
   sits at the far end of this axis: it calls effective uniform sampling of typable
   λ-terms "a **fundamental open problem in the random software testing approach**"
   and frames Pałka's weights as terms that "**were favoured over other kinds of
   equal size terms**." Rocha 2019 takes the same side ("**Ideally, we should have
   uniform testing of rule combinations**… Otherwise, the process is biased"),
   giving a clean 2017–2024 disagreement with Pałka/Frank about whether uniformity
   is even the goal.

2. **The recurring modification pattern is weight/heuristic surgery to escape a
   small-term skew**, introduced by backtracking failure or by base-case-heavy
   uniform rule choice: Pałka §5.2, Fetscher §3.3 + §4.2, Midtgaard §6.2,
   Lampropoulos POPL'18 (`freq`/`backtrack` weights), Frank §3.4, Tjoa §5. Rocha
   is the negative case: facing exactly this problem (exact effect matches make
   "trees longer on average"), the thesis *names* rule reweighting as the right fix
   but instead caps tree height and discards failures — and then attributes missed
   bugs to "the **lack of variability in the probability of the rules**."

3. **Two papers replaced/restructured the generator itself rather than
   reweighting it.** Fetscher built a *new, non-polymorphic* Redex model with 40
   pre-instantiated constants to get a usable size distribution; Tjoa added
   function arguments, size-indexed weights, and frontloaded choices to enlarge
   the reachable distribution space.

4. **Formal guarantees generally stop short of distributions.** Lampropoulos
   POPL'18 proves soundness/completeness over the *support* only; Midtgaard
   "make[s] no formal claims of uniformity"; Claessen 2015 and Bendkowski 2017 are
   the outliers with genuinely uniform algorithms, and both trade the uniformity
   or its feasibility away — Claessen with a quantified bound (factor b+1),
   Bendkowski by conceding that anticipated rejection keeps uniformity only while
   "the number of **expected retrials tends to infinity** as the target term size
   increases"; Zhang 2020's guarantee is stated relative to whatever distribution
   the hand-written generator induces, and the paper admits its sample counts are
   ~20× too small to instantiate it.

5. **Size is never a neutral parameter.** Three papers make the size *measure*
   itself carry distributional intent: Claessen 2015's `Pay` ("the user may choose
   to assign costs differently, which would change… the distribution of
   size-limited generators"), Bendkowski 2017's arity-based "natural size" plus
   unary de Bruijn indices deliberately chosen because they mimic "the fact that,
   in practical programs, variables located farther from their binders are likely
   to occur less frequently," and Tjoa 2025, where the statically-fixed initial
   size is what makes exact inference (and therefore tuning) possible at all.
   Bendkowski even leaves open whether its central sparsity finding is an artifact:
   "this behaviour **could be dependent on the size definition we are using**."
