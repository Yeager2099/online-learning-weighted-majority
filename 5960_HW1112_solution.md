# Report transcription

Text extracted from the author-provided course report. Figures are not reproduced and tables may lose formatting. Student identifiers and institutional email addresses are removed.

## Page 1

CIT 596 Online Fall 2026
Module 11–12
Name: Yeager Ye (QIUYU YE) Collaborator: wliying
1. For each of the following statements say whether it is true, false, or unknown, and provide a
short justification.
(a) If a problem is in P then it is also in NP. (5 points)
Solution:
(C) True. Let L ∈ P, so that some deterministic algorithm A decides L in time O(nk)
for a constant k. Recall that NP is the class of problems whose YES-instances admit a
certificatethatcanbeverified inpolynomialtime,i.e.L ∈ NP iffthereareapolynomial-
time verifier V and a polynomial q with
x ∈ L ⇐⇒ ∃y with |y| ≤ q(|x|) such that V(x,y) = 1.
Take V(x,y) := A(x): the verifier simply ignores the certificate and runs the decider.
Then V runs in polynomial time, and because A decides L correctly,
x ∈ L =⇒ V(x,(cid:15)) = 1, x ∈/ L =⇒ V(x,y) = 0 for every y,
sotheequivalenceaboveholdswiththeemptycertificatey = (cid:15)andq(|x|) = 0. HenceL ∈
NP. (Equivalently, a deterministic polynomial-time Turing machine is a special case of
a nondeterministic one, since it never branches, so P ⊆ NP.) This holds unconditionally:
it does not presuppose P = NP.
(b) Up to polynomial factors all problems in NP require the same amount of time to decide.
(5 points)
Solution:
(C)Unknown—and, moreprecisely, thestatementisequivalent toP=NP,sosettling
it would settle the P vs. NP question.
If P = NP the statement is true. Then every L ∈ NP is decided in time O(ncL) for a
constantc L. ForanytwoproblemsA,B ∈ NP wethenhaveT A(n) ≤ ncA andT B(n) ≥ 1
(any computation takes at least one step), so T A(n) ≤ ncA ·T B(n) and symmetrically
T B(n) ≤ ncB ·T A(n): the two running times differ by at most a polynomial factor.
If P (cid:54)= NP the statement is false. Then there is L ∈ NP \P. Since no polynomial-time
algorithm decides L, any correct decider for L needs super-polynomial time (it exceeds
every polynomial for infinitely many input lengths). On the other hand the trivial
problem ∅ (no input is a YES-instance) lies in P ⊆ NP and is decided in constant time
— it suffices to reject at once, without even reading the input. A polynomial multiple of
a constant is still bounded by a polynomial, whereas L needs more than any polynomial
on infinitely many lengths, so these two problems in NP are not within a polynomial
factor of each other.
So the statement is true iff P = NP. Since nobody knows which case holds, its truth
value is unknown. Note that the analogous statement restricted to the P problems is
different: all problems in P differ by at most a polynomial factor, simply because all of
them are decided within polynomials.
1

## Page 2

