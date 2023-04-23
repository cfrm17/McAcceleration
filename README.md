# Credit Pricing Model Monte Carlo Acceleration


A new procedure is presented to accelerate the convergence of Monte Carlo simulations using the Default Correlation model.  It is found that the modifications have been implemented correctly,  and that the modifications result in a substantial improvement in the convergence rate of the Monte Carlo simulation models.  

A particular benefit of the model is a dramatic improvement in the stability of  delta computations, allowing more accurate and timely risk management of basket trades.  The reason for the improvement in stability of the deltas is a change in the method of their calculation.  The new method is found to be quite accurate  for low and moderate correlations and is explained in detail in the next section.

The new method generates small negative delta amounts in certain cases.  Based on the numerical results, these negative deltas are not confined to situations of high correlation and/or large spread differences between credits.  The negative deltas do seem, however,  to be consistent with zero and to therefore be the result of a noise component introduced in certain cases by the acceleration algorithm.

A typical Monte Carlo simulation algorithm assigns each simulation run, or path, an identical probability weight.  If we allow, however, a different probability to be assigned to each simulation path, we allow more flexibility in the simulation and can thus shape the simulation in accordance with our needs.  For example, we can choose the probabilities of each path in manner such that the simulation is guaranteed to reproduce the prices of “benchmark” securities, whose prices are known from market data.  A simulation thus calibrated to benchmarks will then price off-market securities in a realistic manner.

The technique of assigning probability weights has, at least theoretically, the additional benefits of accelerating the convergence of the simulation, as well as allowing the sensitivities of the simulated price with respect to the benchmark securities to be computed without needing to perform additional simulations.

To see how this works, consider that a contingent claim has a value    for simulation path  .  The value computed according to equally-weighted Monte Carlo will be given by

  (1)

whereas for non-uniformly weighted Monte Carlo we have

  (2)

where   is the probability weight for path  .    

Obviously then, we require a method of computing the probability weights  .    Let us consider that we have   benchmark instruments  .  Let the value of the   benchmark instrument in the   simulation path be denoted  .  Then, in order to match the benchmark instruments, as required,  we must have

  (3)

for all  .

Given that the number of benchmark instruments  , the number of simulations, the system of equations (3) is severly underdetermined, admitting possibly an infinite number of solutions.  The question thus arises as to which of the many solutions to choose.

Intuitively, it makes sense to choose a solution which is as close as possible, in some sense, to the standard  , while simultaneously matching the benchmark prices.  The task now is to define, in this context, what we mean my “as close as possible”.  To do this, let

 

where   is a convex function.   The search for a set of probabilities   which minimize   subject to the constraints (3) can be performed by introducing    Lagrange multipliers   and solve the Lagrangian dual problem, given by

  (4)

There are many possible choices for  , among them

 

which corresponds to the relative entropy,

 

which corresponds to the square-root (Skorohod) distance, and

 

which corresponds to the least-squares distance.

The formal solution to the optimization problem (4) is given by

 .

Here we will consider the relative entropy minimization,  .  It can be shown that the probability weights   which maximize (4) given a set   is given by

  (5)

where   is the partition function, defined as

 . (6)

The minimum of (4) over   can be obtained by minimizing the objective function

 , (7)

then if   are the Lagrange multipliers which minimize (7), we can compute the necessary path probabilities   as

 . (8)

A slight modification of the above procedure can be made if it is felt that an exact match to the benchmark prices is undesirable.  This situation may occur due to liquidity considerations, or other difficulties in obtaining accurate prices for the benchmark securities.  In this case, we allow a small discrepancy between the input benchmark prices and those implied by the calibrated simulation.  This effectively means that we prefer calibrated path probabilities which are closer to the uniform   in exchange for slight mismatches in the benchmarks.  This shifts the relative importance of the benchmarks versus the relative entropy, placing more emphasis on entropy.

To allow this slight mismatch of benchmarks, we introduce a tolerance term into the objective function  , as

  (9)

where   is an overall tolerance factor, while   is an optional tolerance factor, which may in general by a function of the benchmark price  .

There exist many methods by which the objective function   or   may be minimized, some of which require explicit computation of the gradient of (7) or (9).  This is given by

  (10)

and

 . (11)


The path weighting procedure lends itself to an efficient method of calculating sensitivities with respect to the input benchmark prices, or deltas.   The major assumption which makes this application of the method practicable is that the distributions involved in the Monte Carlo simulation from which the paths are generated do not depend strongly on the benchmark prices.  If this is the case, then we can reuse the simulation paths from one simulation run, re-calibrating them to match perturbed benchmark prices.  The security in question is then repriced with the new path weights, enabling the computation of the hedge ratios.

Let the cashflows of a security in simulation path   be denoted  .  We will then have

 .

The sensitivity of   with respect to the benchmark securities   is then written

 .  (16)

Using equation (5) a straightforward calculation shows that

  (17)

and since our benchmark securities are given by equation (3), then we have

  (18)

from which we have the result that

 . (19)

If we perform a linear regression analysis of the   over the benchmark expectations  , that is we compute

  (20)

then we find that

 . (21)

Of course, if the sensitivities desired are not with respect to the benchmark securities, then one has more work to do.  In our case, however, equation (19) is exactly what we want.

It can be shown that in the case that the path weights   are computed using the relative entropy, that the regression (20) can be replaced by a re-computation of the weights using the simulation paths already computed, and the perturbed benchmark securities.

The constraints corresponding to equation (3) for the case of credit contingent contracts can be written in the form of default probabilities, since there is a one-to-one correspondence between between the input credit spreads (prices) and the unconditional default probabilities of individual names.

We are thus able to compute, for each name involved in a given trade, the probability  that the name will survive until time  .  Let   be the unconditional probability that name   survives until time  .  Then we can write

  (12)

where

   (13)

where in turn   is the default time of name   in the  -th simulation path.

However, given the nature of the Monte Carlo simulation used for the pricing of credit derivatives, a more efficient implementation of the constraints (12) can be obtained if we instead define the default probability   as

 

which is the probability that  , ie., that name   defaults between times   and  . We then write

  (14)

where   is the “dual” of  , that is

 . (15)

Running the simulation allows us to track  , from which we can compute the partition function  , and thus to determine those path weights which reproduce the input single-name default probabilities.


Reference:

https://finpricing.com/product.html

