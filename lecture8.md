# Lecture 8: Predicate Logic

## Propositional Logic Review

So far we looked at propositional logic:

- propositions combined with boolean connectives
- we extended it with temporal connectives

**For example:**
- S1: If it rains then I take an umbrella
- S2: If I take an umbrella then it rains
- S3: If I do not take an umbrella then it is not raining

**Questions we looked at:**
- Do S1 and S2 have the same meaning?
- Do S1 and S3 have the same meaning?

### Limitations: Reasoning about Individuals and Properties

Propositional logic is somewhat restricted in that:

- we cannot reason about properties of an individual subject
- we cannot make statements about all/some individuals

**Example: Classical logical syllogism (Aristotle)**

- all humans are mortal
- Socrates is a human
- **Therefore:** Socrates is mortal

**Note:** Propositional logic is too weak for this!

## Motivation: Logic in Computer Science

Sometimes we need to go beyond propositions:

**Security & Databases:**
- Users can access their account only if they provide the right password
- A database API operation preserves the integrity of the database
- Users cannot access files they do not have permissions for

**Programming Examples:**
- If the length of the array $A$ is 0 then the list $L$ is empty (e.g. listFromArray)
- If $x$ is not 0 then divide$(y, x)$ terminates with no errors
- If $x$ is not 0 then divide$(y, x)$ terminates and returns $y$ divided by $x$

## Predicate Logic

### Introduction

A logic of predicates (relations, functions) over individuals that allows us to reason about properties and quantify over subjects.

### Ingredients for Predicate Logic Formulas

- **Constants:** $A, L$, etc.
- **Functions:** size, length, etc.
- **Relations:** elementOf, $=$, etc.
- **Variables:** $x, y$, etc.
- **Quantifiers:** $\forall$ (for all), $\exists$ (there exists)
- **Connectives:** $\wedge, \vee, \rightarrow, \neg$

### Examples of Predicate Logic Statements

- If the length of the array $A$ is 0 then the list $L$ is empty
- The length of the array $A$ is bounded by the size of the list $L$
- Each element of the array $A$ appears in the list $L$

### Arity and Function/Relation Types

A function or relation is:
- **Nullary** if it has 0 arguments
- **Unary** if it has 1 argument
- **Binary** if it has 2 arguments
- etc.

We say that the function or relation has **arity** 0, 1, 2, etc. A constant is simply a nullary function (i.e. needs no arguments).

### Anatomy of a Formula

**Terms** (individuals the logic talks about):
- Variables: $x, y, z, \ldots$
- Functions applied to terms: $\text{size}(x), f(x), \text{length}(A), \ldots$

**Formulas** (statements about individuals):
- Relations applied to terms: $\text{elementOf}(A, x), t_1 = t_2, \ldots$
- Propositional connectives: $\text{elementOf}(A, x) \rightarrow \text{elementOf}(L, x)$
- Quantifiers: $\forall x. (\text{elementOf}(A, x) \rightarrow \text{elementOf}(L, x))$

### Formal Syntax

$$\begin{aligned}
t &= x \mid f(t, \ldots, t) \\
\varphi &= P(t, \ldots, t) \mid \neg \varphi \mid \varphi \wedge \varphi \mid \varphi \vee \varphi \mid \varphi \rightarrow \varphi \mid \forall x. \varphi \mid \exists x. \varphi
\end{aligned}$$

### Example Formulas (Natural Language)

- Every student is younger than some instructor
- Every module has at least one lecturer
- Every module has exactly one module organizer
- For every integer one can find a larger integer
- If an integer is even then it is divisible by 4
- If an integer is divisible by 4 then it is even
- For some positive integer $n$: $2^n < n^2$
- For all positive integers $n$: $2^n \geq n^2$

### Example Formulas (Logical Notation)

$$\forall x. \text{student}(x) \rightarrow \exists y. \text{instructor}(y) \wedge \text{younger}(x, y)$$

$$\forall x. \text{module}(x) \rightarrow \exists y. \text{lecturer}(y, x)$$