Name: Yeager Ye (QIUYU YE) Collaborator: wliying
(c) Up to polynomial factors, all problems that are NP-complete require the same amount
of time to decide. (5 points)
Solution:
(C) True. Let A and B be NP-complete. Because each of them is NP-hard, A ≤ B
p
and B ≤ A: there are maps f,g computable in polynomial time, with |f(x)| ≤ p (|x|)
p 1
and |g(y)| ≤ p (|y|) for polynomials p ,p , such that
2 1 2
x ∈ A ⇐⇒ f(x) ∈ B, y ∈ B ⇐⇒ g(y) ∈ A.
Consequences for the running times. IfB isdecidedbyanalgorithmrunningintimeT ,
B
then A is decided in time p(|x|)+T (q(|x|)) for polynomials p,q — compute f(x), then
B
run the decider for B — and symmetrically T (n) ≤ p(cid:48)(n)+T (q(cid:48)(n)). In particular:
B A
(i) A is decidable in polynomial time if and only if B is; so either all NP-complete
problems are in P or none of them is. This is the sense in which NP-complete problems
are “equally hard”.
(ii) If T (n) = nc is polynomial then T (n) ≤ p(n)+q(n)c is polynomial as well, and
B A
vice versa.
Hence the running times of any two NP-complete problems determine each other with
only a polynomial-time overhead, and no NP-complete problem is harder than another
by more than one polynomial-time reduction: they all stand or fall together. This is
exactly what the assertion of the question means, so the answer is TRUE. (Strictly
speaking the reduction gives T (n) ≤ poly(n)+T (poly(n)); that is the precise form
A B
of “the same up to polynomial factors” here, and it is what makes the whole theory of
NP-completeness meaningful.)
2. A Hamilton Cycle in a graph is a simple cycle that goes through all the vertices.
An instance of the Hamilton Cycle decision problem is a graph G = (V,E), and the question
is: Does G have a Hamilton Cycle? Suppose, hypothetically, that we are given an algorithm
that can decide this question and correctly output Yes or No for any input graph.
Given a graph G that has a Hamilton Cycle, i.e., is a YES-instance of Hamilton Cycle, show
how you can use this hypothetical algorithm as a subroutine, to design a polynomial-time
algorithm to find a Hamilton Cycle in G. Your algorithm should make only polynomially
many calls to the subroutine.
(15 points)
Solution:
(C) Let A be the hypothetical polynomial-time decider for Hamilton Cycle: for any graph
H, A(H) returns YES if H contains a Hamilton cycle and NO otherwise. We are given a
graph G = (V,E), |V| = n, that is promised to be a YES-instance, and we must exhibit a
Hamilton cycle of G, not merely decide that one exists. Note that a single call to A cannot
do the job: A returns only YES or NO, not the cycle itself, and one call on G would merely
re-confirm what the problem statement already tells us. The idea of a self-reduction is to call
A on many slightly modified copies of G so that the answers reveal the cycle edge by edge.
Algorithm (throw away every edge that is not needed).
(1) Set H ← G.
(2) For each edge e ∈ E(G), in any fixed order:
2

## Page 3

Name: Yeager Ye (QIUYU YE) Collaborator: wliying
• Build H −e: the same vertex set, with the single edge e removed. Call A(H −e).
• If A(H −e) answers YES, set H ← H −e (edge e is not needed by any Hamilton
cycle that is left).
• If it answers NO, leave H unchanged (edge e is essential).
(3) Output E(H) as a Hamilton cycle; to list it in cyclic order, start at any vertex and
repeatedly step to the unique neighbour not used yet, until returning to the start.
In words: we examine the edges one at a time and permanently delete an edge exactly when
the remaining graph still contains a Hamilton cycle. Once no edge can be removed any more,
nothing is left over except one Hamilton cycle, and that is what we output.
Proof of correctness. We prove two claims.
Claim 1 (invariant). Ateverymoment, H containsaHamiltoncycle. Proof. InitiallyH = G,
which contains one by hypothesis. Suppose H contains one and we process an edge e. If
A(H −e) answers NO, H is unchanged. If it answers YES, then H −e contains a Hamilton
cycle, and this becomes the new H. In both cases the invariant is preserved after the edge is
processed (here we use that A answers correctly).
Claim 2 (nothing but a cycle survives). Let H be the graph at the end of step (2) and
f
let C be a Hamilton cycle of H , which exists by Claim 1. Then E(H ) = E(C). Proof.
f f
E(C) ⊆ E(H ) because C is a subgraph of H . For the reverse inclusion, let e ∈ E(H )
f f f
and suppose for contradiction that e ∈/ E(C). When the loop processed e, the graph H
cur
at that moment satisfied E(H ) ⊆ E(H ), because the loop only ever deletes edges. Since
f cur
E(C) ⊆ E(H ) ⊆ E(H ) and e ∈/ E(C), the cycle C is still present in H −e, so H −e
f cur cur cur
is a YES-instance and A answered YES. The algorithm would therefore have deleted e,
contradicting e ∈ E(H ). Hence E(H ) ⊆ E(C), and the two edge sets are equal.
f f
By Claim 2 the final graph has exactly the n edges of a Hamilton cycle, so outputting E(H)
in step (3) outputs a Hamilton cycle of G. For the traversal: H contains C and has no
f
other edges, so every vertex has degree exactly 2 in H and H is connected; walking from
f f
any vertex, always taking the edge not yet used, therefore returns to the starting vertex after
exactly n steps and visits all n vertices.
Number of calls and running time. Step (2) loops once per edge of G, so A is called
exactly |E| ≤
(cid:0)n(cid:1)
times, which is polynomial in the input size. Each call is on a graph with n
2
verticesandatmost|E|edges, andbuildingH−eandreadingofftheanswercostsO(n+|E|)
per edge. If A runs in time T(n,m) on graphs with n vertices and m edges, the total running
time is
O(cid:0) |E|·(n+|E|)(cid:1)
+|E|·T(n,|E|) =
O(cid:0) n2T(n,n2)+n2(cid:1)
,
which is polynomial whenever T is (in particular, per the problem statement only the number
of calls matters and each call may be counted as O(1), which gives the concrete bound
O(|E|·(n+|E|)) = O(n4)), plus O(n) for step (3). Since G is promised to be a YES-instance,
the algorithm is never asked to handle the contrary case, and it always terminates with a
genuine Hamilton cycle.
3. The Hamilton Cycle Problem has been defined in the previous problem. For this problem,
you can assume that it is NP-complete. The Hamilton Path problem is as described below.
Problem: Hamilton Path
Instance: A Graph G = (V,E).
3

