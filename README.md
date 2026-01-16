# Multi-step-Koopman

MPC is widely used in real-time applications, but practical implementations are typically restricted to convex QP formulations to ensure fast and certified execution.  A common workaround is to approximate nonlinear dynamics via online linearization, yielding a sequence of QPs. These methods are often referred to as model-based approximation approaches.

Recently, data-driven approximation approaches via the Koopman Operator have emerged as computationally efficient ways to handle nonlinear systems in QP-based MPC. In contrast to localized linearization methods, Koopman-MPC approaches aim to construct a globally valid linear predictor in the lifted observable space. This distinction is also reflected in the resulting control laws: linearization-based MPC yields a QP parameterized directly by the current state $x(t)$, i.e.,  $u(t)=\mathrm{QP}(x(t))$, whereas Koopman-MPC yields a QP parameterized in a high-dimensional observable space via a nonlinear lifting map $\phi(\cdot)$, i.e., $u(t)=\mathrm{QP}(\phi(x(t))$. This richer parameterization explains the improved closed-loop performance often observed in Koopman-MPC 

Extended Dynamic Mode Decomposition (EDMD), a representative Koopman approach, fixes a dictionary of observables $\phi(\cdots)$ and identifies the linear state-space matrices $\{A,B,C\}$ via convex least-squares. While EDMD is computationally scalable and analytically tractable, its performance is limited by the expressiveness and selection of the observable dictionary. Deep neural network (DNN) based representations of the observables and the linear state-space matrices $\{A,B,C\}$ can be jointly identified through a nonconvex learning problem. Within the DNN framework, the training objective can naturally be extended to a multi-step prediction loss. Notably, minimizing this loss promotes high-fidelity multi-step prediction, which is essential in finite-horizon MPC.

Extending existing EDMD approaches to a multi-step prediction minimization setting necessitates terms of the form $\{A^k,\cdots,A\}$, which breaks the convexity of the original least-squares problem and results in a nonconvex optimization formulation; For example,

$$
\min_{A,B}\sum_{j=1}^{M_m}\sum_{k=1}^{H}
\left\|\psi(x_{j,k})-A^k\psi(x_{j,0})
-\sum_{m=0}^{k-1} A^{k-1-m}Bu_{j,m}
\right\|_2^2 .
$$

where $H$ is the prediction horizon and $M_m$ is the number of sampled trajectories:

$$
\mathcal{D}= \left\{
(x_{j,0},x_{j,k},u_{j,0:k-1})
\;:\;
j\in\{1,\dots,M_m\},\;
k\in\{1,\dots,H\}
\right\}.
$$

($x_{j,k}$ and $u_{j,k}$ denote the state and control input at the $k$th sampling time of the $j$th trajectory). 
Moreover, condensing a Koopman-MPC problem into a QP ultimately requires the multi-step state-control mapping: $$(x_{1},\cdots,x_{H})=\mathbf{E}\phi(x(t))+\mathbf{F}(u_0,\cdots,u_{H-1})$$
which is obtained by recursively propagating the lifted linear dynamics defined by $\{A,B,C\}$ over the horizon $H$, where the matrices $\{\mathbf{E},\mathbf{F}\}$ are constructed from $\{A,B,C\}$ accordingly.  

## Method: ($\ell_1$-regularized) least-square multi-step EDMD

Motivated by this observation, this paper proposes a multi-step EDMD algorithm that directly learns the matrices $\{\mathbf{E},\mathbf{F}\}$ governing the multi-step state-control mapping, thereby bypassing the identification of $\{A,B,C\}$ and the subsequent construction of $\{\mathbf{E},\mathbf{F}\}$ used in conventional Koopman-MPC approaches. As a result, our algorithm admits a convex least-squares formulation.

We show that the proposed multi-step EDMD identification problem can be decomposed at a prediction horizon level and state coordinates level, enabling parallelized identification of $\{\mathbf{E},\mathbf{F}\}$ and facilitating the incorporation of row-wise $\ell_1$ regularization on $\mathbf{E}$ for dictionary pruning. Dictionary pruning mitigates the difficulty of dictionary selection by automatically removing functions that are irrelevant to the system dynamics. Consequently, we obtain a least-squares-based multi-step EDMD algorithm with parallelization and integrated dictionary pruning.

Furthermore, we provide a non-asymptotic analysis of both one-step and multi-step EDMD under finite data regimes. For one-step EDMD, we show that model errors compound through repeated composition, potentially leading to error growth that is exponential in the prediction horizon. In contrast, the proposed multi-step EDMD yields error bounds that depend only on the target multi-step mapping, rather than on the accuracy of intermediate EDMD approximations. This distinction provides a principled explanation for the improved long-horizon prediction performance observed with the proposed multi-step EDMD approach.