$$\forall x. \text{module}(x) \rightarrow \exists y. \text{organizer}(y, x) \wedge \forall z. \text{organizer}(z, x) \rightarrow z = y$$

$$\forall x. \text{integer}(x) \rightarrow \exists y. \text{integer}(y) \wedge x < y$$

$$\forall x. (\text{integer}(x) \wedge \text{even}(x)) \rightarrow \text{divisible}(x, 4)$$

$$\forall x. (\text{integer}(x) \wedge \text{divisible}(x, 4)) \rightarrow \text{even}(x)$$

$$\exists n. \text{integer}(n) \wedge n > 0 \wedge \exp(2, n) < \exp(n, 2)$$

$$\forall n. (\text{integer}(n) \wedge n > 0) \rightarrow \exp(2, n) \geq \exp(n, 2)$$

## Predicate Logic Includes Propositional Logic

### Why?

**Example:** In propositional logic: $A \rightarrow B$

In predicate logic: $A \rightarrow B$ where $A$ and $B$ are relations with 0 arguments (nullary relations/propositions).

But in predicate logic we can express more:
- $\forall x. A(x) \rightarrow B(x)$

## Formal Definition of Predicate Logic

### Vocabulary and Syntax

Given a vocabulary $V$ of function and relation symbols, where each symbol has an arity (number of arguments):

**A term** $t$ over $V$ is:
- A variable: $x, y, z, \ldots$
- A function symbol applied to terms: $f(t_1, \ldots, t_n)$

**A formula** $\varphi$ over $V$ is:
- A relation symbol applied to terms: $P(t_1, \ldots, t_n)$
- A propositional connective applied to formulas: $\varphi_1 \wedge \varphi_2, \varphi_1 \vee \varphi_2, \varphi_1 \rightarrow \varphi_2, \neg \varphi_1$
- A quantification of a formula: $\forall x. \varphi_1, \exists x. \varphi_1$

### BNF Grammar

$$\begin{aligned}
t &= x \mid f(t, \ldots, t) \\
\varphi &= P(t, \ldots, t) \mid \neg \varphi \mid \varphi \wedge \varphi \mid \varphi \vee \varphi \mid \varphi \rightarrow \varphi \mid \forall x. \varphi \mid \exists x. \varphi
\end{aligned}$$

### Equality

A relation we typically want to have in our vocabulary is **equality**: $t_1 = t_2$

The intended meaning is: the terms on the two sides of the equality symbol are equal.

We write $t_1 = t_2$ (infix notation) rather than $=(t_1, t_2)$ (prefix notation).

## Predicate Logic Example: Family Relations

### Statement

For all $x$ and all $y$, if $x$ is the father of $m$ and if $y$ is a daughter of $x$, then $y$ is a sister of $m$.

### Encoding (Vocabulary 1)

**Vocabulary:** $V_1 = \{\text{m}, \text{f}, \text{S}, \text{D}\}$

- $m$ is a constant symbol (nullary function) representing an individual (a person)
- $f$ is a unary function symbol representing the Father function
- $S, D,$ and $=$ are binary relation symbols (Sister, Daughter, equality)

**Possible encoding:**
$$\forall x, y. (x = f(m) \wedge D(y, x)) \rightarrow S(y, m)$$

Note: $\forall x, y.$ is an abbreviation for $\forall x. \forall y.$

### Encoding (Vocabulary 2)

**Vocabulary:** $V_2 = \{\text{m}, \text{f}, \text{S}, \text{D}\}$

These symbols have the same intended meaning as before.

**Possible encoding:**
$$\forall y. D(y, f(m)) \rightarrow S(y, m)$$

## Variables: Free and Bound

### The Role of Variables

A variable stands for some individual that we do not specify. We then use a quantifier to bind that variable, specifying that it stands for all ($\forall$) or some ($\exists$) individual.
Possible encoding:

