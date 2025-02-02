# COM SCI 181 - Spring '21 - Campbell



[TOC]

## Lecture 1: Models of Computation

- Course lies at the intersection of multiple disciplines: 
  - Discrete mathematics
    - Associated with left hemispheric thinking
  - Continuous mathematics
  - Computer science
  - Linguistics
    - Associated with right hemispheric thinking
  - Creativity within constraints
  - Clarity of communication
- Computer/Computation is:
  - Input/Output
    - Interfaces
  - Storage state information
  - Processing/Programmable
  - Deterministic
  - Sequential
    - Bounded action - computers can only process a finite amount of information in a finite amount of time
    - Automatic/mechanical
    - Even applies to most parallel machines
  - What problem the computer is trying to solve
  - Turing complete
  - Given a particular type of computer, what class of problems can that type or model solve?
- Models of computation
  - DFA
    - Corresponds to finite state languages
    - Equivalent to a finite state machine
  - CFG - context-free grammar
    - Corresponds to context-free languages
    - Handles syntax of languages, but not semantics
  - TM - Turing machine
  - There are things that none of these models are capable of doing
  - As we go down the models:
    - There's increasing expressiveness/computational power
    - There's decreasing understandability/predictability
      - There are more and more things that are impossible to predict behaviorally
        - Mathematical, not knowledge-based
- DFA
  - Inputs => finite sequence of symbols chosen from a pre-defined alphabet
  - Output => binary
  - Simplest model for a computer:
    - Black box connected to a lightbulb
      - Lightbulb acts as the binary output
    - Finite input string printed on a tape
      - Read left-to-right, symbol-by-symbol
      - At each symbol, the machine has to declare what its output would be if that were the end of the input
      - On => input so far is accepted
      - Off => input so far is rejected
      - Only input that matters is when the end of the tape is reached
        - On => string was accepted
        - Off => string was rejected
    - Black box must have a program storage/state and sequential/deterministic behavior
      - Directed graph with labels on the edges of the graph
      - Labels correspond to the symbols of the alphabet
        - Alphabet => any non-empty, finite set of objects
      - Finite state machine
        - Contains a special state called the accepting state, which implements the output
        - Binary output => accept/reject => decider
        - Each node has exactly 1 outgoing edge per symbol of the alphabet => Determinism
- Notation
  - Given a string `w` over some alphabet: 
    - Define `w^R` ≡ `w` written in reverse order
      - `(0101)^R` = `1010`
    - Define a "run of symbol `x` in `w`"
      - `w = aabcabbb`
      - `x` ∈ the alphabet `{a, b, c}`
        - `w` contains 2 runs of `a`
        - `w` contains 2 runs of `b`
        - `w` contains 1 run of `c`



## Lecture 2: Deterministic Finite Automata

- Problem refers to an infinite number of instances

- An alphabet is defined as any nonempty, finite set of objects (other than epsilon) called symbols

- A string/word is defined as a finite sequence of symbols from a given alphabet `Σ`

  - We generally refer to this as a string or word over an alphabet

  - The length of a word, `|w|` is the number of symbols in `w`

  - $$
    \Sigma^+\equiv\{\text{all nonempty words over}\  \Sigma\}
    $$

    - Infinite set of finite-length strings
      - The strings are indefinitely long

    - String of length 1 could also be a symbol in the alphabet

- Let `Σ` be an alphabet and `w` be a string over `Σ` of the form `rst`

  - `r`, `s`, and `t` are substrings of `w`
    - Any substring of `w` is called a proper substring of `w` as long as its not the entire word
  - `r` is a prefix of `w`
  - `t` is a suffix of `w`

- Define `#(a, w)` where:

  - $$
    a\in\Sigma\\
    w\in\Sigma^+
    $$

  - To be the number of occurrences of symbol `a` in string `w`

- Define a "run of `a` in `w`" as a substring of `w` such that it consists only of 1 symbol `a` and there are no other `a`'s adjacent to it

- Define `w^R` as a word `w` written in reverse order

- Given an alphabet `Σ`, we define a language over `Σ` to be any set of words over `Σ`

  - So, a language is any subset of `Σ^+`
    - `{}` is a language over any alphabet
    - A language is any member of `P(Σ^+)`
  - Languages can represent computational problems

- `ε` can be a proper substring of any string except `ε`

- Generalize concatenate:

  - $$
    \Sigma^+=\Sigma\cup\Sigma\cdot\Sigma\cup\Sigma\cdot\Sigma\cdot\Sigma\cup\cdot\cdot\cdot
    $$

    - Language concatenation:

      - $$
        S\cdot T\equiv\{s\cdot t|\ s\in S, t\in T\}
        $$

- Formal Definition of Deterministic Finite Automata

  - Deterministic Finite Automaton:
  
  - $$
      M=(Q,\Sigma,\delta,q_0,F)
      $$
  
      - `Q` is a finite set of states
  
      - `Σ` is a finite set representing the alphabet
  
      - $$
        q_0\in Q
        $$
  
      - $$
        F\subseteq Q
        $$
  
      - $$
        \delta: Q\times \Sigma\rightarrow Q
        $$
  
        - `δ(q, a) = q'` is interpreted as: in state `q` on input `a`, go to state `q'`
        - Each move consumes one input symbol `a`
  
  - Initial configuration of `M`: in the initial state. `q_0`, with the input on the input tape and the input tape head at the first symbol of the input
  
  - Define computation of `M` on input `w = w_1w_2 ... w_m`:
  
    - A sequence of legal moves from the initial state, proceeding as long as there are legal moves to make
  
    - Formally:
  
      - There exists a sequence of states of `M`, `q_0, q_1, ... , q_m` in `Q` such that:
  
        - $$
          \text{For all}\ 0\le i\le m-1,\delta(q_i, w_{i+1}) = q_{i+1}
          $$
  
  - The computation is accepting if `q_m` is in `F`, otherwise it's rejecting
  
    - Also, if a DFA does not have any legal move to make at some step before reaching the end of the input string, then the computation "blocks" and rejects
  
  - We can extend the definition of `δ()` over `Q x Σ` to `δ*()` over `Q x Σ*`, recursively/inductively on the length of the input string
  
    - Define:
  
      - $$
        \delta^*:(Q\times\Sigma^*)\rightarrow Q
        $$
  
      - $$
        \delta^*(q,\varepsilon)=q,\ \text{for all}\ q\in Q
        $$
  
      - $$
        \delta^*(a, ay)=\delta^*(\delta(q,a),y),\ \text{for all}\ q\in Q,a\in\Sigma,\ \text{and}\ y\in\Sigma^*
        $$
  
    - This is the transitive closure of `δ` with respect to the second argument, `Σ`
  
  - If a DFA is missing some input option, then blocking occurs and the input is rejected



## Lecture 3: Graph Theory, Closure, and NFA

