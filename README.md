# Multi-step-Koopman

MPC is widely used in real-time applications, but practical implementations are typically restricted to convex QP formulations to ensure fast and certified execution.  

A common workaround to address this limitation is to approximate the nonlinear dynamics via online linearization about the current state or a previous solution trajectory, resulting in a sequence of QPs. These methods are often referred to as model-based approximation approaches.

Recently, data-driven approximation approaches via the Koopman Operator have emerged as computationally efficient ways to handle nonlinear systems in QP-based MPC.

In contrast to localized linearization methods, Koopman-MPC approaches aim to construct a globally valid linear predictor in the lifted observable space. This distinction is also reflected in the resulting control laws: linearization-based MPC yields a QP parameterized directly by the current state $x(t)$, i.e.,  $u(t)=\mathrm{QP}(x(t))$, whereas Koopman-MPC yields a QP parameterized in a high-dimensional observable space via a nonlinear lifting map $\phi(\cdot)$, i.e., $u(t)=\mathrm{QP}(\phi(x(t))$. This richer parameterization explains the improved closed-loop performance often observed in Koopman-MPC 