−∀x,y.(x=f(m)∧D(y,x))→S(y,m) note: ∀x,y. is just an abbreviation for ∀x.∀y. 
\begin{aligned}
& -\forall x, y .(x=f(m) \wedge D(y, x)) \rightarrow S(y, m) \\
& \text { note: } \forall x, y \text {. is just an abbreviation for } \forall x . \forall y \text {. }
\end{aligned}
​−∀x,y.(x=f(m)∧D(y,x))→S(y,m) note: ∀x,y. is just an abbreviation for ∀x.∀y. ​
Predicate logic example
Consider the statement:
For all xxx and all yyy, if xxx is the father of mmm and if yyy is a daughter of xxx, then yyy is a sister of mmm
Predicate logic encoding using V2={m,f,S,D}V_{2}=\{m, f, S, D\}V2​={m,f,S,D} :

these have the same intended meaning as before

Predicate logic example
Consider the statement:
For all xxx and all yyy, if xxx is the father of mmm and if yyy is a daughter of xxx, then yyy is a sister of mmm
Predicate logic encoding using V2={m,f,S,D}V_{2}=\{m, f, S, D\}V2​={m,f,S,D} :

these have the same intended meaning as before
Possible encoding:

−∀y.D(y,f(m))→S(y,m)
-\forall y . D(y, f(m)) \rightarrow S(y, m)
−∀y.D(y,f(m))→S(y,m)
Variables
Let us focus more on the role of variables.

A variable stands for some individual that we do not specify.
We then use a quantifier to bind that variable, i.e. specify that we meant it to stand for all (∀) or for some (∃) individual.

E.g. in this formula:
∀x.∀y. (x = f (m)) ∧ D(y, x) → S(y, m)
the variables x and y are bound by the ∀ quantifier.
Variables
Let us focus more on the role of variables.

A variable stands for some individual that we do not specify.
We then use a quantifier to bind that variable, i.e. specify that we meant it to stand for all (∀)(\forall)(∀) or for some (3) individual.
free and bound variables in Maths and CS:
Algebra: ∑i=1n2i+k\sum_{i=1}^{n} 2^{i+k}∑i=1n​2i+k
nnn and kkk are free variables, iii is a bound variable Calculus: ∫abc+sin⁡(2t+1)dt\int_{a}^{b} c+\sin (2 t+1) d t∫ab​c+sin(2t+1)dt
a,b,ca, b, ca,b,c are free variables, ttt is a bound variable Programming: global and local variables
the variables xxx and yyy are bound by the ∀\forall∀ quantifier.

### Free Variables

A variable that is not bound is called **free**. For example, $x$ is free in:
$$\forall y. (x = f(m)) \wedge D(y, x) \rightarrow S(y, m)$$

## Semantics of Formulas

We will next look at the semantics of predicate logic formulas. This means the meaning of formulas, which can be true or false, but it depends on the context.

For example, is this formula true?
$$\forall x. \text{student}(x) \rightarrow \exists y. \text{instructor}(y) \wedge \text{younger}(x, y)$$



Semantics of formulas
We will next look at the semantics of predicate logic formulas


Logic: t=x∣f(t,…,t)t = x \mid f(t, \ldots, t)t=x∣f(t,…,t)

Vocabulary: V⋅formulasV \cdot \text{formulas}V⋅formulas

Formulas: φ=P(t,…,t)∣¬φ∣φ∧φ∣φ∨φ∣φ→φ∣∀x.φ∣∃x.φ\varphi = P(t, \ldots, t) \mid \neg \varphi \mid \varphi \wedge \varphi \mid \varphi \vee \varphi \mid \varphi \rightarrow \varphi \mid \forall x . \varphi \mid \exists x . \varphiφ=P(t,…,t)∣¬φ∣φ∧φ∣φ∨φ∣φ→φ∣∀x.φ∣∃x.φ

Formulas: ∀x.student(x)→∃y.instructor(y)∧younger(x,y)\forall x . \text{student}(x) \rightarrow \exists y . \text{instructor}(y) \wedge \text{younger}(x, y)∀x.student(x)→∃y.instructor(y)∧younger(x,y)

Model: universe U\text{universe } Uuniverse U

Func/rel interpretations:


Semantics of formulas
We will next look at the semantics of predicate logic formulas


