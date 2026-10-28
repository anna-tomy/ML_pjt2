Machine Learning Assignment 2
Design of a Vector Embedding for Capability Composition
Abstract

This project designs and implements a problem-specific vector
representation for formally specified states, goals and executable
capabilities. The primary objective is to preserve relationships needed
to identify compatible capabilities and construct complex capabilities
from simpler ones.

The implementation represents functional properties including
preconditions, effects, inputs, outputs and capability type. It
additionally represents operational attributes including execution time,
monetary cost, resource cost, risk and reliability.

The evaluation uses an e-commerce capability dataset and five
experiments covering compatibility, sequential composition, alternative
implementations, irrelevant capabilities and operational attributes.

1. Introduction

A capability-oriented application can be represented using states, goals
and executable capabilities. A capability has conditions under which it
can execute, inputs, outputs, effects, implementation information and
operational properties.

For capability composition, the representation must preserve more than
superficial similarity.

The central research question is:

Can formal states, goals and executable capabilities be represented in
a vector space while preserving relationships required for capability
compatibility, composition and construction of complex application
functionality?

The project focuses on representation rather than path-search
algorithms.

2. Formal Representation

The application is represented conceptually as:

A = (S, C, SI, G, R, K)

where S is the state space, C the set of capabilities, SI the
initial state, G the goal specification, R the resources and K the
global constraints and policies.

The implementation focuses on states, goals and capabilities.

3. State Representation

A state is represented as state-variable/value pairs. The experimental
dataset contains variables such as:

User.authenticated
User.role
Cart.exists
Cart.item_count
Order.exists
Payment.status
Notification.sent

For each variable/value feature:

1.0 means the specified value matches.
-1.0 means the variable is specified with another value.
0.0 means the variable is unspecified.

State vector dimension: 16.

4. Goal Representation

A goal is a collection of desired conditions. The experimental goal
includes:

Order.exists = True
Payment.status = SUCCESS
Notification.sent = True

Goals use the same condition encoding as states.

Goal vector dimension: 16.

5. Capability Representation

A capability is represented conceptually by type, inputs, outputs,
preconditions, effects, constraints, resources, operational quality,
reliability, availability and execution mechanism.

The implementation encodes:

[preconditions]
[effects]
[inputs]
[outputs]
[capability type]
[time cost]
[money cost]
[resource cost]
[risk]
[reliability]

Capability vector dimension: 48.

6. Operational Attributes

Five operational attributes are included:

Time cost
Money cost
Resource cost
Risk
Reliability

Time uses a logarithmic transformation:

log(1 + time)

to reduce the effect of large raw values.

7. Similarity

Cosine similarity is used:

sim(a,b) = (a · b) / (||a|| ||b||)

Similarity is not treated as the definition of composability.

The implementation distinguishes:

VECTOR SIMILARITY
        ≠
FUNCTIONAL COMPATIBILITY
        ≠
DEPENDENCY
8. Functional Compatibility

Compatibility is evaluated using precondition/effect and input/output
relationships.

The score is:

Compatibility =
    0.7 × precondition/effect score
  + 0.3 × input/output score

A separate dependency score indicates whether the second capability
actually depends on requirements supplied by the first.

9. Capability Composition

For:

C1 : S0 → S1
C2 : S1 → S2

the capabilities can be composed when the relevant requirements of C2
are satisfied by C1.

The experiment constructs:

CreateOrder
    →
MakePayment_API
    →
SendNotification

Composite properties:

Time cost:     225.00 ms
Money cost:    0.0650
Reliability:  0.990017

Effects include:

Order.exists = True
Order.status = PAID
Payment.status = SUCCESS
Notification.sent = True
10. Experimental Setup
States:          3
Goals:           1
Capabilities:    8

State dimension:       16
Goal dimension:        16
Capability dimension: 48

Capabilities:

CreateOrder
MakePayment_API
MakePayment_DB
MakePayment_GUI
SendNotification
CancelCart
BrowseCatalog
UpdateWishlist
11. Experiment 1 --- Capability Compatibility

Pair Vector Precondition/Effect Input/Output Dependency Compatibility
Similarity

CreateOrder → 0.0748 1.0000 1.0000 1.0000 1.0000
MakePayment_API