- Consider the language defined by:

  - $$
    \Sigma=\{a,b,\#\}\\
    L=\{w\#w^R|\ w\in\{a,b\}^*\}
    $$

  - Proof:

    - Suppose `L` were a finite state language, then by definition, there must be a DFA for `L` such that:

      - $$
        M=(Q,\Sigma,\delta,q_0,F)
        $$

    - Take the input `w#w^R`, and assume the DFA has read `w`

    - There are `2^|w|` possible prefixes prior to the delimiter

    - If `2^|w| > |Q|`:

      - The DFA will go from state `q_0` to some state `q`, where the input `w#` has been read
      - From there, it will read `w^R` and end up in a state `q_F ∈ F`
      - Now consider the input `w'#w'^R`, where `w != w'`
        - Since there are a finite number of states and an indefinite number of strings, some string `w'#` must end up in the same state `q` as the prior input by the pigeon hold principle
        - Imagine if the input following arrival at that state were `w^R`
        - Then the machine would have to accept it, as established above, which would mean the DFA was accepting the string `w'#w^R`, which is not a part of the language -><-
        - Implies no such DFA may exist => `L` is not an FSL

- Graph Theory

  - $$
    G=(V,E)\\E\subseteq V\times V
    $$

    

  - Any acyclic graph is a tree

  - Any node of a tree (regardless of how its drawn) can be a root

  - Directed rooted tree: directed graph that is acyclic even when you view it as undirected, and it has a designated root node

    - $$
      G=(V,E,r)\\r\in V
      $$

    - In CS, we say that all edges are directed from the root towards the leaves

    - A directed rooted `k`-ary tree is a directed rooted tree where the outdegree of every node is `<=` to `k`

- Intro to Closure Properties of Families of Languages

  - Recall: a language is any set of strings/words over an alphabet `Σ`

  - A family of languages is any set of languages over a common alphabet `Σ`

    - Usually, they have something in common, hence the name "family"
      - e.g., the FSLs are a family of languages, and the thing they have in common is every one is accepted by some DFA
      - e.g., fact: if `L` is an FSL, then so is `L'` => FSLs are closed under complementation
        - When a family of languages has a property where you take a language and modify it by some operation and the result is guaranteed to still be in the same family of languages,  we say the family is closed under that operation

  - FSLs are closed under complementation

    - Proof:

      - `L` is FSL => there is a DFA that accepts it

      - Assume:

        - $$
          M=(Q,\Sigma,\delta,q_0,F)
          $$

      - Then:

        - $$
          \bar{M}=(Q,\Sigma,\delta,q_0,(Q - F))
          $$

          - Make any accepting state reject and vice versa

- Formal Definition of Nondeterministic Finite Automata

  - Nondeterministic Finite Automaton (NFA):

    - $$
      N=(Q,\Sigma,\delta,q_0,F)
      $$

      - Where:

        - `Q`, `Σ`, `q_0`, and `F` are the same as for a DFA

        - $$
          \Sigma_\varepsilon=\Sigma\cup\{\varepsilon\}
          $$

        - $$
          \delta:Q\times\Sigma_\varepsilon\rightarrow\mathcal{P}(Q)
          $$

          - The result of the `δ` function being a set of possible states in `P(Q)`, is how the definition allows for nondeterminism
            - `δ(q, a)` contains `q'` is imterpreted as: in state `q` on input `a`, `N` can go to state `q'`
            - Each move on a symbol `a` in `Σ` consumes that one input symbol
            - `ε` moves allow `N` to change state without consuming input

    - Initial configuration of `N`: in the initial state `q_0`, with the input on the input tape and the input tape head at the first symbol of the input

    - Define computation of `N` on input `w`:

      - A sequence of legal moves from the initial state, proceeding as long as there are legal moves to make

      - Formally:

        - There exists a sequence `y_1, y_2, ... , y_m`, where each `y_i` is in `Σ_ε`, such that `y_1y_2 ... y_m = w`

        - There exists a sequence of states of `N`, `q_0, q_1, ... , q_m` in `Q` such that:

          - $$
            \text{For all}\ 0\le i\le m-1,\ \text{the set}\ \delta(q_i,y_{i+1})\ \text{contains}\ q_{i+1}
            $$

      - The computation is accepting if:

        - $$
          q_m\in F
          $$

        - Non-accepting computations don't count

        - `N` accepts input `w` if any computation by `N` on `w` accepts

        - If an NFA does not have any legal move to make at some step before reaching the end of the input string, then that nondeterministic computation doesn't accept

        - If an NFA passes through one or more states on `ε` moves after reading the last input symbol, it accepts if any of the states visited after the end of the input are accepting

    - We can extend the domain of the transition function to `δ*()` in exactly the same manner as we did for the DFA

      - Thus, instead of applying the `δ` function one step at a time, we can apply `δ*` to an entire word, `w`
      - The result of the function `δ*(q, w)` is the set of all states the NFA could be in after starting in state `q` and then reading input `w`
        - This automatically takes into account `ε` moves, including the special case at the end of the input



## Lecture 4: NFAs and Regular Expressions

- Main closure properties of the FSLs (or "Regular Languages/Sets"):

  - Union, Concatenation, Kleene* and Kleene+
  - Complementation and Intersection (and therefore, set difference, symmetric set difference, etc. via DeMorgan's Laws)
  - Reversal
  - If a language is not an FSL, these properties generally tell you nothing about the resulting language
    - The exceptions to this are complementation and reversal (complementing a complement and reversing a reversal give you the original language) 

- Pattern Matching

  - $$
    \Sigma=\{a,b\}\\L=\{\text{strings with}\ abab\ \text{as a substring}\}
    $$

  - DFA:

    - Have consecutive states marking each step of the target substring
      - Once you reach the end, mark it as an accept state and loop into that state for the rest of the string
    - Intermediate steps can be filled in based on target behavior

  - NFA:

    - Same consecutive states + tail end as the DFA
    - Use nondeterminism to loop each input to the beginning of the check
      - No rejection in NFAs

- Several equally valid interpretations of nondeterminism

  - Search NFA digraph for path from `q_0` to any `q_i` in `F`
  - Every time you come to a nondeterministic choice, consider both paths in parallel from then on similar to consider tree of all possible computations
  - Guess and check: NFA guesses which choices will lead to accepting states, but it must check its guess before accepting
  - Choosy generator
    - Follow whatever paths you want, but the machine only successfully generates strings that end in an accepting state

- Intro to Regular Expressions

  - All regular expressions (syntax) => all regular sets = FSLs (semantics)
  - Formally, `L(R)` is the set of strings denoted by regular expression `R`, but we usually use them interchangeably
  - `a^n` shorthand for `a` concatenated with itself `n` times
    - `a^n` concatenated with `b^n` is not a regular expression => `n`s have to be equal, not possible to check in a FSL



## Lecture 5: The Pumping Lemma

- Pumping Lemma for FSLs

  - A property of every language in the family of finite state languages which allows us to prove that a given language is not finite state

  - If `L` is a finite state language accepted by a DFA or NFA or represented by a regular expression, then there exists a constant `p` that depends only on `L` such that for all `s ∈ L`, if `|s| ≥ p`, then there exist substrings `x`, `y`, and `z` such that `s` can be broken down into `s = xyz` and `x`, `y`, and `z` obey the following constraints:

    - $$
      x\in\Sigma^*,y\in\Sigma^+,z\in\Sigma^*
      $$

    - $$
      |y|\ge 1
      $$

    - $$
      |x|+|y|\le p
      $$

    - $$
      \text{for all}\ i\ge 0, xy^iz\in L
      $$

-   Context-Free Grammars

  - A representation of languages with roots in linguistics
  - People seeking to express language in mathematical terms invented grammars (aka "rewriting systems")
    - Hoped grammars could represent all human language => didn't fully work out
    - Grammars can represent most features of human language syntax and most features of artificial languages: programming languages, XML, etc.
  - CFG defines the family of context-free languages (CFLs)
    - The family of FSLs is a proper subset of the family of CFLs
  - Formal Grammar:
    - Alphabet `Σ`, variables (aka nonterminals), rewriting rules, start variable

   

## Lecture 6: Context-Free Grammars

- Pumping Lemma

  - Your proof must contain the four essential elements of this kind of proof:
    - Selecting the appropriate string `s` and ensuring that it is in `L`
    - Correctly using the constraints on `x`, `y`, and `z` imposed by the lemma
    - Showing that you have covered all the possible cases of `x`, `y`, and `z`
    - Using sound and complete logic in each case to show that `s'` cannot be in `L`
  - Notes:
    - You can only give a name to `p`, you cannot assume anything about `p` other than `p > 0` and whatever you can infer about `p` from the definition of the language
    - You can and must choose string `s` to be a specific string in `L` 
      - You can choose any string `s` you want, as long as it's in `L` and `|s| > p`
      - Some choices of `s` may not lead to the desired contradiction and some may work but make your job drastically more difficult than necessary
    - You cannot assume anything about how `s` is divided into `xyz`, other than the constraints imposed by the lemma
      - You cannot say "Let `x` = ... " where the substring at the ... is of your choosing
      - Rather, you have to consider every possible way that `s` could be divided into `xyz`
      - The way `s` is divided into `xyz` does not have to line up in any way with the format of the strings in the language
    - You can choose `x'` and `i`

- CFGs

  - A CFG, `G = (V, Σ, R, S)` where:

    - `V` is a finite, nonempty set of variables aka nonterminals

    - `Σ` is an alphabet aka terminals

      - `V` doesn't contain any elements of `Σ`

    - `S ∈ V`

    - `R` is a finite, nonempty set of rewriting rules of the form:

      - $$
        A\rightarrow\alpha\\A\in V\\\alpha\in(\Sigma\cup V)^*
        $$

  - "Context-free" because we can apply any rule for a given variable anywhere it appears

  - Each time we're about to replace a variable, underline it

  - For any string `s ∈ L` for a grammar `G`:

    - There will be at least one parse tree
    - There will be exactly one leftmost derivation that corresponds only to that tree
    - There will be exactly one rightmost derivation that corresponds only to that tree
    - If there are other parse trees, each will have its own pair of one leftmost and one rightmost corresponding derivations
    - There can be other derivations which are neither leftmost nor rightmost for a given parse tree

  - If there is `> 1` parse tree for `s ∈ L` in `G`, we say "`s` is ambiguous in `G`"

  - If `G` has any ambiguous string, then we say "`G` is an ambiguous grammar" or "`G` is ambiguous"

  - If every grammar for a language `L` must be ambiguous, then we say "`L` is an inherently ambiguous language"

  - Ambiguity

    - Good:
      - Human language is ambiguous, so any representation of language should represent ambiguity
    - Bad:
      - If the programming language is ambiguous, then what should the compiler do?



## Lecture 7: CFG Design and GNFAs

- Design Patterns for CFGs
  - Sequencing
    - `S -> ABC` - all patterns of `ABC`
    - `S -> AB | CD` - all patterns of `AB` or all patterns of `CD`
  - Iteration
    - `B -> AB | T` or `A -> AB | T` or `S -> SS | T`
      - Right associative or left associative or freely associative
  - Recursion/Nesting
    - `B -> ABC | T`
      - If `C = ε` => tail-recursion
- Outline of proof that the families of languages represented by DFA, NFA, and regular expression models are equal
  - These 3 models are equivalent
  - Two sets =: `A ⊆ B & B ⊆ A => A = B`
    - Only need three proofs: `DFA ⊆ NFA ⊆ Reg. Exp ⊆ DFA`
    - DFA => NFA => GNFA => Reg. Exp.
    - Reg. Exp. => NFA => DFA
  - DFA => NFA is trivial, DFA is just an NFA without nondeterministic moves
  - NFA => GNFA is trivial
  - GNFA => Reg. Exp.
  - Reg. Exp. => NFA is construction by composition
  - NFA => DFA is direct construction
- GNFA
  - No outgoing edges from the accept state
  - Move across the machine using pattern matching => defined with regular expressions
  - Outgoing edge from every node to every other node, except for `q_start` and `q_accept`
    - `q_start` has a transition to every other node
    - `q_accept` has a transition from every other node
  - Brief Introduction to Generalized NFAs
    - Let `R` denote the set of all regular expressions
    - `N = (Q, Σ, δ, q_0, q_F)` where:
      - `Q`, `Σ`, `q_0` are exactly as for DFAs and NFAs
      - `q_F ∈ Q` (similar to DFAs and NFAs)
      - `δ: ((Q - {q_F}) x (Q - {q_0})) -> R`
        - Recall: in digraph `(u, v, label)` = `δ(q_u, label)` = `q_v` in a DFA
        - Equivalently, `δ(q_u, q_v) = label` in a GNFA
          - This is merely so that `δ()` will be fully defined, and to make the notation in a proof that we're going to do a little simpler
    - `N` accepts `w ∈ Σ*`: there exists a path from `q_0` to `q_F` such that the input matches the series of regular expressions along the path
    - Formally:
      - If there exists strings `w_1, w_2, ... , w_k ∈ Σ*`, such that `w = w_1 ... w_k`, and there exists a sequence of states `q_0, ... , q_k` such that:
        - `q_k = q_f` is the accepting state
        - For each `1 <= i <= k`, `w_i ∈ L(δ(q_i-1, q_i))`
          - i.e., `w_i ∈ the regular set denoted by the regular expression L(δ(q_i-1, q_i))`
          - Or equivalently, `w_i` matches the regular expression on the edge from `q_i-1` to `q_i`
- If `L_1` and `L_2` are FSLs, then so is `L_1 ∩ L_2`



## Lecture 8: Closure of CFLs

- Known CFLs:

  - $$
    \{a^nb^m|\ m=n\ \text{or}\ m>n\ \text{or}\ m\le n\}\\\{a^nbc^n\}\\\{w|\ w\in\{a,b\}^*\ \text{where}\ \#(a,w)=\#(b,w)\}\\\{ww^R|\ w\in\Sigma^*\}\\\{w\in\Sigma^*|\ w=w^R\}\\\{a^ib^jc^k|\ i=j\ \text{or}\ i=k\}\\\{a^ib^jc^k|\ i=j\ \text{or}\ i\ne k\}\\\{a^nb^{2n}\}\\\{a^nb^nc^md^m\}\\\{a^nb^mc^md^m\}
    $$

- Known Non-CFLs:

  - $$
    \{a^nb^nc^n\}\\\{a^ib^jc^kd^l|\ i=k\ \text{and}\ j=l\}\\\{a^nb^nc^md^m|\ n>m\}\\\{a^n|\ n\ \text{is a prime #}\}\\\{a^nb^m|\ m=n^2\}\\\{a^ib^jc^k|\ i<j<k\}\\\{ww|\ w\in\Sigma^*\}
    $$

- Closure Properties

  - FSLs:
    - Concatenation
    - Kleene* and Kleene+
    - Union and Intersection
    - Complementation
    - Reversal
  - CFLs:
    - Concatenation
    - Kleene* and Kleene+
    - Union
    - Reversal

- The family of CFLs can also be represented by a machine-like model: Pushdown Store Automaton (PDA)

  - LIFO stack
  - PDA can only see top of stack to see symbols underneath, it must pop the top of stack
  - Both DFAs and PDAs read the input tape from left to right exactly once
  - Represented as a semi-infinite tape
  - Initial stack = `ε`
  - Can consider the input symbol and the top of the stack together
  - Represent contents of stack during computation as indefinitely long string



## Lecture 9: Pushdown Automata



## Lecture 10: CFL Pumping Lemma

- PDA State Transition Diagrams:
  - `q -> (w_i, a -> b) q'`
    - `w_i` is in the input alphabet, and is either the input symbol or `ε`, which means ignore the input
    - `a` and `b` are in the stack alphabet or are `ε`
    - Any combination of `w_i`, `a`, and `b` can be `ε`
    - `w_i` and/or `a` = `ε` means "don't care"
      - `b = ε` is more complicated
  - Allowable actions of the PDA model
    - Input: read or ignore
    - Stack: push, pop (depend on top of stack), peek, swap
      - `ε -> <input symbol>` represents a push
      - `<input symbol> -> ε` represents a pop
      - `ε -> ε` represents a move where the symbol at the top of the stack doesn't matter
      - `<input symbol> -> <input symbol>` represents a swap or a peek
  - Can't explicitly test for empty stack
  - Nondeterministic: if any computation foes into or passes through an accepting state after reading last symbol of the input, we accept; otherwise, we don't
- Pumping Lemma for CFLs
  - If `L` is a context-free language over `Σ`, then there exists a number, `p`, depending only on `L` such that:
    - For all strings `s` in `L`, if `|s| >= p`, then:
      - There exist five substrings of `s`: `u`, `v`, `x`, `y`, and `z` in `Σ*` such that:
        - `s = uvxyz`
        - For all `i >= 0`, `uv^ixy^iz` is in `L`
        - `|vy > 0|`
        - `vxy| <= p`
    - Note that the substring `vxy` could be anywhere in `s`, i.e., `u` and/or `z` could be `ε`



## Lecture 11: Deterministic PDAs

- The intention of the definition of a DPDA is to ensure that at each stage in the computation, there is only one way to proceed

- You can test if a PDA is deterministic by going through each state and testing if there are nondeterministic moves

- FSLs are a subset of DCFLs which are a subset of CFLs, which are a subset of Non-CFLs

  - Inherently ambiguous CFLs are a subset of CFLs, outside of the subset of DCFLs

- Closure properties

  - FSLs are closed under union, concatenation, Kleene operations, intersection, complementation, and reversal

  - CFLs are closed under union, concatenation, Kleene operations, and reversal, but not under intersection or complementation

  - DCFLs are closed under complementation, but are not under union, concatenation, Kleene operations, intersection, or reversal

    - Reversal:

      - $$
        \Sigma=\{a,b,?,.\}
        $$

      - $$
        L=\{a^ib^j?\ |\ j=i\}\cup\{a^ib^j.\ |\ j=2i\}
        $$

        - `L` is not a DCFL, but `L^R` is



## Lecture 12: PDA Notation

- Shorthand for PDA and DPDA state transition diagrams

  - Consider PDA pseudocode:

    - ```
      If input = a and top = don't care, push x
      If input = a and top = x, push y on top of x
      	Two steps: a, x -> x and ε, ε -> y
      	Shorthand: a, x -> yx
      ```

      - View of stack as a string, where the top of the stack is on the left
      - Replace stack symbol with 0+ moves



## Lecture 13: Turing Machines

- Turing Machine (TM) vs. other models
  - Recognize recursive languages/recursively enumerable languages
    - The difference between these relies on the definition of an accepted computation
    - Recursively enumerable languages are anything that can be represented by a TM
    - Recursive languages are a subset of recursively enumerable languages
- TM computations: Accept, Reject, other...
  - Single tape TM
    - Can reach accept state, reject state, or loop infinitely
    - Work tape that is indefinitely long
    - Input alphabet and a work tape alphabet that contains both the input alphabet and the blank symbol
  - Multi-tape TM
    - Only one tape is special: the input tape
    - Other tapes are controlled by a finite state diagram
    - Exactly equivalent to single tape TMs
  - Accepting TM computations:
    - On input `w`, if TM, `M`, reaches `q_accept`, it halts and accepts input `w` (even if `M` doesn't read all of `w`)
    - On input `w`, if `M` reaches `q_reject`, it halts and rejects input `w`
    - If `M` never reaches either state, the `M` doesn't accept `w`
    - TMs which halt (and accept or reject) for all inputs are called algorithms
      - The family of languages represented by TMs which halt for all inputs is called "recursive"
- Algorithms and the Family of Recursive Languages
- Encodings of Problems as Strings/languages
- Church-Turing Thesis



## Lecture 14: Recursively Enumerable Languages

- Every Turing Machine is a procedure, algorithms are subsets of procedures
  - Algorithms are always halting
  - Procedures may reject by looping infinitely
- TM Models
  - TM Algorithm
    - Always halts and accepts or rejects
    - Recursive/Decidable/Turing-decidable
  - TM Procedure
    - For a string accepted, it halts and accepts; for others, it halts and rejects or fails to halt
    - Recursively enumerable

- Church-Turing thesis - all formal models of computation equivalent to the TM model correspond to our mental notion of a computer => represent everything computable



## Lecture 15: TM Analysis

- Turing machines decide a language if its an algorithm
- Turing machines recognize a language if its a procedure
  - Procedures are the superset and algorithms are a proper subset
- There are languages that cannot be represented with any model people have created
- Static analysis:
  - Does a given DFA accept `{}`?
  - Does a given DFA accept an infinite set?
  - Does a given DFA accept a given string?
- Dynamic analysis/interpreter:
  - TM could interpret a DFA diagram by the Church-Turing Thesis
  - TM could interpret an NFA diagram (search interpretation)
  - Note: this is different from saying a DFA is a special case of a TM: compile-time vs. runtime
  - Same for PDA, using its work tape to simulate the stack => definitely dynamic
- What about semantics of TM?
  - Can a TM simulate a TM? Or is that something we can disprove using proof by contradiction
    - Yes, we can predict what `(M, w)` will do, but in the most general case, we can only do this by simulating the computation step-by-step



## Lecture 16: TM Theory

- We can try to determine what a given TM would do on a given input, however, in the most general case, the best we can do is simulate its behavior with the UTM and watch what happens
- CS Theory preceded CS practice
  - Math preceded CST, which preceded computers
  - Math = closed forms, infinite series and sequences, recursions: set of formulas that is finite = fixed set of formulas that solve an infinite set of problems
  - Used to say "computable" = recursion = "recursive"
    - Then Turing and others showed that there were problems that were "computable" in some sense, but not "recursive", such as the language HP => leads to naming of "recursively enumerable"
  - Turing Recognizer vs. Turing Decider vs. Turing Enumerator
- Let `L_R` be a recursive (decidable) language over `{0, 1}`, let `M` be a decider for `L_R` (aka algorithm = always-halting Turing machine), we can create an enumerator
  - We can create a TM that generates `{0, 1}*` by writing words `0`, `1`, `00`, etc. onto a work tape sequentially
  - After each word, we parse that program and let the decider read the current word
  - Eventually it will return Accept or Reject and halt
  - If it accepts, we copy the word to the special output tape of the enumerator
  - Then, we let the generator for `{0, 1}*` produce the next word and repeat the process indefinitely
- Suppose `L_1` and `L_2` are recursive, prove by construction that `L_1 ∩ L_2` is recursive
  - Let `M_1` be a TM Decider for `L_1` => same for `L_2`
  - Construct `M` for `L_1 ∩ L_2`:
    - Use UTM to simulate `M_1` on a given input `w`
    - Because `M_1` is an algorithm, it will eventually halt and accept or reject
    - If `M_1` halts and rejects, `M` halts and rejects
    - Else, use UTM to simulate `M_2` on `w`
      - If `M_2` halts and rejects, `M` halts and rejects
      - Else, `M` halts and accepts



## Lecture 17: CFG Reductions

- Reductions in CFGs: given `Σ`, CFG `G`
  - Let the language represented by `G` be `L = L(G)`
  - If there exists `xhy` and `xTy` and `G` contains a rule `T -> h`, then we say "`xhy` reduces to `xTy`" or we write "`xhy >-> xTy`" in one step
  - For any two sentential forms `α` and `β`:
    - `α >-> β` (one reduction step)
    - `α >->* β` (zero or more reduction steps)
    - The string replaced is called the "reducing string"
- Reductions are "harder"
  - Recall in derivations, we have two choices: which variable to replace and which rule to apply
  - In reductions:
    - Which string `h` to replace with a variable
    - Which rule to apply (backwards)
    - The possible reducing strings in a step can overlap
  - Leftmost reduction:
    - Each reducing string is replaced only after all other reducing strings *entirely* to the left in that reduction have been replaced
- Not all infinities are equal
  - Any set which can be put in a 1:1 correspondence with the integers is called "countably infinite"
  - No set can be put in a 1:1 correspondence with its own power set



## Reading 1: Discrete Concepts

- Mathematical Notions and Terminology

  - Sets

    - A **set** is a group of objects represented as a unit

    - Sets can contain numbers, symbols, and other sets

    - Objects in a set are called **elements** or **members**

    - Can be described as:

      - $$
        S = \{7, 21, 5\}
        $$

    - We say that a set `A` is a **subset** of a set `B` if every member of `A` is also a member of `B`

      - `A` is a **proper subset** of `B` if `A` is a subset of `B` and `B` is not a subset of `A`

    - Order and repetition of elements don't matter

      - If the number of occurrences of members matter, the group is instead called a **multiset**
      - An **infinite set** consists of infinitely many elements
      - The **empty set** contains 0 members
      - A set containing 1 member is called the **singleton set**
      - A set with 2 members is called an **unordered pair**

    - Sets can be described with a rule rather than with discrete elements

    - The **union** of 2 sets is the set obtained by combining the elements of both sets

    - The **intersection** of 2 sets is the set of elements that are in both sets

    - The **complement** of a set is the set of all elements under consideration that are not in the set

  - Sequences and Tuples

    - A **sequence** of objects is a list of objects in some order

      - Can be described as:

        - $$
          \\(7, 21, 57)
          $$

      - Order and repetition matter in a sequence

      - Finite sequences are called **tuples**

        - A sequence with `k` elements is called a **`k`-tuple**
        - A 2-tuple is also called a **ordered pair**

      - Sets and sequences may appear as elements of other sets and sequences

        - The **power set** of `A` is the set of all subsets of `A`
        - The **Cartesian product** or **cross product** of 2 sets `A` and `B` is the set of all ordered pairs wherein the first element is a member of `A` and the second is a member of `B`
          - The Cartesian product of `k` sets is the set consisting of all `k`-tuples following the same guidelines

  - Functions and Relations

    - A **function** is an object that sets up an I/O relationship

      - The same input always produces the same output

      - Also called a **mapping**, where if `f(a) = b`, then `f` maps `a` to `b`

      - The set of possible inputs to a function is called its **domain**

      - The outputs of a function come from a set called the **range**

        - $$
          f: D\rightarrow R
          $$

          - `f` is a function with domain `D` and range `R`

        - A function may not use all of the elements of the specified range

        - A function that does use all the elements of the range is said to be **onto** the range

      - When the domain of a function `f` is `A_1 x ... x A_k` for some sets `A_1, ... , A_k`, the input to `f` is a `k`-tuple `(a_1, a_2, ... , a_k)` and `a_i` is an **argument** to `f`

        - A function with `k` arguments is called a **`k`-ary function**, where `k` is the **arity** of function
          - If `k` is 1, then `f` is a **unary function**
          - If `k` is 2, then `f` is a **binary function**
            - Common binary functions are written using **infix notation** rather than **prefix notation**

      - A **predicate** or **property** is a function whose range is `{TRUE, FALSE}`

        - A property whose domain is a set of `k`-tuples `A x ... x A` is called a **relation**, **`k`-ary relation**, or a  **`k`-ary relation on `A`**
          - Common case is a 2-ary relation, or **binary relation**
            - Usually use infix notation

    - Special type of binary relation that captures the notion of 2 objects being equal in some feature is called an **equivalence relation**

      - `R` is **reflexive** if for every `x`, `xRx`
      - `R` is **symmetric** if for every `x` and `y`, `xRy` implies `yRx`
      - `R` is **transitive** if for every `x`, `y`, and `z`, `xRy` and `yRz` implies `xRz`

  - Graphs

    - An **undirected graph**, or simply a **graph** is a set of points with lines connecting some of the points
      - The points are called **nodes** or **vertices**
      - The lines are called **edges**
        - The number of edges at a particular node is the **degree** of that node
        - No more than 1 edge is allowed between any 2 nodes
        - Edges can go from a node to itself, called a **self-loop**
      - If `V` is the set of nodes of a graph `G` and `E` is the set of edges, we say `G = (V, E)`
    - Graphs are used to represent data
      - For convenience, we label the nodes/edges of the graph, which makes the graph a **labeled graph**
    - `G` is a **subgraph** of `H` if the nodes of `G` are a subset of the nodes of `H` and the edges of `G` are the edges of `H` on the corresponding nodes
    - A **path** in a graph is a sequence of nodes connected by edges
      - A **simple path** is a path that doesn't repeat any nodes
      - A graph is **connected** if every 2 nodes have a path between them
      - A path is a **cycle** if it starts and ends in the same node
      - A **simple cycle** is one that contains at least 3 nodes and repeats only the first and last nodes
      - A graph is a **tree** if it's connected and has no simple cycles
        - Tree may contain a specially designated node called the **root**
        - Nodes of degree 1 in a tree, other than the root are called **leaves**
    - A **directed graph** has arrows instead of lines
      - Number of arrows pointing from a particular node is the **outdegree** of that node
      - The number of arrows pointing to a particular node is the **indegree**
      - A path in which all the arrows point in the same direction as its steps is called a **directed path**
        - A directed graph is **strongly connected** if a directed path connects every 2 nodes
      - Useful for depicting binary relations

  - Strings and Languages

    - An **alphabet** is any nonempty finite set
      - The members of the alphabet are **symbols** of the alphabet
    - A **string over an alphabet** is a finite sequence of symbols from that alphabet, usually written next to one another and not separated by commas
    - The **length** of a string `w` is the number of symbols `w` contains, written `|w|`
      - The string of length 0 is called the **empty string**, written `ε`
    - The **reverse** of `w`, written `w^R` is the string obtained by writing `w` in the opposite order
    - String `z` is a **substring** of `w` if `z` appears consecutively within `w`
    - Given string `x` of length `m` and string `y` of length `n`, the concatenation of `x` and `y`, `xy`, is the string obtained by appending `y` to the end of `x`
    - The **lexicographic order** of strings is the same as the familiar dictionary order
      - We occasionally use a modified lexicographic order called **shortlex order** or **string order** where shorter strings precede longer strings
    - String `x` is a **prefix** of string `y` if a string `z` exists where `xz = y`
      - `x` is a **proper prefix** of `y` if in addition `x != y`
    - A **language** is a set of strings
      - A language is **prefix-free** if no member is a proper prefix of another member

  - Boolean Logic

    - **Boolean logic** is a mathematical system built around the two values `TRUE` and `FALSE`
      - The values `TRUE` and `FALSE` are called the **Boolean values** and are often represented by the values 1 and 0
      - We can manipulate Boolean values with the **Boolean operations**
        - The simplest Boolean operation is the **negation** or **`NOT`** operation, designated with the symbol `¬`, which converts a Boolean value to the opposite value
        - We designate the **conjunction** or **`AND`** operation with the symbol `∧`
          - The conjunction of two Boolean values is `1` if both of those values are `1`
        - The **disjunction** or **`OR`** operation is designated with the symbol `∨`
          - The disjunction of 2 Boolean values is `1` if either of those values is `1`
        - The operations are used on **operands** 
        - The **exclusive or**, or **`XOR`**, operation is designated by the `⊕` symbol and is `1` if either but not both of its 2 operands is `1`
        - The **equality** operation, written with the symbol `↔`, is `1` if both of its operands have the same value
        - Finally, the **implication** operation is designated by the symbol `→` and is `0` if its first operand is `1` and its second operand is `0`; otherwise, `→` is `1`
      - The **distributive law** for `AND` and `OR`:
        - `P ∧ (Q ∨ R)` equals `(P ∧ Q) ∨ (P ∧ R)`
        - `P ∨ (Q ∧ R)` equals `(P ∨ Q) ∧ (P ∨ R)`

- Definitions, Theorems, and Proofs

  - **Definitions** describe the objects and notions that we use
    - Precision is essential to any mathematical definition
  - After we have defined various objects and notions, we usually make **mathematical statements** about them
    - Typically, a statement expresses that some object has a certain property
    - No ambiguity about its meaning is allowed
  - A **proof** is a convincing logical argument that a statement is true
    - A mathematician demands proof beyond *any* doubt
  - A **theorem** is a mathematical statement proved true
    - A **lemma** is a statement that is only interesting because it assist in the proof of another, more interesting statement
    - A **corollary** of a theorem is a statement that can be proved true due to its relation to the theorem or its proof
  - Finding Proofs
    - One frequently occurring type of multipart statement has the form “`P` if and only if `Q`”
      - If `P` is true, then `Q` is true, written `P ⇒ Q` => the **forward direction** of the original statement
      - If `Q` is true, then `P` is true, written `P ⇐ Q` => the **reverse direction** of the original statement
    - An object that fails to have the property you're trying to prove is called a **counterexample**
    - A well-written proof is a sequence of statements, wherein each one follows by simple reasoning from previous statements in the sequence

- Types of Proof

  - Proof by Construction
    - **Proof by construction** is a proof technique where you demonstrate how to construct the object that you are trying to prove exists
  - Proof by Contradiction
    - **Proof by contradiction** is a proof technique where you begin by assuming the theorem is false and then showing that this assumption leads to a false consequence called a contradiction
  - Proof by Induction
    - **Proof by induction** is a method used to show that all elements in an infinite set have a specified property
    - Each proof by induction has 2 parts:
      - The **basis** proves that `P(1)` is true
      - The **induction step** proves that for each `i ≥ 1`, if `P(i)` is true, then so is `P(i + 1)`
        - In the induction step, the assumption that `P(i)` is true is called the **induction hypothesis**



## Reading 2: Finite Automata

- Finite automata are good models for computers with an extremely limited amount of memory

  - Automatic door example, elevators, household appliances, etc.

- Finite automata and their probabilistic counterpart **Markov chains** are useful tools when we are attempting to recognize patterns in data

  - Used in speech processing and in optical character recognition
  - Markov chains have been used to model and predict price changes in financial markets

- A **state diagram** is a visualization of the interaction between inputs, outputs, and states of a finite automaton

  - The **start state** is indicated by the arrow pointing at it from nowhere, and is the state where processing starts at
  - The **accept state** is the one with a double circle
  - The arrows going from one state to another are called **transitions**
  - The output is either **`accept`** or **`reject`**
    - Output is `accept` when the automaton is in an accept state and `reject` otherwise

- Formal Definition of a Finite Automaton

  - A finite automaton has several parts:

    - It has a set of states and rules for going from one state to another, depending on the input symbol
    - It has an input alphabet that indicates the allowed input symbols
    - It has a start state and a set of accept states

  - The formal definition says that a finite automaton is a list of those five objects: set of states, input alphabet, rules for moving, start state, and accept states

  - We use something called a **transition function**, frequently denoted `δ`, to define the rules for moving

    - Ex) the finite automaton has an arrow from a state `x` to a state `y` labeled with the input symbol `1`

      - $$
        \delta (x, 1)=y
        $$

  - Formal definition:

    - A **finite automaton** is a 5-tuple `(Q, Σ, δ, q_0, F)`, where:

      - `Q` is a finite set called the **states**
      - `Σ` is a finite set called the **alphabet**
      - `δ: Q × Σ → Q` is the **transition function**
        - Specifies exactly 1 transition arrow exits every state for each possible input symbol
      - `q_0 ∈ Q` is the **start state**
      - `F ⊆ Q` is the **set of accept states** or **final states**
        - The null set is valid for `F` => there could be no accept states

    - If `A` is the set of all strings that machine `M` accepts, we say that `A` is the **language of machine `M`** and write:

      - $$
        L(M) = A
        $$

      - We say that **`M` recognizes `A`** or that **`M` accepts `A`**

      - A machine may accept several strings, but it always recognizes only one language

- Formal Definition of Computation

  - Let `M = (Q,Σ,δ,q_0,F)` be a finite automaton and let `w = w_1w_2 ··· w_n` be a string where each `w_i` is a member of the alphabet `Σ`

    - `M` **accepts** `w` if a sequence of states `r_0,r_1, ... ,r_n` in `Q` exists with 3 conditions:

      - $$
        r_0=q_0
        $$

        - The machine starts in the start state

      - $$
        \delta(r_i, w_{i+1}) = r_{i+1},\ \text{for}\  i=0,...,n-1
        $$

        - The machine goes from state to state according to the transition function

      - $$
        r_n\in F
        $$

        - The machine accepts its input if it ends up in an accept state

    - `M` **recognizes language** `A` if:

      - $$
        A=\{w|\ M\ \text{accepts}\ w\}
        $$

      - A language is called a **regular language** if some finite automaton recognizes it

- Designing Finite Automata

  - "Reader as automata" method

- The Regular Operations

  - We define three operations on languages, called the **regular operations**, and use them to study properties of the regular languages:

    - **Union**:

      - $$
        A\cup B=\{x|\ x\in A\ \text{or}\ x\in B \}
        $$

        - Takes all the strings in both `A`and `B` and lumps them together in 1 language

    - **Concatenation**:

      - $$
        A \circ B =\{xy|\ x\in A\ \text{and}\ y\in B\}
        $$

        - Attaches a string from `A` in front of a string from `B` in all possible ways to get the strings in the new language

    - **Star**:

      - 
        $$
        A^*=\{x_1x_2...x_k|\ k\ge 0\ \text{and each}\ x_i\in A\}
        $$

        - Works by attaching any number of strings in `A` together to get a string in the new language
        - **Unary operation** rather than **binary operation**

  - Generally speaking, a collection of objects is **closed** under some operation if applying that operation to members of the collection returns an object still in the collection

    - The class of regular languages is closed under the union operation



## Reading 3: Nondeterminism

- When a DFA machine is in a given state and reads the next input symbol, we know what the next state will be - **deterministic** computation

  - In a **nondeterministic** machine, several choices may exist for the next state at any point
  - Nondeterminism is a generalization of determinism => every deterministic finite automaton is automatically a nondeterministic finite automaton

- Differences:

  - Every state of a DFA has exactly 1 exiting transition arrow for each symbol in the alphabet
    - In an NFA, a state may have 0, 1, or many exiting arrows for each alphabet symbol
  - In a DFA, labels on the transition arrows are symbols from the alphabet
    - An NFA may have 0, 1, or many arrows from each state labeled with `ε`

- How does an NFA compute?

  - After reading a symbol, the NFA splits into multiple copies of itself and follows all possibilities in parallel
    - Each copy takes a single way to proceed and continues as before
    - For subsequent choices, the machine will split again
    - If the next input symbol doesn't appear on any of the arrows exiting the current state, that copy of the machine dies
  - If any one of the copies of the machine is in an accept state at the end of the input, the input string is accepted
  - If a state with a `ε` symbol on an exiting arrow is encountered, the machine will split and follow each of these arrows, while leaving a copy of the machine at the current state
  - Operates like a tree of possibilities, where the root of the tree is the start of the computation

- Every NFA can be converted into an equivalent DFA

- Formal Definition of a Nondeterministic Finite Automaton

  - In a DFA, the transition function takes a state and an input symbol and produces the next state

  - In an NFA, the transition function takes a state and an input symbol or the empty string and produces the set of possible next states

  - A **nondeterministic finite automaton** is a 5 tuple `(Q, Σ, δ, q_0, F)` where:

    - `Q` is a finite set of states

    - `Σ` is a finite alphabet

    - The transition function is:

      - $$
        \delta : Q\times \Sigma_\varepsilon\rightarrow\mathcal{P}(Q)
        $$

    - The start state is:

      - $$
        q_0\in Q
        $$

    - The set of accept states is:

      - $$
        F\subseteq Q
        $$

- Equivalence of NFAs and DFAs

  - Deterministic and nondeterministic finite automata recognize the same class of languages

  - 2 machines are **equivalent** if they recognize the same language

  - > Every nondeterministic finite automaton has an equivalent deterministic finite automaton
  
- Proof:
  
  - Let `N = (Q, Σ, δ, q_0, F)` be the NFA recognizing some language `A`, we construct a DFA `M = (Q', Σ', δ, q_0', F')` recognizing `A`
    
  - $$
        Q'=\mathcal{P}(Q)
    $$
    
    - Every state of `M` is a set of states of `N` => `P(Q)` is the set of subsets of Q
    
  - For `R ∈ Q'` and `a ∈ Σ`, let:
    
    - $$
          \delta '(R, a) = \{q\in Q|\ q\in \delta(r, a) \ \text{for some}\ r\in R\}
      $$
    
      - If `R` is a state of `M`, it is also a set of states of `N`
    
      - When `M` reads a symbol `a` in state `R`, it shows where `a` takes each state in `R`
    
      - $$
            \delta '(R,a) =\bigcup_{r\in R}\delta(r,a)
        $$
    
  - $$
        q_0' =\{q_0\}
    $$
    
    - `M` starts in the state corresponding to the collection containing just the start state of `N`
    
  - $$
        F' =\{R\in Q'|\ R\ \text{contains an accept state of}\ N\}
    $$
    
    - The machine `M` accepts if one of the possible states that `N` could be in at this point is an accept state
    
  - For any state `R` of `M`, we define `E(R)` to be the collection of states that can be reached from members of `R` by going only along `ε` arrows, including the members of `R` themselves:
    
    - $$
          E(R)=\{q|\ q\ \text{can be reached from}\ R\ \text{by traveling along 0 or more}\ \varepsilon\ \text{arrows}\}
      $$
    
  - $$
        \delta'(R, a)=\{q\in Q|\ q\in E(\delta(r,a))\ \text{for some}\ r\in R\}
    $$
    
    - Modify the transition function of `M` to place additional "fingers" on all states that can be reached by going along `ε` arrows after every step
    
  - $$
        q'_0=E(\{q_0\})
    $$
    
    - Modify the start state of `M` to move the fingers initially to all possible states that can be reached from the start state of `N` along the `ε` arrows
  
- > A language is regular if and only if some nondeterministic finite automaton recognizes it
  
- Closure Under the Regular Operations

  - > The class of regular languages is closed under the union operation

    - Proof:

      - Let `N_1 = (Q_1, Σ, δ_1, q_1, F_1)` recognize `A_1` and `N_2 = (Q_2, Σ, δ_2, q_2, F_2)` recognize `A_2`, construct `N = (Q, Σ, δ, q, F)` to recognize `A_1 ∪ A_2`

      - $$
        Q=\{q_0\}\cup Q_1\cup Q_2
        $$

        - The states of `N` are all the states of `N_1` and `N_2`, with the addition of a new start state `q_0`

      - The state `q_0` is the start state of `N`

      - The set of accept states:

        - $$
          F=F_1\cup F_2
          $$

          - The accept states of `N` are all the accept states of `N_1` and `N_2` => `N` accepts if either `N_1` or `N_2` accept

      - Define `δ` so that for any `q ∈ Q'` and any `a ∈ Σ_ε`:

        - $$
          \delta(q,a)=\begin{cases}\delta_1(q,a)&q\in Q_1\\\delta_2(q,a)&q\in Q_2\\\{q_1,q_2\}&q=q_0\ \text{and}\ a=\varepsilon\\\emptyset&q=q_0\ \text{and}\ a\ne\varepsilon\end{cases}
          $$

  - > The class of regular languages is closed under the concatenation operation

    - Proof:

      - Let `N_1 = (Q_1, Σ, δ_1, q_1, F_1)` recognize `A_1` and `N_2 = (Q_2, Σ, δ_2, q_2, F_2)` recognize `A_2`, construct `N = (Q, Σ, δ, q, F)` tp recognize `A_1 ◦ A_2`

      - $$
        Q=Q_1\cup Q_2
        $$

        - The states of `N` are all the states of `N_1` and `N_2`

      - The state `q_1` is the same as the start state of `N_1`

      - The accept states `F_2` are the same as the accept states of `N_2`

      - Define `δ` so that for any `q ∈ Q'` and any `a ∈ Σ_ε`:

        - $$
          \delta(q,a)=\begin{cases}\delta_1(q,a)&q\in Q_1\ \text{and}\ q\notin F_1\\\delta_2(q,a)&q\in F_1\ \text{and}\ a\ne\varepsilon\\\delta_1(q,a)\cup\{q_2\}&q\in F_1\ \text{and}\ a=\varepsilon\\\delta_2(q,a)&q\in Q_2\end{cases}
          $$

  - > The class of regular languages is closed under the star operation

    - Proof:

      - Let `N_1 = (Q_1, Σ, δ_1, q_1, F_1)` recognize `A_1`, construct `N = (Q, Σ, δ, q, F)` tp recognize `A_1*`

      - $$
        Q=\{q_0\}\cup Q_1
        $$

        - The states of `N` are the states of `N_1` plus a new start state

      - The state `q_0` is the new start state

      - $$
        F=\{q_0\}\cup F_1
        $$

        - The accept states are the old accept states plus the new start state

      - Define `δ` so that for any `q ∈ Q'` and any `a ∈ Σ_ε`:

        - $$
          \delta(q,a)=\begin{cases}\delta_1(q,a)&q\in Q_1\ \text{and}\ q\notin F_1\\\delta_1(q,a)&q\in F_1\ \text{and}\ a\ne\varepsilon\\\delta_1(q,a)\cup\{q_1\}&q\in F_1\ \text{and}\ a=\varepsilon\\\{q_1\}&q=q_0\ \text{and}\ a=\varepsilon\\\emptyset&q=q_0\ \text{and}\ a\ne\varepsilon\end{cases}
          $$



## Reading 4: Regular Expressions

- We can use the regular operations to build up expressions describing languages, which are called **regular expressions**

  - $$
    (0\cup 1)0*
    $$

    - `0` and `1` are shorthand for `{0}` and `{1}` => union of these leads to the language `{0, 1}`
    - Concatenation symbol can be thought of as implicit in regular expressions

  - The value of a regular expression is a language => example above: the language consisting of all strings starting with a `0` or `1`, followed by any number of `0`s

  - Order of operations: star, concatenation, union (unless parentheses are used)

- Formal Definition of a Regular Expression

  - Say that `R` is a **regular expression** if `R` is:

    - `a` for some `a` in the alphabet `Σ`
    - `ε`
    - `∅`
    - `(R_1 ∪ R_2)`, where `R_1` and `R_2` are regular expressions
    - `(R_1 ◦ R_2)`, where `R_1` and `R_2` are regular expressions
    - `(R_1*)`, where `R_1` is a regular expression

  - May be in danger of defining the notion of a regular expression in terms of itself => **circular definition**

    - Would be invalid
    - However, `R_1` and `R_2` are always smaller than `R`, so we're definining regular expressions in terms of smaller regular expressions, avoiding circularity
      - **Inductive definition**

  - `R+` is shorthand for `RR*`

    - `R*` has all strings that are 0+ concatenations of strings from `R`, while `R+` has all strings that are 1+ concatenations of strings from `R`

      - $$
        R^+\cup\varepsilon=R^*
        $$

  - `R^k` is shorthand for the concatenation of `k` `R`'s with each other

  - We use `L(R)` to distinguish between the language of `R` and the regular expression `R`

  - Identities:

    - $$
      R\cup\emptyset=R
      $$

    - $$
      R\circ\varepsilon=R
      $$

  - Elemental objects in a programming language, called **tokens** can be described with regular expressions

    - Once syntax has be described, automatic systems can generate the **lexical analyzer**, which initially processes the input program

- Equivalence with Finite Automata

  - Regular expressions and finite automata are equivalent in their descriptive power

    - Any regular expression can be converted into a finite automaton that recognizes the language it describes and vice versa

  - > A language is regular if and only if some regular expression describes it

    - > If a language is described by a regular expression, then it is regular

      - Proof:

        - Convert `R` into an NFA `N`

        - `R = a` for some `a` in `Σ` => then `L(R) = {a}`

          - $$
            N=(\{q_1,q_2\},\Sigma,\delta,q_1,\{q_2\})\\\delta(q_1,a)=\{q_2\}\\\delta(r,b)=\emptyset\ \text{for}\ r\ne q_1\ \text{or}\ b\ne a
            $$

        - `R = ε` => then `L(R) = ε` 

          - $$
            N=(\{q_1\},\Sigma,\delta,q_1,\{q_1\})\\\delta(r,b)=\emptyset\ \text{for any}\ r\ \text{and}\ b
            $$

        - `R = ∅` => then `L(R) = ∅`

          - $$
            N=(\{q\},\Sigma,\delta,q,\emptyset)
            \delta(r,b)=\emptyset\ \text{for any}\ r\ \text{and}\ b
            $$

        - $$
          R=R_1\cup R_2
          $$

        - $$
          R=R_1\circ R_2
          $$

        - $$
          R=R_1^*
          $$

          - For the last three cases, we use the constructions given in the proofs for closure

    - > If a language is regular, then it is described by a regular expression

      - **Generalized nondeterministic finite automata** are nondeterministic finite automata wherein the transition arrows may have any regular expressions as labels

        - GNFAs read blocks of symbols from the input, not just one input at a time like normal NFA
        - Require:
          - The start state has transition arrows going to every other state, but no arrows coming in from any other state
          - There is only a single accept state, and it has arrows coming in from every other state, but no arrows going to any other state
            - The accept state is not the same as the start state
          - Except for the start and accept states, one arrow goes from every state to every other state and also from each state to itself

      - Convert DFA to GNFA

        - Add a new start state with an `ε` arrow to the old start state and a new accept state with `ε` arrows from the old accept states
        - If any arrows have multiple labels, we replace each with a single arrow whose label is the union of the previous labels
        - Add arrows labeled `∅` between states that had no arrows

      - Convert GNFA to regular expression

        - Say the GNFA has `k` states

          - Since GNFAs have distinct start and accept states, we know that `k >= 2`
          - If `k > 2`, then we construct an equivalent GNFA with `k - 1` states => repeat until `k = 2`
            - Select a state, rip it out of the machine, and repair the remainder by altering the regular expressions that label each of the remaining arrows
          - When `k = 2`, there's a single arrow going from the start state to the accept state, labelled by the equivalent regular expression

        - Formally:

          - GNFAs are similar to NFAs except for the transition function:

            - $$
              \delta: (Q-\{q_{accept}\})\times(Q-\{q_{start}\})\rightarrow\mathcal{R}
              $$

              - The symbol `R` is the collection of all regular expressions over the alphabet `Σ`

            - A GNFA is a 5-tuple:

              - $$
                (Q,\Sigma,\delta,q_{start},q_{accept})
                $$

                - `Q` is the finite set of states

                - `Σ` is the input alphabet

                - The transition function is:

                  - $$
                    \delta: (Q-\{q_{accept}\})\times(Q-\{q_{start}\})\rightarrow\mathcal{R}
                    $$

                - `q_start` is the start state

                - `q_accept` is the accept state

          - A GNFA accepts a string `w` in `Σ*` if `w = w_1w_2 ... w_k`, where each `w_i` is in `Σ*` and a sequence of states `q_0, q_1, ... , q_k` exists such that:

            - $$
              q_0=q_{start}
              $$

            - $$
              q_k=q_{accept}
              $$

            - `R_i` is the expression on the arrow from `q_i-1` to `q_i`

      - Proof:

        - Let `M` be the DFA for language `A`

        - Convert `M` to GNFA `G` using `CONVERT(G)`, which uses **recursion** (it calls on itself)

          - `CONVERT(G)`:

            - Let `k` be the number of states of `G`

            - If `k = 2`, then `G` must consist of a start state, an accept state, and a single arrow connecting them and labeled with a regular expression `R` => return the expression `R`

            - If `k > 2`, we select any state `q_rip` in `Q` different from `q_start` and `q_accept` and let `G'` be the GNFA where:

              - $$
                Q'=Q-\{Q_{rip}\}
                $$

              - $$
                q_i\in Q'-\{q_{accept}\}\\q_j\in Q'-\{q_{start}\}\\\delta'(q_i,q_j)=(R_1)(R_2)^*(R_3)\cup(R_4)\\R_1=\delta(q_i,q_{rip})\\R_2=\delta(q_{rip},q_{rip})\\R_3=\delta(q_{rip},q_j)\\R_4=\delta(q_i,q_j) 
                $$

            - Compute `CONVERT(G')` and return this value

        - Claim: for any GNFA, `CONVERT(G)` is equivalent to `G`

        - Basis: prove the claim true for `k = 2` states

          - If `G` has only two states, it can have only a single arrow, which goes from the start state to the accept state
          - The regular expression that labels this arrow describes all the strings that allow `G` to get to the accept state
          - This expression is equivalent to `G`

        - Induction step: assume that the claim is true for `k - 1` states and use this assumption to prove that the claim is true for `k` states

          - Prove `G` and `G'` recognize the same language
            - Suppose `G` accepts an input `w`
            - In an accepting branch of the computation, `G` enters a sequence of states: `q_start, q_1, ... , q_accept`
            - If none of them is the removed state `q_rip`, then clearly `G'` also accepts `w`
              - Each of the new regular expressions labeling the arrows of `G'` contains the old regular expression as part of a union
            - If `q_rip` does appear, removing each run of consecutive `q_rip` states forms an accepting computation for `G'`
              - The states `q_i` and `q_j` bracketing a run have a new regular expression on the arrow between them that describes all strings taking `q_i` to `q_j` via `q_rip` on `G` => `G'` accepts `w`
            - The induction hypothesis states that when an algorithm calls itself recursively on input `G'`, the result is a regular expression that is equivalent to `G'` because `G'` has `k - 1` states
              - This regular expression is also equivalent to `G` and the algorithm is proved correct



## Reading 5: Nonregular Languages

- $$
  B=\{0^n1^n|\ n\ge 0\}
  $$

  - DFAs cannot recognize this language => you need to be able to track an indefinite number of 0s with a finite number of states

- The Pumping Lemma for Regular Languages

  - The **pumping lemma** states that all regular languages have a special property

    - If we can show that a language doesn't have this property, we are guaranteed that it is not regular
    - The property states that all strings in the language can be "pumped" if they are at least as long as a certain special value called the **pumping length**
      - This means that each such string contains a section that can be repeated any number of times with the resulting string remaining in the language

  - > If `A` is a regular language, then there is a number `p` (the pumping length) where if `s` is any string in `A` of length at least `p`, then `s` may be divided into three pieces, `s = xyz`, satisfying the following conditions:
    >
    > - For each `i >= 0`:
    >
    >   - $$
    >     xy^iz\in A
    >     $$
    >
    > - `|y| > 0`
    >
    > - `|xy| <= p`

  - Proof idea:

    - Let `M` be a DFA that recognizes `A`
    - Assign the pumping length `p` to be the number of states of `M`
    - If `s` in `A` has at least length `p`, then we can use **pigeonhole principle** to show that a state will be revisited
      - If `n` is the length of `s`, we know `n + 1` is greater than `p` since `n` is at least `p`
    - Since a state is revisited, it is possible to repeat the states that are visited between the revisited states

  - Proof:

    - Let `M` be a DFA recognizing `A` and `p` be the number of states of `M`

    - Let `s = s_1s_2 ... s_n` be a string in `A` of length `n`, where `n >= p`

    - Let `r_1, ... , r_n+1` be the sequence of states that `M` enters while processing `s`

      - $$
        r_{i+1}=\delta(r_i,s_i)\ \text{for}\ 1\le i\le n
        $$

      - This sequence has length `n + 1`, which is at least `p + 1`

    - By the pigeonhole principle, there must be 2 states in the first `p + 1` elements of the sequence that are the same state

      - The first is `r_j` and the second is `r_l`

        - $$
          l\le p+1\\x=s_1...s_{j-1},y=s_j...s_{l-1},z=s_l...s_n
          $$

    - `x` takes `M` from `r_1` to `r_j`, `y` takes `M` from `r_j` to `r_j`, and `z` takes `M` from `r_j` to `r_n+1`, which means `M` must accept `xy^iz` for `i >= 0`

    - `j != l`, so `|y| > 0`

    - `l <= p + 1`, so `|xy| < p`

  - Prove a language `B` is not regular using the pumping lemma

    - Use the pumping lemma to guarantee the existence of a pumping length `p` such that all strings of length `p` or greater in `B` can be pumped
    - Find a string `s` in `B` that has length `p` or greater but that cannot be pumped
    - Demonstrate that `s` cannot be pumped by considering all ways of dividing `s` into `x`, `y`, and `z` and, for each division, finding a value `i` where `xy^iz` is not in `B`
      - Involves grouping various ways of dividing `s` into several cases and analyzing them individually
    - The existence of `s` contradicts the pumping lemma if `B` were regular => `B` must be nonregular



## Reading 6: Context-Free Grammars

- **Context-free grammars** - describe certain features that have a recursive structure

  - First used in the study of human languages

- **Parser** - extracts the meanings of a program prior to generating the compiled code or performing the interpreted execution

  - A number of methodologies facilitate the construction of a parser once a CFG is available
  - The compiler extracts the meaning of the code to be compiled in a process called **parsing**

- **Context-free languages** - languages associated with CFGs

  - Includes all the regular languages and many additional languages

- **Pushdown automata** - class of machines recognizing the CFGs

- Context-Free Grammars

  - $$
    A\rightarrow0A1\\A\rightarrow B\\ B\rightarrow\#
    $$

  - A grammar consists of a collection of **substitution rules** or **productions**

    - Each of these appears as a line in the grammar, comprising of a symbol and a string separated by an arrow
      - The symbol is called a **variable**
        - Often represented by capital letters
        - One variable is designated as the **start variable**, usually occurring on the LHS of the top-most rule
      - The string consists of variables and other symbols called **terminals**
        - Analogous to the input alphabet and often are represented by lowercase letters, numbers, or special symbols

  - A grammar can be used to describe a language by generating each string in the following manner:

    - Write down the start variable
    - Find a variable that is written down and a rule that starts with that variable
    - Replace the written down variable with the RHS of that rule
    - Repeat until no variables remain

  - **Derivation** - the sequence of substitutions to obtain a string

    - Can be represented pictorially with a **parse tree**
    - All strings generated in these ways constitute the **language of the grammar**
      - Any language that can be generated by some CFG is called a **context-free language**

  - The symbol `|` is often used to abbreviate several rules with the same LHS

  - Formal Definition of a Context-Free Grammar

    - A **context-free-grammar** is a 4-tuple `(V, Σ, R, S)` where:
      - `V` is a finite set called the variables
      - `Σ` is a finite set, disjoint from `V`, called the terminals
      - `R` is a finite set of rules, with each rule being a variable and a string of variables and terminals
      - `S ∈ V` is the start variable
    - If `u`, `v`, and `w` are strings of variables and terminals and `A -> w` is a rule of the grammar, we say that `uAv` **yields** `uwv`
    - Often a grammar is specified by writing down only its rules

  - Designing Context-Free Grammars

    - Many CFLs are the union of simpler CFLs
    - Constructing a CFG for a language that happens to be regular is easy if you can first construct a DFA for that language
      - Make a variable `R_i` for each state `q_i` of the DFA
      - Add the rule `R_i -> aR_j` to the CFG if `δ(q_i, a) = q_j` us a transition in the DFA
      - Add the rule `R_i -> ε` if `q_i` is an accept state of the DFA
      - Make `R_0` the start variable of the grammar, where `q_0` is the start state of the machine
    - Certain CGLs contains strings with two substrings that are linked in the sense that a machine for such a language would need to remember an unbounded amount of information about one of the substrings to verify that it corresponds properly to the other substring
      - `R -> uRv`
    - Strings may contain certain structures that appear recursively as part of other (or the same) structures
      - Place the variable symbol generating the structure in the location of the rules corresponding to where that structure may recursively appear

  - Ambiguity

    - Sometimes a grammar can generate the same string in several different ways

      - String will have several different parse trees and thus several different meanings
      - We say this string is derived **ambiguously** in that grammar
        - If this occurs for a string in a grammar, we say that grammar is ambiguous

    - A derivation of a string `w` in a grammar `G` is a **leftmost derivation** if at every step the leftmost remaining variable is the one replaced

    - A string `w` is derived **ambiguously** in context-free grammar `G` if it has two or more different leftmost derivations

      - Grammar `G` is **ambiguous** if it generates some string ambiguously

    - Sometimes ambiguous grammars can be rewritten to become unambiguous

      - CFLs that can only be generated by ambiguous grammars are **inherently ambiguous**

    - Chomsky Normal Form

      - A CFG is in **Chomsky normal form** if every rule is of the form:

        - $$
          A\rightarrow BC\\A\rightarrow a
          $$

          - Where `a` is any terminal and `A`, `B`, and `C` are any variables
          - The rule `S -> ε`, where `S` is the start variable is also permitted

      - > Any context-free language is generated by a context-free grammar in Chomsky normal form

        - Proof:
          - Add a new start variable `S_0` and the rule `S_0 -> S`, guaranteeing that the start variable doesn't occur on the RHS of any rule
          - Remove `ε`-rules of the form `A -> ε`, where `A` is not the start variable
            - For each occurrence of `A` on the RHS of a rule, we add a new rule with that occurrence deleted
            - For the rule `R -> A`, we add `R -> ε` unless that rule was already removed previously
            - Repeat until there are no more`ε`-rules
          - Remove all unit rules of the form `A -> B`
            - Whenever a rule `B -> u` appears, we add the rule `A -> u` unless it was a unit rule already removed
            - Repeat until all unit rules are removed
          - Convert remaining rules into proper form
            - Replace all rules `A -> u_1u_2 ... u_k` where `k >= 3` and each `u_i` is a variable or terminal symbol with the rules `A -> u_1A_1`, `A_1 -> u_2A_2`, ..., `A_k-2 -> u_k-1uk`
              - The `A_i`s are new variables
              - We replace any terminal `u_i` in the preceding rules with the new variable `U_i` and add the rule `U_i -> u_i`



## Reading 7: Pushdown Automata

- **Pushdown automata** - like NFAs, but with an extra component called a **stack**, which provides additional memory beyond the finite amount available in the control

  - Allows PDA to recognize some nonregular languages
  - Equivalent in power to CFGs
    - Can now prove CFLs are context-free by creating a CFG or a PDA that recognizes it
  - Can write symbols on the stack and read them back later
    - Writing a symbol on the stack is referred to as **pushing** the symbol
    - Removing a symbol from the stack is referred to as **popping** the symbol
    - All access to the stack is done at the top of it => LIFO
  - Stack is valuable because it can hold an unlimited amount of information
    - Allows a PDA to store numbers of unbounded size
  - May be nondeterministic
    - Deterministic PDA and nondeterministic PDA are not equal in power => NPDA can recognize languages that no DPDA can
    - NPDA are equivalent in power to CFGs

- Formal Definition of a Pushdown Automata

  - A **pushdown automata** is a 6-tuple `(Q, Σ, Γ, δ, q_0, F)` where `Q`, `Σ`, `Γ`, and `F` are all finite sets and:

    - `Q` is the set of states

    - `Σ` is the input alphabet

    - `Γ` is the stack alphabet

    - The transition function is:

      - $$
        \delta:Q\times\Sigma_\varepsilon\times\Gamma_\varepsilon\rightarrow\mathcal{P}(Q\times\Gamma_\varepsilon)
        $$

    - `q_0 ∈ Q` is the start state

    - `F ⊆ Q` is the set of accept states

  - Computes as follows:

    - Accepts input `w` if `w` can be written as `w = w_1w_2...w_m` where each `w_i ∈ Σ_ε` and sequences of states `r_0, r_1, ... , r_m ∈ Q` and strings `s_0, s_1, ... , s_m ∈ Γ*` exist that satisfy the following three conditions:
      - `r_0 = q_0` and `s_0 = ε` => signifies that `M` starts out properly, in the start state and with an empty stack
      - For `i = 0, ... , m - 1`, we have `(r_i+1, b) ∈ δ(r_i, w_i+1, a)`, where `s_i = at` and `s_i+1 = bt` for some `a, b ∈ Γ_ε` and `t ∈ Γ_ε` => states that `M` moves properly according to the state, stack, and next input symbol
      - `r_m ∈ F` => states that an accept state occurs at the input end

  - Contains no formal mechanism for the PDA to test for an empty stack

    - Can use the special symbol `$` to signify beginning of stack

  - Contains no formal mechanism for checking if the input has reached the end

    - Allow the accept states to only work when the machine is at the end of the input

- Equivalence with Context-Free Grammars

  - > A language is context free if and only if some pushdown automata recognizes it

    - > If a language is context free, then some pushdown automata recognizes it

      - Proof:

        - Let `q` and `r` be the states of the PDA and let `a` be in `Σ_ε` and `s` be in `Γ_ε`

        - We want the PDA to go from `q` to `r` when it reads `a` and pops `s`, while also pushing the entire string `u = u_1 ... u_l` on the stack

        - We use the notation `(r, u) ∈ δ(q, a, s)` to mean that when `q` is the state of the automaton, `a` is the next input symbol, and `s` is the symbol on top of the stack, the PDA may read the `a` and pop the `s`, then push the string `u` onto the stack and go on to the state `r`

        - The states of `P` are `Q = {q_start, q_loop, q_accept} ∪ E`, where `E` is the set of states we need for implementing the shorthand just described

          - The start state is `q_start` and the only accept state is `q_accept`

        - Transition function:

          - Begin by initializing the stack to contain the symbols `$` and `S`

          - Main loop:

            - Case where the top of the stack contains a variable: 

              - $$
                \delta(q_{loop}, \varepsilon,A)=\{(q_{loop},w)|\ \text{where}\ A\rightarrow w\ \text{is a rule in}\ R\}
                $$

            - Case where the top of the stack contains a terminal:

              - $$
                \delta(q_{loop},a,a)=\{(q_{loop},\varepsilon)\}
                $$

            - Case where the empty stack marker `$` is on top of the stack:

              - $$
                \delta(q_{loop},\varepsilon,$)=\{(q_{accept},\varepsilon)\}
                $$

    - > If a pushdown automaton recognizes some language, then it is context free

      - We want to make a CFG that generates all the strings that a given PDA accepts

        - Design a grammar that does more

          - For each pair of states `p` and `q` in `P`, the grammar will have a variable `A_pq`, which generates all the strings that can take `P` from `p` with an empty stack to `q` with an empty stack
            - Such strings can also take `P` from `p` to `q`, regardless of the stack contents at `p`, leaving the stack at `q` in the same condition as it was at `p`

        - `P` has the following three features:

          - It has a single accept state, `q_accept`

          - It empties its stack before accepting

          - Each transition either pushes a symbol onto the stack or pops one off the stack, but it doesn't do both at the same time

            - Replace each transition that simultaneously pops and pushes with a two transition sequence that foes through a new state, and we replace each transition that neither pops nor pushes with a two transition sequence that pushes then pops an arbitrary stack symbol

          - First move on `P` must be a push, as the empty stack can't be popped => last move must be a pop

            - Two possibilities: the symbol popped at the end is the symbol that was pushed in the beginning, or not

              - If so, the stack is only empty at the beginning and end

                - $$
                  A_{pq}\rightarrow aA_{rs}b
                  $$

                  - `a` is the input read at the first move, `b` is the input read at the last move, `r` is the state following `p`, and `s` is the state preceding `q`

              - Otherwise, the initial symbol must have been popped at some point, making the stack empty

                - $$
                  A_{pq}\rightarrow A_{pr}A_{rq}
                  $$

                  - `r` is the state when the stack becomes empty

      - Proof:

        - Say that `P = (Q, Σ, Γ, δ, q_0, {q_accept})` and construct `G`

          - Variables of `G` are:

            - $$
              \{A_{pq}|\ p,q\in Q\}
              $$

          - Start variable is `A_q0,q_accept`

          - `G`'s rules:

            - For each `p, q, r, s ∈ Q`, `u ∈ Γ`, and `a, b ∈ Σ_ε`, if `δ(p, a, ε)` contains `(r, u)` and `δ(s, b u)` contains `(q, ε)`, put the rule `A_pq -> aA_rsb` in `G`
            - For each `p, q, r ∈ Q`, put the rule `A_pq -> A_prA_rq` in `G`
            - Finally, for each `p ∈ Q`, put the rule `A_pp -> ε` in `G`

        - Prove this construction works by demonstrating that `A_pq` generates `x` if and only if `x` can bring `P` from `p` with empty stack to `q` with empty stack

          - > If `A_pq` generates `x`, then `x` can bring `P` from `p` to empty stack with `q` to empty stack

            - Basis: the derivation has 1 step
              - A derivation with a single step must use a rule whose RHS contains no variables
              - The only rules in `G` where no variables occur on the RHS are `A_pp -> ε`
              - Input `ε` takes `P` from `p` with empty stack to `q` with empty stack so the basis is proved
            - Induction step:
              - Assume true for derivations of length at most `k`, where `k >= 1` and prove true for derivations of length `k + 1`
              - Suppose that `A_pq` derives `x` with `k + 1` steps
              - The first step in this derivation is either `A_pq -> aA_rsb` or `A_pq -> A_prA_rq`
                - First case, consider the portion `y` of `x` that `A_rs` generates, so `x = ayb`
                  - Since `A_rs` derives `y` with `k` steps, the induction hypothesis tells us that `P` can go from `r` on empty stack to `s` on empty stack
                  - Since `A_pq -> aA_rsb` is a rule of `G`, `δ(p, a, ε)` contains `(r, u)` and `δ(s, b, u)` contains `(q, ε)` for some stack symbol `u`
                  - If `P` starts at `p` with empty stack, after reading `a` it can go to state `r` and push `u` onto the stack
                  - Then, reading string `y` can bring it to `s` and leave `u` on the stack
                  - Then, after reading `b`, it can go to state `q` and pop `u` off the stack
                  - Therefore, `x` can bring it from `p` with empty stack to `q` with empty stack
                - Second case, consider the portions `y` and `z` of `x` that `A_pr` and `A_rq` respectively generate, so `x = yz`
                  - `A_pr` derives `y` in at most `k` steps and `A_rq` derives `z` in at most `k` steps, so the induction hypothesis tells us that `y` can bring `P` from `p` to `r`, and `z` can bring `P` from `r` to `q`, with empty stacks at the beginning and end
                  - `x` can bring it from `p` with empty stack to `q` with empty stack
              - Induction step complete

          - > If `x` can bring `P` from `p` with empty stack to `q` with empty stack, `A_pq` generates `x`

            - Basis: the computation has 0 steps
              - The computation starts and ends at the same state, say `p`
              - We must show that `A_pp` derives `x`
              - In 0 steps, `P` cannot read any characters, so `x = ε`
              - `G` has the rule `A_pp -> ε`, so the basis is proved
            - Induction step:
              - Assume true for computations of length at most `k`, where `k >= 0` and prove true for computations of length `k + 1`
              - Suppose that `P` has a computation, wherein `x` brings `p` to `q` with empty stacks in `k + 1` steps
              - Either the stack is empty only at the beginning and end of this computation, or it becomes empty somewhere else too
                - First case, the symbol (`u`) that is pushed at the first move must be the same as the symbol that is popped at the last move
                  - Let `a` be the input read in the first move, `b` be the input read in the last move, `r` be the state after the first move, and `s` be the state before the last move
                  - Then `δ(p, a, ε)` contains `(r, u)` and `δ(s, b, u)` contains `(q, ε)`, and so rule `A_pq -> aA_rsb` is in `G`
                  - Let `y` be the portion of `x` without `a` and `b`, so `x = ayb`
                  - Input `y` can bring `P` from `r` to `s` without touching the symbol `u` that is on the stack and so `P` can go from `r` to `s` with and empty stack to `s` with an empty stack on input `y`
                  - We have removed the first and last steps of the `k + 1` steps in the original computation on `x` so the computation on `y` has `(k + 1) - 2 = k - 1` steps
                  - The induction hypothesis tells us that `A_rs` derives `y`, hence `A_pq` derives `x`
                - Second case, let `r` be a state where the stack becomes empty other than at the beginning or end of computation
                  - The portions of computation from `p` to `r` and from `r` to `q` each contain at most `k` steps
                  - Say that `y` is the input read during the first portion and `z` is the input read during the second portion
                  - The induction hypothesis tells us that `A_pr` derives `y` and `A_rq` derives `z`
                  - Since rule `A_pq -> A_prA_rq` is in `G`, `A_pq` derives `x`
              - Induction step complete

  - > Every regular language is context free



## Reading 8: Deterministic Context-Free Languages

- Pumping lemma for CFLs

  - > If `A` is a context-free language, then there is a number `p` (the pumping length) where, if `s` is any string in `A` of length at least `p`, then `s` may be divided into five pieces `s = uvxyz` satisfying the conditions
    >
    >  - for each `i >= 0`
    >
    >    - $$
    >      uv^ixy^iz\in A
    >      $$
    >
    > - $$
    >   |vy|>0
    >   $$
    >
    > - $$
    >   |vxy|\le p
    >   $$

  - When `s` is being divided into `uvxyz`, condition 2 states that either `v` or `y` is not the empty string

  - Condition 3 states that the pieces `v`, `x`, and `y` together have length at most `p`

- Languages that are recognizable by deterministic pushdown automata are called deterministic context-free languages

  - Relevant to practical applications like the design of parsers in programming languages

- Conform to the basic principle of determinism: at each step of its computation, the DPDA has at most one way to proceed according to its transition function

  - May read an input symbol without popping a stack symbol and vice versa
  - Allow `ε` moves of two forms: **`ε`-input moves** corresponding to `δ(q, ε, x)` and **`ε`-stack moves** corresponding to `δ(q, a, ε)`
    - Move may combine both forms, corresponding to `δ(q, ε, ε)`
      - If a DPDA makes an `ε` move in a certain situation, it's prohibited from making a move in that same situation that involves processing a symbol other than `ε`

- A **deterministic pushdown automata** is a 6-tuple `(Q, Σ, Γ, δ, q_0, F)`, where `Q`, `Σ`, `Γ`, and `F` are all finite sets and:

  - `Q` is the set of states

  - `Σ` is the input alphabet

  - `Γ` is the stack alphabet

  - The transition function is:

    - $$
      \delta:Q\times\Sigma_\varepsilon\times\Gamma_\varepsilon\rightarrow(Q\times\Gamma_\varepsilon)\cup\{\emptyset\}
      $$

  - The start state is:

    - $$
      q_0\in Q
      $$

  - The set of accept states is:

    - $$
      F\subseteq Q
      $$

  - The transition function must satisfy the following condition:

    - For every `q ∈ Q`, `a ∈ Σ`, and `x ∈ Γ`, exactly one of the values  `δ(q, a, x)`, `δ(q, a, ε)`, `δ(q, ε, x)`, `δ(q, ε, ε)` is not the empty set
    - It may output either a single move of the form `(r, y)`, or it may indicate not action by outputting the empty set
    - Enforces nondeterminism by preventing the DPDA from having multiple moves in the same situation

  - A DPDA has exactly one legal move in every situation where the stack is nonempty

    - If the stack is empty a DPDA can move only if the transition function specifies a move that pops `ε`
    - Otherwise, the DPDA has no legal move and it rejects without reading the rest of the input

- The language of a DPDA is called a **deterministic context-free language**

- > Every DPDA has an equivalent DPDA that always reads the entire input string

- Properties of DCFLs

  - > The class of DCFLs is closed under complementation

    - Implies that some CFLs are not DCFLs => any CFL whose complement isn't a CFL isn't a DCFL

  - > `A` is a DCFL if and only if `A⊣` is a DCFL

- Deterministic Context-Free Grammars

  - In defining DCFGs, we take a bottom-up approach, starting with a string of terminals and processing the derivation in reverse, employing a series of reduce steps until reaching the start variable

    - Each **reduce step** is a reversed substitution, whereby the string of terminals and variables on the RHS of a rule is replaced by the variable on the corresponding LHS
    - The string replaced is called the **reducing string**
    - We call the entire reversed derivation a **reduction**

  - If `u` and `v` are strings of variables and terminals, a **reduction from `u` to `v`** is a sequence

    - $$
      u=u_1\rightarrowtail u_2\rightarrowtail\ ...\ \rightarrowtail u_k=v
      $$

      - We say that `u` **is reducible** to `v`
      - A **reduction from `u`** is a reduction from `u` to the start variable
      - In a **leftmost reduction**, each reducing string is reduced only after all other reducing strings that lie entirely to its left
        - Leftmost reduction is a rightmost derivation in reverse

  - Let `w` be a string in the languages of CFG `G` and let `u_i` appear in a leftmost reduction of `w` 

    - In a reduce step from `u_i` to `u_i+1`, say that the rule `T -> h` was applied in reverse
    - We can write `u_i = xhy` and `u_i+1 = xTy`, where `h` is the reducing string, `x` is the part of `u_i` that appears leftward of `h`, and `y` is the part of `u_i` that appears rightward of `h`
    - We call `h`, together with its reducing rule `T -> h`, a **handle** of `u_i`
      - A handle of a string `u_i` that appears in a leftmost reduction of `w` is the occurrence of the reducing string in `u_i`, together with the reducing rule for `u_i` in this reduction
    - A string that appears in a leftmost reduction of some string in `L(G)` is called a **valid string**
      - Handles are only defined for valid strings
      - Valid strings may have several handles if the grammar is ambiguous

  - Handles play an important role in defining DCFGs because handles determine reductions => once we know the handle of a string, we know the next reduce step

    - The initial part of a valid string, up to and including its handle, must be sufficient to determine the handle
      - We don't need to read beyond the handle in order to identify the handle
    - We say a handle `h` of a valid string `v = xhy` is a **forced handle** if `h` is the unique handle in every valid string `xhy_cap` where `y_cap` is in `Σ*`

  - A **deterministic context-free grammar** is a context-free grammar such that every valid string has a forced handle

  - The DK-test relies on the fact that for any CFG `G`, we can construct an associated DFA `DK` that can identify handles

    - `DK` accepts its input `z` if:
      - `z` is the prefix of some valid string `v = zy`
      - `z` ends with a handle of `v`
    - Each accept state of `DK` indicates the associated reducing rules



## Reading 9: Turing Machines

- Turing machines are similar to finite automaton, but with an unlimited and unrestricted memory

  - Much more accurate model of a general purpose computer
  - Can do everything a real computer can do
  - Cannot solve certain problems, which are beyond the theoretical limits of computation
  - Uses an infinite tape as its unlimited memory
    - Has a tape head that can read and write symbols, while also moving around on the tape
    - Initially contains only the input string, more information written onto the tape as needed
  - Continues computation until it decides to produce an output
    - If it doesn't enter an accepting or rejecting state, it will go on forever and never halt

- Differences between Turing machines and finite automata

  - A Turing machine can both write on the tape and read from it
  - The read-write head can move both to the left and to the right
  - The tape is infinite
  - The special states for rejecting and accepting take effect immediately

- Formal Definition of a Turing Machine

  - `δ` takes the form:

    - $$
      Q\times\Gamma\rightarrow Q\times\Gamma\times\{L,R\}
      $$

      - If the machine is in a certain state `q` and the head is over a tape square containing the symbol `a` and if `δ(q, a) = (r, b, L)`, the machine writes the symbol `b` replacing the `a`, and goes to state `r`
      - The third component is either `L` or `R`, indicating if the head moves to the left or right after writing

  - A **Turing machine** is a 7-tuple, `(Q, Σ, Γ, δ, q_0, q_accept, q_reject)`, where `Q`, `Σ`, `Γ` are all finite sets and:

    - `Q` is the state of sets

    - `Σ` is the input alphabet, not containing the blank symbol `⊔`

    - `Γ` is the tape alphabet, where `⊔` is in `Γ` and `Σ` is a subset of `Γ`

    - The transition function is:

      - $$
        \delta:Q\times\Gamma\rightarrow Q\times\Gamma\times\{L,R\}
        $$

        

    - The start state is:

      - $$
        q_o\in Q
        $$

    - The accept state is:

      - $$
        q_{accept}\in Q
        $$

    - The reject state is:

      - $$
        q_{reject}\in Q\\q_{reject}\in Q
        $$

  - Computation:

    - Initially, `M` receives its input `w = w_1w_2 ... w_n` on the leftmost `n` squares of the tap, and the rest of the tap is blank
      - Note that `Σ` doesn't contain the blank symbol, so the first blank appearing on the tape marks the end of the input
      - `M` proceeds according to the rules described by the transition function
        - If `M` ever tries to move left off the tape, the head stays in the same place
      - Computation continues until `M` enters either the accept or reject states, at which point it halts
        - If this never occurs, `M` goes on forever

  - A **configuration** of a Turing machine is a setting of the machine's state, tape contents, and head location

    - We write `uqv` where the current state is `q`, the current tape contents is `uv` and the head location is the first symbol of `v`
    - `C_1` **yields** `C_2` if the Turing machine can legally go from `C_1` to `C_2` in a single step
    - The **start configuration** of `M` on input `w` is the configuration `q_0w`, which indicates that the machine is in the start state `q_0` with its head at the leftmost position on the tape
    - In an **accepting configuration**, the state of the configuration is `q_accept`
    - In a **rejecting configuration**, the state of the configuration is `q_reject`
      - These are both **halting configurations** and do not yield further configurations
    - A Turing machine **accepts** input `w` if a sequence of configurations `C_1, C_2, ... , C_k` exists such that:
      - `C_1` is the start configuration of `M` on input `w`
      - Each `C_i` yields `C_i+1`
      - `C_k` is an accepting configuration

  - The collection of strings that `M` accepts is **the language of `M`**, or **the language recognized by `M`**, denoted `L(M)`

  - Call a language **Turing-recognizable** if some Turing machine recognizes it

  - When we start a Turing machine on an input, there are three outcomes: accept, reject, loop

    - A **loop** means the machine doesn't halt
    - The machine can fail to accept by rejecting or looping
    - **Deciders** are machine that always make a decision to accept or reject
      - A decider that recognizes some language is also said to **decide** that language
      - We call a language **Turing-decidable** if some Turing machine decides it