Logic: t=x∣f(t,…,t)t = x \mid f(t, \ldots, t)t=x∣f(t,…,t)
φ=P(t,…,t)∣¬φ∣φ∧φ∣φ∨φ∣φ→φ∣∀x.φ∣∃x.φ\varphi = P(t, \ldots, t) \mid \neg \varphi \mid \varphi \wedge \varphi \mid \varphi \vee \varphi \mid \varphi \rightarrow \varphi \mid \forall x. \varphi \mid \exists x. \varphiφ=P(t,…,t)∣¬φ∣φ∧φ∣φ∨φ∣φ→φ∣∀x.φ∣∃x.φ
∀x. student(x)→∃y. instructor(y)∧ younger(x,y)\forall x. \text{ student}(x) \rightarrow \exists y. \text{ instructor}(y) \wedge \text{ younger}(x, y)∀x. student(x)→∃y. instructor(y)∧ younger(x,y)

Formulas:

Model:
Universe $$U$$
Func/rel interpretations





Semantics of formulas
We will next look at the semantics of predicate logic formulas

Semantics of formulas
We will next look at the semantics of predicate logic formulas

Semantics of formulas
We will next look at the semantics of predicate logic formulas:

this means the meaning of formulas, and can be true or false
but it depends on the context, e.g. is this true?

∀x.student⁡(x)→∃y. instructor (y)∧ younger (x,y)
\forall x . \operatorname{student}(x) \rightarrow \exists y . \text { instructor }(y) \wedge \text { younger }(x, y)
∀x.student(x)→∃y. instructor (y)∧ younger (x,y)
well, it depends on:

who the students and the instructors are
and what is their age!