CreateOrder → MakePayment_API has low cosine similarity but perfect
functional compatibility. CreateOrder → CancelCart has higher cosine
similarity but lower functional compatibility.

This demonstrates that vector similarity and composability are not
identical relationships.

12. Experiment 2 --- Capability Composition

The chain is:

CreateOrder
    →
MakePayment_API
    →
SendNotification

Pairwise compatibility:

CreateOrder → MakePayment_API = 1.0000
MakePayment_API → SendNotification = 1.0000

The composite capability has:

Time cost:     225.00 ms
Money cost:    0.0650
Reliability:   0.990017

This demonstrates construction of a larger capability from smaller
capabilities.

13. Experiment 3 --- Alternative Implementations

The three payment implementations are:

MakePayment_API
MakePayment_DB
MakePayment_GUI

Pairwise similarities:

Pair Similarity

API ↔ Database 0.9372
API ↔ GUI 0.9383
Database ↔ GUI 0.9368

Their common functional structure keeps them closely related while their
capability types distinguish the implementation mechanisms.

14. Experiment 4 --- Irrelevant Capabilities

Reference:

MakePayment_API

Results:

Capability Similarity Compatibility Dependency

BrowseCatalog 0.3027 1.0000 0.0000
UpdateWishlist 0.2647 0.3000 0.0000
CancelCart 0.0135 0.3000 0.0000

BrowseCatalog receives compatibility 1.0000 because it has no
requirements that are violated. Its dependency score is 0.0000,
showing that it does not depend on payment.

This illustrates why compatibility and dependency are separately
reported.

15. Experiment 5 --- Operational Attributes

Implementation Time (ms) Money Resource Risk Reliability

API 120 0.050 1 0.010 0.992
Database 35 0.002 2 0.050 0.965
GUI 200 0.080 1 0.030 0.975

A controlled experiment changes only operational attributes of the API
capability:

Comparison Similarity

Original → Cheap/Fast 0.998981
Original → Expensive/Slow 0.935757
Cheap/Fast → Expensive/Slow 0.929360

Operational vectors:

Original:
[0.479579, 0.05, 0.1, 0.01, 0.992]

Cheap/Fast:
[0.304452, 0.001, 0.1, 0.001, 0.999]

Expensive/Slow:
[0.690875, 1.0, 1.0, 0.5, 0.5]

The functional fields remain unchanged while the operational dimensions
change.

16. Evaluation Against Required Properties
Capability identity

Different capabilities have distinct structured representations.

State awareness

Preconditions and effects are encoded using state-variable/value
features.

Precondition-effect compatibility

The implementation evaluates whether effects of one capability satisfy
preconditions of another.

Input-output compatibility

Inputs and outputs are represented and compared separately.

Similarity versus composability

Cosine similarity is explicitly separated from functional compatibility.

Composition

Sequential capabilities can be combined into composite capabilities.

Goal relevance

Goals are represented in the same condition-based space as states, and
capability effects provide the information needed to relate capabilities
to desired state conditions.

Operational properties

Time, money, resource cost, risk and reliability are included.

Consistency

Fixed feature ordering ensures that equivalent vector positions have
consistent meanings.

Efficiency

The representation uses fixed-size numeric vectors and direct
comparisons.

17. Limitations

The dataset is small and domain-specific, so the experiments do not
establish generalization to other application domains.

The feature space is manually designed and the compatibility weights are
manually selected:

70% precondition/effect
30% input/output

Functional compatibility is explicitly calculated from formal
relationships rather than inferred solely from cosine similarity.

The current evaluation also contains limited quantitative testing of
direct capability-to-goal relevance.

18. Conclusion

The implementation provides a structured, problem-specific vector
representation for formal states, goals and executable capabilities.

The representation combines state conditions, preconditions, effects,
inputs/outputs, capability type and operational attributes.

The experiments demonstrate that:

Vector similarity and functional compatibility capture different
relationships.
Alternative implementations of the same function remain closely
related.
Sequential capabilities can be combined into composite capabilities.
Operational attributes influence the resulting representation.

The resulting system therefore provides a concrete vector representation
supporting capability identity, state awareness, compatibility analysis,
sequential composition and operational-property representation.