## Page 4

Name: Yeager Ye (QIUYU YE) Collaborator: wliying
Question: Does G have a simple path that goes through all the vertices?
Prove that Hamilton Path is NP-complete. Pay special attention to the proof of correctness
of your reduction.
Solution:
(C) Hamilton Path is in NP. A certificate is a sequence v ,v ,...,v of the vertices of G,
1 2 n
n = |V|, presented as the claimed Hamilton path. The verifier checks that (i) the sequence
is a permutation of V — every vertex appears exactly once, which is checked with a boolean
array in O(n) time — and (ii) (v ,v ) ∈ E for every i = 1,...,n−1, which takes O(n)
i i+1
edge lookups in an adjacency matrix or a hash set. Both checks run in time polynomial in
the input size, and a valid certificate exists exactly when G has a Hamilton path. Hence
Hamilton Path ∈ NP.
Hamilton Path is NP-hard: Hamilton Cycle ≤ Hamilton Path. Let G = (V,E) be
p
an instance of Hamilton Cycle, n = |V|.
Construction. If n ≤ 2, output the fixed graph G with two vertices and no edges, which has
0
no Hamilton path; this is correct because a simple cycle needs at least three vertices, so such
a G is a NO-instance of Hamilton Cycle. If n ≥ 3, choose any vertex v ∈ V, introduce three
new vertices v(cid:48),(cid:96) ,(cid:96) , and set
1 2
V(cid:48) = V ∪{v(cid:48),(cid:96) ,(cid:96) }, E(cid:48) = E ∪ {(v(cid:48),u) : u ∈ N (v)} ∪ {((cid:96) ,v(cid:48)), ((cid:96) ,v)},
1 2 G 1 2
where N (v) is the set of neighbours of v in G. Thus v(cid:48) is a clone of v (it is adjacent to
G
exactly the neighbours of v), (cid:96) is a leaf hanging off v(cid:48), and (cid:96) is a leaf hanging off v. The
1 2
output of the reduction is G(cid:48) = (V(cid:48),E(cid:48)), an instance of Hamilton Path. It has n+3 vertices
and |E|+|N (v)|+2 edges and is built in O(n+|E|) time, so the reduction is polynomial.
G
Correctness: G has a Hamilton cycle ⇐⇒ G(cid:48) has a Hamilton path.
(⇒) Let v,x ,x ,...,x ,v be a Hamilton cycle of G, so {x ,...,x } = V \ {v} and
1 2 n−1 1 n−1
(v,x ),(x ,v) ∈ E. Consider the sequence
1 n−1
(cid:96) , v(cid:48), x , x , ..., x , v, (cid:96) .
1 1 2 n−1 2
Every consecutive pair is an edge of G(cid:48): ((cid:96) ,v(cid:48)) ∈ E(cid:48); (v(cid:48),x ) ∈ E(cid:48) because x ∈ N (v);
1 1 1 G
(x ,x ) ∈ E ⊆ E(cid:48); (x ,v) ∈ E ⊆ E(cid:48); and (v,(cid:96) ) ∈ E(cid:48). The sequence lists the n + 3
i i+1 n−1 2
vertices of V(cid:48) each exactly once, so it is a Hamilton path of G(cid:48).
(⇐) Let P be a Hamilton path of G(cid:48). The vertices (cid:96) and (cid:96) have degree 1 in G(cid:48), with
1 2
N ((cid:96) ) = {v(cid:48)} and N ((cid:96) ) = {v}. A vertex of degree 1 in a graph that has a Hamilton path
G(cid:48) 1 G(cid:48) 2
must be an endpoint of that path (it has only one incident edge, and in a Hamilton path
it must have at least one), and a path has exactly two endpoints; hence (cid:96) and (cid:96) are the
1 2
endpoints of P. Reading P from the (cid:96) end, its second vertex is v(cid:48) (the only neighbour of
1
(cid:96) ), and its second-to-last vertex is v (the only neighbour of (cid:96) ). Since P visits every vertex
1 2
of V(cid:48) exactly once and {v(cid:48),(cid:96) ,(cid:96) } are used up by these three positions, the remaining n−1
1 2
positions are exactly the vertices of V \{v}; write them as x ,...,x . Thus
1 n−1
P = (cid:96) , v(cid:48), x , x , ..., x , v, (cid:96) .
1 1 2 n−1 2
We now read off the cycle v,x ,x ,...,x ,v in G and check its edges:
1 2 n−1
• (v,x ) ∈ E: the neighbours of v(cid:48) in G(cid:48) are exactly N (v)∪{(cid:96) }, and (v(cid:48),x ) is an edge
1 G 1 1
of P with x (cid:54)= (cid:96) ; hence x ∈ N (v).
1 1 1 G
4

