Metasketch
A small, auditable language for drawing mathematical structures, statements about them, and checked proof steps in one diagrammatic system.
Metasketch is an experimental research prototype for a diagrammatic calculus for metamathematics.
In ordinary mathematics, we usually keep these things separate:
a mathematical object, such as a graph or algebra;
a statement about that object;
a proof of the statement; and
a diagram illustrating the proof.
Metasketch explores a more unified approach. It represents structures, simple logical statements, proof certificates, and selected “proofs about proofs” as related typed objects. The result can be checked by software and rendered as an auditable SVG diagram.
This is not a finished foundation for all of mathematics. It is a working finite prototype with a deliberately narrow logical fragment, explicit safety boundaries for self-reference, and three machine-checked Lean 4 theorem slices.
What you can do with it today
Metasketch currently supports:
finite typed structures and finite models;
directed monoid rewriting, including associativity and unit simplification;
finite relational patterns, such as graph queries;
existential-conjunctive statements, roughly “there exist objects satisfying all of these relations”;
explicit witness certificates for why a query holds;
reconstruction of a witness from raw proof evidence, without storing the answer separately;
transport of witnesses along structure-preserving maps;
selected coherence cells, which show that two different proof routes agree;
quotation of a lower-level proof object into a higher-level code object;
checked SVG diagrams with embedded semantic metadata and audit hashes.
A simple way to think about it is:
structure  +  statement  +  proof witness  +  diagram
                         ↓
                 one checked object
A concrete example
Suppose a graph contains a path
Alice → Bob → Carol
The statement
“There is a two-step path from Alice to Carol”
can be represented as a small relational pattern. Metasketch can:
check that the pattern really matches the graph;
store the witness, here Bob;
transport that witness through a graph homomorphism;
show that direct transport and step-by-step transport agree; and
render the whole certificate as a diagram.
The same system also handles a small algebraic example: it normalizes monoid expressions such as
((a · b) · c) · d
into a standard right-associated form,
a · (b · (c · d))
while recording the rewrite path and the coherence relation between alternative paths.
Why the project uses levels
A central goal is to let diagrams talk about other diagrams without quietly creating an unrestricted self-reference system.
Metasketch therefore uses levels:
Level 0: ordinary structures and proofs
Level 1: code that describes Level 0 objects
Level 2: code that describes Level 1 proof objects
A lower-level proof may be quoted and checked from a higher level. But the public interface does not allow a same-level, total, representation-complete truth operation.
This is not merely a software preference. It is the project’s practical response to the family of diagonal arguments associated with Tarski and Lawvere:
useful self-description: yes
unrestricted same-level truth: no
The current formalization does not claim to prove Tarski’s theorem or Lawvere’s theorem. It proves a narrower typed-interface safety result for Metasketch’s declared reflection grammar.
What has been machine-checked
The repository contains three Lean 4 formalizations, using Lean 4.31.0 and no external theorem-library dependencies.
Target
What is formalized
Scope
F1
Directed monoid normalization
Strict descent, right-comb normal forms, soundness, uniqueness, and confluence for the ground monoid fragment.
F2
Pointed raw-certificate reconstruction
Complete and consistent evidence determines a unique assignment; canonical erasure round-trips.
F3
Reflection-interface safety
The safe interface excludes same-level total representation-complete truth; it also proves a Boolean diagonal obstruction and legal cross-level predicate coding.
The v1.4 verification record reports that all three Lean files compile with no sorry, admit, custom axioms, unsafe, or partial def declarations.
Important limits:
F2 formalizes the evidence-reconstruction core. Parsing, relation membership, sort and arity checks, binder-spine validation, and finite-model enumeration remain checked in Python.
F3 is a theorem about the declared interface grammar and finite Boolean diagonal setting. It is not a theorem about universal evaluators, semantic truth in arbitrary languages, Tarski’s theorem, or Lawvere’s theorem.
The Python renderer is an auditable projection from checked proof objects to SVG. It is not yet a two-way visual proof editor.
Quick start
Run the Python regression suite
The Python kernel is dependency-free.
python run_all_tests.py
The v1.4 bundle records 156 passing regression checks.
Recheck the Lean theorem slices
Install the pinned Lean toolchain, then run:
cd formal/lean
./verify.sh
The script recompiles F1, F2, and F3 and prints their axiom footprints.
Explore the rendered examples
Open this file in a browser:
rendered_examples/index.html
The gallery includes:
a directed rewrite-theoretic pentagon;
a relational witness certificate;
direct versus sequential witness transport;
a quoted proof behind a level-raising membrane; and
a pointed-boundary example.
Repository map
metasketch0/
  api.py                 Supported checked public API
  syntax.py              Terms, predicates, contexts, and diagrams
  semantics.py           Finite-model interpretation
  proofs.py              Witness proofs and coherence cells
  raw_certificates.py    Raw proof evidence and witness reconstruction
  adequacy.py            Certificate-to-match correspondence
  rewriting.py           Directed monoid rewriting
  reflection.py          Level-indexed quotation and evaluation interfaces
  reflection_audit.py    Program E reflection-boundary audit
  render.py              Semantic scene graph and SVG generation
  certified_core.py      Replayable finite certificate checks

formal/
  lean/                  Lean 4 theorem slices and verification script
  fixtures/              Deterministic cross-language fixtures

design/                  Design documents and theorem notes
rendered_examples/       Auditable SVG examples and gallery
Trusted public API
Use metasketch0.api for normal construction. It validates objects before they enter proof checking, rendering, quotation, or theorem-facing code.
Useful entry points include:
checked_context
checked_term
checked_predicate
checked_diagram
checked_tile
checked_model
checked_raw_certificate
BoundaryAssignmentProof
certify_monoid_normalization
certify_pointed_reconstruction
Raw abstract-syntax-tree construction lives under metasketch0.unsafe_ast and is intentionally experimental.
What Metasketch is not yet
Metasketch is not yet:
a general theorem prover;
a complete implementation of regular logic or first-order logic;
a proof assistant replacement;
a universal diagrammatic foundation for mathematics;
a general proof of sound self-reference; or
a visual editor that can recover arbitrary proofs from arbitrary SVG files.
The present fragment is finite, equality-free, relation-only on the logical side, and intentionally conservative around reflection.
Research direction
The long-term aim is a visual language in which:
structural maps          become diagrammatic transformations
logical inferences       become proof tiles
proof-route agreement    becomes higher-dimensional coherence
quoted proofs            become typed higher-level objects
The immediate research priorities are to formalize more of the F2 front end, generalize coherence lifting beyond the current examples, and determine how far reflective interfaces can be extended without losing the project’s explicit stratification guarantees.
Status
Current release: v1.4
Best description: a finite, audited, partially Lean-verified research kernel for diagrammatic structure, existential witness proofs, coherence, and controlled reflection.
For technical detail, start with:
design/metasketch_reflective_diagrammatic_calculus_design_v1_4.md
formal/README.md
formal/lean/VERIFICATION.txt
License
No license is included in the current bundle. Add an explicit license before accepting outside contributions or publishing a public repository.
