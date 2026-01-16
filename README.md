# Multi-step-Koopman

MPC is widely used in real-time applications, but practical implementations are typically restricted to convex QP formulations to ensure fast and certified execution.  

A common workaround to address this limitation is to approximate the nonlinear dynamics via online linearization about the current state or a previous solution trajectory, resulting in a sequence of QPs. These methods are often referred to as model-based approximation approaches.

Recently, data-driven approximation approaches via the Koopman Operator have emerged as computationally efficient ways to handle nonlinear systems in QP-based MPC.

In contrast to localized linearization methods, Koopman-MPC approaches aim to construct a globally valid linear predictor in the lifted observable space. This distinction is also reflected in the resulting control laws: linearization-based MPC yields a QP parameterized directly by the current state $x(t)$, i.e.,  $u(t)=\mathrm{QP}(x(t))$, whereas Koopman-MPC yields a QP parameterized in a high-dimensional observable space via a nonlinear lifting map $\phi(\cdot)$, i.e., $u(t)=\mathrm{QP}(\phi(x(t))$. This richer parameterization explains the improved closed-loop performance often observed in Koopman-MPC 

Extended Dynamic Mode Decomposition (EDMD), one popular data-driven Koopman approach, first specifies a dictionary of nonlinear lifting observables $\phi(\cdots)$ a priori and then solves a convex least-squares problem to identify the linear state-space matrices $\{A,B,C\}$. EDMD offers scalable computation and algorithmic simplicity due to its reliance on least-squares estimation, but this comes at the cost of limited representation capacity and sensitivity to the choice of nonlinear observable dictionary. Moreover, its least-squares structure enables analytical bounds on model approximation errors in Koopman-MPC, which provides a tractable foundation for theoretical analysis.