## Page 5

Name: Yeager Ye (QIUYU YE) Collaborator: wliying
• (x ,v) ∈ E: symmetrically, the neighbours of v in G(cid:48) are exactly N (v)∪{(cid:96) }, and
n−1 G 2
(x ,v) is an edge of P with x (cid:54)= (cid:96) ; hence x ∈ N (v).
n−1 n−1 2 n−1 G
• (x ,x ) ∈ E for 1 ≤ i ≤ n−2: both endpoints lie in V \{v}, and every edge of E(cid:48)
i i+1
incident with one of the three new vertices v(cid:48),(cid:96) ,(cid:96) has that new vertex as an endpoint,
1 2
so an edge between two vertices of V \{v} must already belong to E.
The sequence v,x ,...,x ,v lists every vertex of V exactly once, starts and ends at v, and
1 n−1
uses only edges of E; with n ≥ 3 it is therefore a simple cycle, i.e. a Hamilton cycle of G.
The two directions together give the equivalence, and the small cases n ≤ 2 are handled
correctly by the construction (both sides are NO), so YES-instances map to YES-instances
and NO-instances to NO-instances.
Conclusion. Hamilton Path ∈ NP and Hamilton Cycle ≤ Hamilton Path, where Hamilton
p
Cycle is NP-complete; therefore Hamilton Path is NP-complete.
4. While Graph Isomorphism is not known (and not believed) to be NP-complete, the seemingly
related problem of subgraph isomorphism is NP-complete.
Problem Subgraph Isomorphism
Instance Two graphs G and H.
Question Isthereaninjective(i.e.,one-to-one)mappingf oftheverticesofGtothevertices
of H such that for any two vertices u, v in G, (u,v) ∈ E ⇔ (f(u),f(v)) ∈ E ?
G H
Prove the NP-completeness of subgraph isomorphism using any problem you know to be NP-
complete. (20
points)
Solution:
(C) Since the condition in the definition is an equivalence, the problem as stated asks for
an induced subgraph isomorphism: non-edges of G must map to non-edges of H as well.
We reduce from Clique, whose NP-completeness was established in lecture (see also the M11
Module Summary), so it may be cited directly as NP-complete.
Subgraph Isomorphism is in NP. A certificate is the mapping f, given as the list of the
|V | images f(u) ∈ V for u ∈ V . The verifier checks that (i) the listed images are pairwise
G H G
distinct, so that f is injective, and (ii) for every unordered pair {u,v} of distinct vertices of
G,
(u,v) ∈ E ⇐⇒ (f(u),f(v)) ∈ E .
G H
Step (i) takes O(|V |log|V |) time with sorting, or O(|V |2) by pairwise comparison; step
G G G
(ii) takes O(|V |2) adjacency lookups. Both are polynomial in the size of the instance, and a
G
certificate passes exactly when the instance is a YES-instance, so Subgraph Isomorphism ∈
NP.
Subgraph Isomorphism is NP-hard: Clique ≤ Subgraph Isomorphism. Recall that
p
Clique is NP-complete: given a graph H and an integer k, decide whether H contains a set
of k vertices that are pairwise adjacent.
Construction. On input (H,k) of Clique, let K be the complete graph on k freshly created
k
vertices and output the pair
G := K , H := H.
k
5

