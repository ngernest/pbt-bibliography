# Claims about generator distributions in nine PBT papers

A survey of every claim about *distributions* — formal or informal — made in the
following papers, including places where a generator had to be modified in a way
that changes its distribution.

Papers covered (chronological):

| Paper | Venue |
|---|---|
| Pałka, Claessen, Russo, Hughes — *Testing an Optimising Compiler by Generating Random Lambda Terms* | AST 2011 |
| Fetscher, Claessen, Pałka, Hughes, Findler — *Making Random Judgments* | ESOP 2015 |
| Claessen, Duregård, Pałka — *Generating Constrained Random Data with Uniform Distribution* | JFP 2015 |
| Midtgaard, Justesen, Kasting, Nielson, Nielson — *Effect-Driven QuickChecking of Compilers* | ICFP 2017 |
| Lampropoulos, Paraskevopoulou, Pierce — *Generating Good Generators for Inductive Relations* | POPL 2018 |
| Foner, Zhang, Lampropoulos — *Keep Your Laziness in Check* | ICFP 2018 |
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

### Most explicit of the nine about modifying a generator to change its achievable distributions (§5, "Constructing Tunable Generators")

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
   acknowledge the need for other distributions than uniform").

2. **The recurring modification pattern is weight/heuristic surgery to escape a
   small-term skew**, introduced by backtracking failure or by base-case-heavy
   uniform rule choice: Pałka §5.2, Fetscher §3.3 + §4.2, Midtgaard §6.2,
   Lampropoulos POPL'18 (`freq`/`backtrack` weights), Frank §3.4, Tjoa §5.

3. **Two papers replaced/restructured the generator itself rather than
   reweighting it.** Fetscher built a *new, non-polymorphic* Redex model with 40
   pre-instantiated constants to get a usable size distribution; Tjoa added
   function arguments, size-indexed weights, and frontloaded choices to enlarge
   the reachable distribution space.

4. **Formal guarantees generally stop short of distributions.** Lampropoulos
   POPL'18 proves soundness/completeness over the *support* only; Midtgaard
   "make[s] no formal claims of uniformity"; Claessen 2015 is the outlier with a
   proved-uniform algorithm, and even it trades uniformity away for performance
   with a quantified bound (factor b+1); Zhang 2020's guarantee is stated relative
   to whatever distribution the hand-written generator induces, and the paper
   admits its sample counts are ~20× too small to instantiate it.