So, to give meaning to predicate logic formulas we first need to specify the setting we are talking about - we call this a model
Example models
∀x.student⁡(x)→∃y. instructor (y)∧ younger (x,y)
\forall x . \operatorname{student}(x) \rightarrow \exists y . \text { instructor }(y) \wedge \text { younger }(x, y)
∀x.student(x)→∃y. instructor (y)∧ younger (x,y)
with the vocabulary V={V=\{V={ student, instructor, younger }

one model could be:


U={U=\{U={ Nikos, Pasquale, Adam, Maz, Aisha }
instructors: Nikos, Pasquale
students: Adam, Aisha, Maz
younger relation: (Adam, Nikos), (Adam, Pasquale), (Adam, Maz), (Aisha, Adam), (Aisha, Nikos), (Maz, Nikos)
is the formula true in this model?

Example models
∀x.student⁡(x)→∃y. instructor (y)∧ younger (x,y)
\forall x . \operatorname{student}(x) \rightarrow \exists y . \text { instructor }(y) \wedge \text { younger }(x, y)
∀x.student(x)→∃y. instructor (y)∧ younger (x,y)
with the vocabulary V={V=\{V={ student, instructor, younger }
2. another model could be:

U={U=\{U={ Arthur, Adam, Maz, Aisha }
instructors: Arthur
students: Adam, Aisha, Maz
younger relation: (Adam, Arthur), (Adam, Maz), (Aisha, Adam), (Aisha, Arthur)
is the formula true in this model?

Models formally
Formally, given a vocabulary VVV, a model MMM for VVV consists of:

a set UUU of individuals - we call this the universe
an interpretation of each function and relation in VVV, i.e:
for each function symbol, we have an actual function over the universe UUU
for each relation symbol, we have an actual relation over the universe UUU

Models formally
Formally, given a vocabulary VVV, a model MMM for VVV consists of:

a set UUU of individuals - we call this the universe
an interpretation of each function and relation in VVV, i.e:
for each function symbol fff with arity kkk, we have a function

fM:Uk→U
f_{M}: U^{k} \rightarrow U
fM​:Uk→U

for each relation symbol PPP with arity kkk, we have a relation

PM⊆Uk
P_{M} \subseteq U^{k}
PM​⊆Uk
So, the model is : M=(U,fM,…,PM,…)M=\left(U, f_{M}, \ldots, P_{M}, \ldots\right)M=(U,fM​,…,PM​,…)
Example models
∀x.student⁡(x)→∃y.instructor⁡(y)∧younger⁡(x,y)
\forall x . \operatorname{student}(x) \rightarrow \exists y . \operatorname{instructor}(y) \wedge \operatorname{younger}(x, y)
∀x.student(x)→∃y.instructor(y)∧younger(x,y)
with the vocabulary V={V=\{V={ student, instructor, younger }

one model could be MMM with:


U={U=\{U={ Nikos, Pasquale, Adam, Maz, Aisha }
instructor M={_{M}=\{M​={ Nikos, Pasquale }
student M={_{M}=\{M​={ Adam, Aisha, Maz }
younger M={_{M}=\{M​={ (Adam, Nikos), (Adam, Pasquale),
(Adam, Maz), (Aisha, Adam), (Aisha, Nikos), (Maz, Nikos) }

Example models
∀x.student⁡(x)→∃y.instructor⁡(y)∧younger⁡(x,y)
\forall x . \operatorname{student}(x) \rightarrow \exists y . \operatorname{instructor}(y) \wedge \operatorname{younger}(x, y)
∀x.student(x)→∃y.instructor(y)∧younger(x,y)
with the vocabulary V={V=\{V={ student, instructor, younger }
2. another model could be MMM with:

U={U=\{U={ Arthur, Adam, Maz, Aisha }
instructor M={_{M}=\{M​={ Arthur }
student M={_{M}=\{M​={ Adam, Aisha, Maz }
younger M={_{M}=\{M​={ (Adam, Arthur), (Adam, Maz), (Aisha, Adam), (Aisha, Arthur) }

Functions and Relations

A function g1:U→Ug_{1}: U \rightarrow Ug1​:U→U maps elements of UUU to elements of UUU
A function g2:U2→Ug_{2}: U^{2} \rightarrow Ug2​:U2→U maps pairs of elements of UUU to elements of UUU
etc.
a relation P1⊆UP_{1} \subseteq UP1​⊆U is a set of elements of UUU
a relation P2⊆U2P_{2} \subseteq U^{2}P2​⊆U2 is a set of pairs of elements of UUU
etc.

Functions and Relations

A function g1:U→Ug_{1}: U \rightarrow Ug1​:U→U maps elements of UUU to elements of UUU
A function g2:U2→Ug_{2}: U^{2} \rightarrow Ug2​:U2→U maps pairs of elements of UUU to elements of UUU
etc.
a relation P1⊆UP_{1} \subseteq UP1​⊆U is a set of elements of UUU (i.e. P1:U→{P_{1}: U \rightarrow\{P1​:U→{ true, false }\}} )
a relation P2⊆U2P_{2} \subseteq U^{2}P2​⊆U2 is a set of pairs of elements of UUU (i.e. P2:U2→{P_{2}: U^{2} \rightarrow\{P2​:U2→{ true, false }\}} )
etc.

Functions and Relations

A function g1:U→Ug_{1}: U \rightarrow Ug1​:U→U maps elements of UUU to elements of UUU
A function g2:U2→Ug_{2}: U^{2} \rightarrow Ug2​:U2→U maps pairs of elements of UUU to elements of UUU
etc.
a relation P1⊆UP_{1} \subseteq UP1​⊆U is a set of elements of UUU (i.e. P1:U→{P_{1}: U \rightarrow\{P1​:U→{ true, false }\}} )
a relation P2⊆U2P_{2} \subseteq U^{2}P2​⊆U2 is a set of pairs of elements of UUU (i.e. P2:U2→{P_{2}: U^{2} \rightarrow\{P2​:U2→{ true, false }\}} )
etc.

In any model M=(U,…)M=(U, \ldots)M=(U,…), unless specified otherwise, the equality relation is interpreted to equality in UUU :
=M={(u,u)∣u in U}
=_{M}=\{(u, u) \mid u \text { in } U\}
=M​={(u,u)∣u in U}
A CS example (cyclic lists)
Consider the vocabulary V=head,next,reachV = {head, next, reach}V=head,next,reach where:

head is a constant (meaning the head, or first element, of a list)
next is a unary function (giving the next element in the list)
reach is a binary relation (reach(x, y) means we can reach y from x in the list)


A CS example (cyclic lists)
Consider the vocabulary V=head,next,reachV = {head, next, reach}V=head,next,reach where:

head is a constant (meaning the head, or first element, of a list)
next is a unary function (giving the next element in the list)
reach is a binary relation (reach(x, y) means we can reach y from x in the list)

Here is a model UUU:

U=u1,u2U = {u_1, u_2}U=u1​,u2​
headM=u1head_M = u_1headM​=u1​
nextMnext_MnextM​ is such that nextM(u1)=nextM(u2)=u2next_M(u_1) = next_M(u_2) = u_2nextM​(u1​)=nextM​(u2​)=u2​
reachMreach_MreachM​ is given by reach_M = {u_1, u_1}, (u_1, u_2), (u_2, u_2)}

In such cases, it helps to draw the model, as on the right above!

Semantics formally
∀x.student⁡(x)→∃y. instructor (y)∧ younger (x,y)
\forall x . \operatorname{student}(x) \rightarrow \exists y . \text { instructor }(y) \wedge \text { younger }(x, y)
∀x.student(x)→∃y. instructor (y)∧ younger (x,y)
We gave informal arguments of why the formula is/isn't true in given models. Can this be made formal?
Yes, there is a recursive way to go through the formula in order to establish its truth value.
Given a model MMM and a formula φ\varphiφ, we write:
M⊨φ
M \vDash \varphi
M⊨φ
to mean that φ\varphiφ is true in MMM. We may also say that MMM satisfies φ\varphiφ.
We next look at how to define M⊨φM \vDash \varphiM⊨φ.
The need for environments
We next look at how to define M⊨φM \vDash \varphiM⊨φ.
We need just one more ingredient:

recall that our definition will go down the formula φ\varphiφ
e.g. if φ\varphiφ is of the form ∀x.φ1\forall x . \varphi_{1}∀x.φ1​, then we would like to say:
M⊨∀x.φ1M \vDash \forall x . \varphi_{1}M⊨∀x.φ1​ holds if M⊨φ1M \vDash \varphi_{1}M⊨φ1​ holds for every value that xxx can take (from the universe)
so, we need to remember what values our variables are supposed to take
we therefore use an environment EEE that maps variables to elements of the universe UUU (think of this as a look-up table)

Semantics formally
We therefore define M,E⊨φM, E \vDash \varphiM,E⊨φ. We look at the form of φ\varphiφ :

if φ\varphiφ is P(t1,…,tn)P\left(t_{1}, \ldots, t_{n}\right)P(t1​,…,tn​) then interpret t1,…,tnt_{1}, \ldots, t_{n}t1​,…,tn​ into elements u1,…,unu_{1}, \ldots, u_{n}u1​,…,un​ of UUU, by:
replacing each variable xxx with its stored value E(x)E(x)E(x)
replacing each function fff with its interpretation fMf_{M}fM​

Then, M,E⊨P(t1,…,tn)M, E \vDash P\left(t_{1}, \ldots, t_{n}\right)M,E⊨P(t1​,…,tn​) holds if (u1,…,un)\left(u_{1}, \ldots, u_{n}\right)(u1​,…,un​) is in PMP_{M}PM​
Semantics formally
We therefore define M,E⊨φM, E \vDash \varphiM,E⊨φ. We look at the form of φ\varphiφ :

if φ\varphiφ is P(t1,…,tn)P\left(t_{1}, \ldots, t_{n}\right)P(t1​,…,tn​) then interpret t1,…,tnt_{1}, \ldots, t_{n}t1​,…,tn​ into elements u1,…,unu_{1}, \ldots, u_{n}u1​,…,un​ of UUU, [...]

Then, M,E⊨P(t1,…,tn)M, E \vDash P\left(t_{1}, \ldots, t_{n}\right)M,E⊨P(t1​,…,tn​) holds if (u1,…,un)\left(u_{1}, \ldots, u_{n}\right)(u1​,…,un​) is in PMP_{M}PM​

if φ\varphiφ is one of φ1∧φ2,φ1∨φ2,φ1→φ2,¬φ1\varphi_{1} \wedge \varphi_{2}, \varphi_{1} \vee \varphi_{2}, \varphi_{1} \rightarrow \varphi_{2}, \neg \varphi_{1}φ1​∧φ2​,φ1​∨φ2​,φ1​→φ2​,¬φ1​ then:
M,E⊨¬φ1M, E \vDash \neg \varphi_{1}M,E⊨¬φ1​ holds if M,E⊨φ1M, E \vDash \varphi_{1}M,E⊨φ1​ does not hold
M,E⊨φ1∧φ2M, E \vDash \varphi_{1} \wedge \varphi_{2}M,E⊨φ1​∧φ2​ holds if M,E⊨φ1M, E \vDash \varphi_{1}M,E⊨φ1​ and M,E⊨φ2M, E \vDash \varphi_{2}M,E⊨φ2​ hold
M,E⊨φ1∨φ2M, E \vDash \varphi_{1} \vee \varphi_{2}M,E⊨φ1​∨φ2​ holds if M,E⊨φ1M, E \vDash \varphi_{1}M,E⊨φ1​ or M,E⊨φ2M, E \vDash \varphi_{2}M,E⊨φ2​ holds
M,E⊨φ1→φ2M, E \vDash \varphi_{1} \rightarrow \varphi_{2}M,E⊨φ1​→φ2​ holds if M,E⊨φ1M, E \vDash \varphi_{1}M,E⊨φ1​ doesn't hold or M,E⊨φ2M, E \vDash \varphi_{2}M,E⊨φ2​ holds

Semantics formally
We therefore define M,E⊨φM, E \vDash \varphiM,E⊨φ. We look at the form of φ\varphiφ :

if φ\varphiφ is P(t1,…,tn)P\left(t_{1}, \ldots, t_{n}\right)P(t1​,…,tn​) then interpret t1,…,tnt_{1}, \ldots, t_{n}t1​,…,tn​ into elements u1,…,unu_{1}, \ldots, u_{n}u1​,…,un​ of UUU, [...]

Then, M,E⊨P(t1,…,tn)M, E \vDash P\left(t_{1}, \ldots, t_{n}\right)M,E⊨P(t1​,…,tn​) holds if (u1,…,un)\left(u_{1}, \ldots, u_{n}\right)(u1​,…,un​) is in PMP_{M}PM​

if φ\varphiφ is one of φ1∧φ2,φ1∨φ2,φ1→φ2,¬φ1\varphi_{1} \wedge \varphi_{2}, \varphi_{1} \vee \varphi_{2}, \varphi_{1} \rightarrow \varphi_{2}, \neg \varphi_{1}φ1​∧φ2​,φ1​∨φ2​,φ1​→φ2​,¬φ1​ then use propositional logic rules
if φ\varphiφ is a quantification of a formula, i.e. ∀x.φ1\forall x . \varphi_{1}∀x.φ1​ or ∃x.φ1\exists x . \varphi_{1}∃x.φ1​ then:
M,E⊨∀x.φ1M, E \vDash \forall x . \varphi_{1}M,E⊨∀x.φ1​ holds if, for all uuu in U:M,E[x↦u]⊨φ1U: M, E[x \mapsto u] \vDash \varphi_{1}U:M,E[x↦u]⊨φ1​ holds
M,E⊨∃x.φ1M, E \vDash \exists x . \varphi_{1}M,E⊨∃x.φ1​ holds if, for some uuu in U:M,E[x↦u]⊨φ1U: M, E[x \mapsto u] \vDash \varphi_{1}U:M,E[x↦u]⊨φ1​ holds

Example revisited
φ=∀x.student⁡(x)→∃y. instructor (y)∧ younger (x,y)
\varphi=\forall x . \operatorname{student}(x) \rightarrow \exists y . \text { instructor }(y) \wedge \text { younger }(x, y)
φ=∀x.student(x)→∃y. instructor (y)∧ younger (x,y)
Consider MMM as below and let [ ] be the empty environment.

U={U=\{U={ Nikos, Pasquale, Adam, Maz, Aisha }
instructor M={_{M}=\{M​={ Nikos, Pasquale }
student M={_{M}=\{M​={ Adam, Aisha, Maz }
younger M={_{M}=\{M​={ (Adam, Nikos), (Adam, Pasquale),
(Adam, Maz), (Aisha, Adam), (Aisha, Nikos), (Maz, Nikos) }
so M,[]⊨φ⇔M,[] \vDash \varphi \LeftrightarrowM,[]⊨φ⇔ for all uuu in UUU,

M,[x↦u]⊨student⁡(x)→∃y. instructor (y)∧ younger (x,y)
M,[x \mapsto u] \vDash \operatorname{student}(x) \rightarrow \exists y . \text { instructor }(y) \wedge \text { younger }(x, y)
M,[x↦u]⊨student(x)→∃y. instructor (y)∧ younger (x,y)
and now we can check that this is true by examining each uuu in UUU
e.g. M,[x↦M,[x \mapstoM,[x↦ Nikos ]⊨student⁡(x)→∃y] \vDash \operatorname{student}(x) \rightarrow \exists y]⊨student(x)→∃y. instructor (y)∧younger⁡(x,y)(y) \wedge \operatorname{younger}(x, y)(y)∧younger(x,y) holds because M,[x↦M,[x \mapstoM,[x↦ Nikos ]⊨student⁡(x)] \vDash \operatorname{student}(x)]⊨student(x) does not hold why not? Because Nikos is not in student M{ }_{M}M​ !
Example revisited
φ=∀x.student⁡(x)→∃y. instructor (y)∧ younger (x,y)
\varphi=\forall x . \operatorname{student}(x) \rightarrow \exists y . \text { instructor }(y) \wedge \text { younger }(x, y)
φ=∀x.student(x)→∃y. instructor (y)∧ younger (x,y)
Consider MMM as below and let [ ] be the empty environment.

U={U=\{U={ Nikos, Pasquale, Adam, Maz, Aisha }
instructor M={_{M}=\{M​={ Nikos, Pasquale }
student M={_{M}=\{M​={ Adam, Aisha, Maz }
younger M={_{M}=\{M​={ (Adam, Nikos), (Adam, Pasquale),
(Adam, Maz), (Aisha, Adam), (Aisha, Nikos), (Maz, Nikos) }
so M,[]⊨φ⇔M,[] \vDash \varphi \LeftrightarrowM,[]⊨φ⇔ for all uuu in UUU,

M,[x↦u]⊨student⁡(x)→∃y. instructor (y)∧ younger (x,y)
M,[x \mapsto u] \vDash \operatorname{student}(x) \rightarrow \exists y . \text { instructor }(y) \wedge \text { younger }(x, y)
M,[x↦u]⊨student(x)→∃y. instructor (y)∧ younger (x,y)
and now we can check that this is true by examining each uuu in UUU
e.g. M,[x↦M,[x \mapstoM,[x↦ Adam ]⊨student⁡(x)→∃y] \vDash \operatorname{student}(x) \rightarrow \exists y]⊨student(x)→∃y. instructor (y)∧younger⁡(x,y)(y) \wedge \operatorname{younger}(x, y)(y)∧younger(x,y) holds because M,[x↦M,[x \mapstoM,[x↦ Adam ]⊨∃y] \vDash \exists y]⊨∃y. instructor (y)∧younger⁡(x,y)(y) \wedge \operatorname{younger}(x, y)(y)∧younger(x,y) holds
which holds because M,[x↦M,[x \mapstoM,[x↦ Adam, y↦y \mapstoy↦ Nikos ]⊨] \vDash]⊨ instructor (y)∧younger⁡(x,y)(y) \wedge \operatorname{younger}(x, y)(y)∧younger(x,y) holds (as Nikos is in instructor M{ }_{M}M​ and (Adam,Nikos) is in younger M{ }_{M}M​ )!
Summary
We introduced predicate logic, which allows us to talk about individuals and their properties, and to quantify over them
We looked at different vocabularies and thus different predicate logic syntaxes
We examined the semantics of a predicate logic formula and we saw that this is defined with respect to a specific model, which needs to be given; we also looked at different models
With models defined, we defined the semantics of a predicate logic formula (formally and informally), and looked at some examples
More examples in the exercises!