## Page 6

Name: Yeager Ye (QIUYU YE) Collaborator: wliying
That is, the pattern graph is the complete graph K and the host graph is the given graph H.
k
Writing down K costs O(k2) time (as an adjacency matrix) and copying H costs O(|V |2)
k H
at most, so the reduction runs in polynomial time.
Correctness: H has a k-clique ⇐⇒ there is an injective f : V(K ) → V(H) satisfying the
k
condition.
(⇒) Suppose v ,...,v are pairwise adjacent in H. Let u ,...,u be the vertices of K and
1 k 1 k k
define f(u ) = v . Then f is injective, because v ,...,v are distinct. Let i (cid:54)= j. Both sides of
i i 1 k
the equivalence hold: (u ,u ) ∈ E because K is complete, and (f(u ),f(u )) = (v ,v ) ∈
i j K k k i j i j
E because v and v are adjacent. So the condition is satisfied for every pair u (cid:54)= v of K ,
H i j k
and our instance is a YES-instance.
(⇐) Suppose an injective f : V(K ) → V(H) satisfies the condition, and let S = f(V(K )) ⊆
k k
V(H). Injectivity gives |S| = k. Take any two distinct a,b ∈ S; then a = f(u ) and
i
b = f(u ) for some i (cid:54)= j, and (u ,u ) ∈ E because K is complete. The condition
j i j K k k
(u ,u ) ∈ E ⇐⇒ (f(u ),f(u )) ∈ E then gives (a,b) = (f(u ),f(u )) ∈ E . Hence
i j K i j H i j H
k
every two vertices of S are adjacent, i.e. S is a k-clique of H.
(The case k > |V | needs no separate treatment: H has no k-clique, and no injective map
H
V(K ) → V(H) exists either, so both sides are NO.)
k
Conclusion. Clique ≤ Subgraph Isomorphism and Clique is NP-complete, so Subgraph
p
IsomorphismisNP-hard; togetherwithmembershipinNPthismakesSubgraphIsomorphism
NP-complete.
5. Suppose we were to modify the Go-with-weighted-Majority algorithm where instead of atten-
uating the weight of each expert by 1/2 on making a mistake, we attenuated by a factor of
(1−ε), for some ε ∈ (0,1/2). All other details of the algorithm remains the same.
(a) For each round t, let w(i) denote the weight of the ith expert after round t, and let
t
W =
(cid:80)N w(i)
. Let T be the total number of rounds our algorithm runs for, and let
t i=1 t
M be our algorithm’s loss. Show that W ≤ (1−ε/2)M ·N. (10 points)
T
Solution:
(C) The algorithm and the notation. Everything except the attenuation factor is as
(i)
inGo-with-weighted-Majority. ThereareN experts,expertistartswithweightw = 1,
0
so W = N. In each round t = 1,...,T:
0
(i)
(1) every expert i makes a prediction p ∈ {0,1};
t
(2) the algorithm predicts the value b ∈ {0,1} supported by the larger total weight, i.e.
t
the side whose supporters carry weight at least 1W (ties are broken arbitrarily);
2 t−1
(3) the true outcome y ∈ {0,1} is revealed, and every expert who predicted wrongly
t
has its weight multiplied by (1−ε):
(i)
(cid:40) (1−ε)w t( −i) 1, p( ti) (cid:54)= y t, (cid:88)N
(i)
w = W = w .
t (i) (i) t t
w , p = y ,
t−1 t t i=1
The algorithm’s loss is M = |{t : b (cid:54)= y }|, the number of rounds in which it predicted
t t
wrongly, and T is the number of rounds it is run for.
Bookkeeping for a single round. Let
(cid:88) (i)
X = w
t t−1
i:p( ti)(cid:54)=yt
6

