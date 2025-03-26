# OpenQASM 3.0 `minimal` profile


## Overview

This *profile* defines a minimum subset of the OpenQASM 3.0 language
that implementors are assumed to support. This should be sufficient to
form a target for experiment authors or circuit libraries who intend
to express *executable* OpenQASM -- that is, OpenQASM programs that
may be assumed to be runnable by conforming implementations.

The [OpenQASM specification](https://openqasm.com/) serves as the
definition of the language. This profile introduces assumptions or
constraints on top of the language specification, effectively defining
a subset of valid OpenQASM programs. This profile also gives
suggestions or guidelines for implementors.

In this document, we introduce a small bit of jargon loosely inspired
by [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119):
- `MAY` indicates some aspect of the language whose support is
  required, and whose usage is consistent with this minimal profile
- `MAY NOT` indicates some aspect of the language that might be
  supported by particular implementors, but which implies that the
  OpenQASM program is not within the purview of this minimal profile
- `SHOULD` and `SHOULD NOT` indicate some best-effort requirement on
  behalf of implementors. Typically this is not a constraint on the
  set of programs that meet the profile, but rather a suggestion as to
  their meaning or execution.

## This Profile

At a high level, this profile assumes that implementors can execute
"straight-line circuits" with quantum measurements. We do not consider
nontrivial control flow or classical data processing. The goal is to
make this truly minimal: we are ready to assume a certain amount of
upstream language processing.

##  Language Requirements

1.   MAY NOT use `include` statements, other than `include "stdgates.inc"`
	-  Why: I am not convinced that `include` is used much. I would
       also assume that the search path depends on implementation
       specific details.
2. MAY NOT declare or use *variables* of any classical type
	- Why: Implementations are probably quite uneven both in the
      types, operations, and circumstances in which classical data
      computed at run-time may be used. It's not needed for many
      "textbook" quantum circuits.
3. MAY use `float` literals
	- Why: needed for parametric gates
4. MAY NOT use integer, boolean, bit string, or duration literals
	- Why: These are only usable with other language features I am
      excluding; hence we can exclude these.
5. MAY NOT use any other compile-time constant expressions
	- Why: Reduce barrier to entry for implementors. It's easy to
      imagine a library (or even a visitor in the OpenQASM repo)
      folding constants, so we'd end up at the literals.
6. MAY NOT use qubit registers or classical registers
	- Why: Register manipulations require bounds checking -- why bother.
7. MAY NOT declare qubits (`qubit <name>;`)
	- Why: Use of user-defined qubits implies that language processors
      manage a symbol table, and that implementors support some kind
      of physical qubit mapping/allocation. The latter typically
      imposes some constraints and to state clear assumptions here
      would require going beyond a "minimal" profile.
8. MAY use physical qubits
	- Why: Easy for language processors, and more or less
      unavoidable. Implementors will still have constraints on the
      number of qubits and their supported operations.
9. MAY NOT declare new gates
	- Why: The "gates = unitaries" model of OpenQASM is far from the
      reality of hardware implementations. This implies some sort of
      compilation -- we can assume that this has already been done for
      us.
10. MAY use  "built-in gates" or gates defined in `stdgates.inc`
	- Why: Hard to avoid this, but by merit of this being a fixed set
      of gates implementations can hardcode the implementation as
      needed.
11. MAY NOT use gate modifiers
	- Why: Unnecessary complexity at this level -- we assume that
      these have been compiled away.
12. MAY NOT use broadcasting in gate applications
	- Why: This is implied in our restriction to physical qubits, but
      worth stating more explicitly: this is unnecessary complexity.
13. MAY use `reset` as the first operation on a physical qubit
	- Why: Not sure if the spec assumes zero-initialization otherwise,
      but this is easy to support or ignore in pretty much all
      implementations I am aware of.
14. MAY NOT use `reset` in any other context
	- Why: I wouldn't want to require implementors support active reset.
15. MAY use `measure` statements as the last operation on a physical qubit
	- Why: Measurement is necessary but implementations differ
      somewhat in how they handle it. This seems to be the least we
      can get away with. Note that since we don't allow `bit`
      declarations, measurement values cannot be used in the program
      itself. We assume that implementations have some way of making
      these available to the user (but probably there is value in
      pinning this down at some point).
16. MAY NOT use classical data-flow or control-flow instructions
	- Why: There is a lot of variation as to what is allowed or
      disallowed here by implementors. And it's not necessary for the
      most basic circuits.
17. MAY NOT rely on user-defined subroutines
	- Why: Lots of questions re: what we want to mandate here. This would go in another profile.
18. MAY NOT rely on vendor-supplied externs
	- Why: We'd like portability in the "minimal" profile, and aren't
      in a position to declare what externs are assumed just yet.
19. MAY use pragmas
	- Why: Can be ignored / passed through by libraries & implementors as needed.
20. MAY use annotations
	- Why: Can be ignored / passed through by libraries & implementors as needed.
21. MAY NOT use `barrier` statements
	- Why: The barrier statement is awkward and abstraction breaking
      in the OpenQASM spec. In principle, gates are unitary
      operations, and don't imply any particular physical
      realization. But `barrier` is intended to "prevent commutation
      and gate reordering". This seems to assume some underlying model
      of execution that is not articulated by the spec. If we wish to
      allow `barrier` in this profile, we should consider defining
      SHOULD or SHOULD NOT requirements for implementors.
22. MAY NOT use `delay` statements
	- Why: Totally ignoring timing in this profile. It's an abstraction-breaking concept that is hard to get right.
23. MAY NOT use `box` statements
	- Why: Totally ignoring timing in this profile. It's an abstraction-breaking concept that is hard to get right.
24. MAY NOT introduce `defcal` statements
	- Why: Out of scope for this profile.

### Implementation Requirements

TODO