## Page 7

Name: Yeager Ye (QIUYU YE) Collaborator: wliying
be the total weight of the experts who are wrong in round t. Each of them loses exactly
an ε fraction of its weight, and everybody else keeps theirs, so
W = W −εX . (1)
t t−1 t
Key claim. If the algorithm errs in round t, then X ≥ W /2.
t t−1
Proof. The algorithm predicted the weighted majority b , so the experts predicting
t
b carry weight at least 1W : there are only two possible predictions, every expert
t 2 t−1
predicts one of them, and the algorithm picked the side of larger (or equal) total weight.
Since the algorithm erred, the truth was y = 1−b , so the experts predicting b are
t t t
exactly the experts that are counted in X by (1). Therefore X ≥ W /2.
t t t−1
Bounding the total weight. In a round in which the algorithm is correct, (1) and
X ≥ 0 give W ≤ W . In a round in which it errs, the key claim together with (1)
t t t−1
gives
ε (cid:16) ε(cid:17)
W = W −εX ≤ W − W = 1− W .
t t−1 t t−1 t−1 t−1
2 2
Multiplying these inequalities over the T rounds — a factor (1−ε/2) for each of the M
rounds with a mistake, and a factor at most 1 for every other round — gives
(cid:16) ε(cid:17)M (cid:16) ε(cid:17)M
W ≤ 1− W = 1− N,
T 0
2 2
which is what was to be shown. (Note 1−ε/2 ∈ (0,1), because ε ∈ (0,1/2).)
(b) Let OPT be the loss of the best expert. Show that M ≤ 2·(1+ε)·OPT+ 2lnN. You
ε
may use the fact that −ε−ε2 < ln(1−ε) < −ε for every ε ∈ (0,1/2). (15 points)
Solution:
(C) Let i∗ be an expert whose loss is minimal, so L = OPT, where L denotes the
i∗ i
number of rounds in which expert i predicted wrongly. Its weight is multiplied by (1−ε)
exactly once per mistake and never otherwise, hence
w(i∗) = (1−ε)OPT.
T
(i∗)
All weights are non-negative, so W ≥ w , and part (a) gives
T T
(cid:16) ε(cid:17)M
(1−ε)OPT ≤ W ≤ 1− N. (2)
T
2
Taking natural logarithms (all quantities are positive, and both bases lie in (0,1), so
their logarithms are negative),
(cid:16) ε(cid:17)
OPT ln(1−ε) ≤ M ln 1− +lnN,
2
that is,
(cid:16) ε(cid:17)
M ln 1− ≥ OPT ln(1−ε)−lnN.
2
Since ln(1−ε/2) < 0, dividing by it reverses the inequality:
(cid:0) (cid:1)
OPT ln(1−ε)−lnN lnN −OPT ln(1−ε) lnN +OPT −ln(1−ε)
M ≤ = = .
ln(1−ε/2) −ln(1−ε/2) −ln(1−ε/2)
(3)
7

## Page 8

Name: Yeager Ye (QIUYU YE) Collaborator: wliying
Now we use the given inequalities, which say that for x ∈ (0,1/2)
x < −ln(1−x) < x+x2.
With x = ε this gives −ln(1 − ε) < ε + ε2 = ε(1 + ε). With x = ε/2, which lies in
(0,1/4) ⊂ (0,1/2), it gives −ln(1−ε/2) > ε/2. Substituting these two bounds into (3),
lnN +OPT·ε(1+ε) 2lnN
M < = +2(1+ε)OPT,
ε/2 ε
which is the claimed bound. So the algorithm’s loss exceeds the best expert’s loss by at
most a factor 2(1+ε) plus an additive term 2lnN that grows only logarithmically in the
ε
number of experts — which is why a larger pool of experts is essentially free.
8
