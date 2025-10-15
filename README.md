# FEM-BEM hybrid method to solve the demagnetisation field

From Maxwell's equations, the main PDE we get to solve for the demagnetisation field in the absence of an electric current is:
$$
\Delta u = \nabla \cdot \vec{M},
$$
where $u$ magnetosatic potential, and $\vec{M}$ is the magnetisation. This equation holds over a domain $\Omega$ where magnetisation is finite. The Poisson equation has an open boundary condition as $u$ decays to zero at infinity. Further, one can obtain the demagnetisation field from the magnetistatic potential as:
$$
\vec{H}_{dmg} = - \nabla{u}.
$$

This can be solved using standard FEM formulation, however, one needs to mesh region outside $\Omega$ to apply the Dirichlet boundary condition $\lim_{x\to\infty} u(x) = 0$. To obtain $u$ over $\Omega$ without meshing region with no magnetisation, a hybrid FEM-BEM technique is used as presented by [Fredkin & Koehler](https://ieeexplore.ieee.org/abstract/document/106342?casa_token=DFfg4bkytawAAAAA:2VGK2UMtHkyFV8Oy8AgzQgkf2A5w1YDifH08BTpjo7A06EcFqN8cJC02Ku1r-KK4hAD0Ln7Nrw). The method essentially divides $u$ in two parts, $u_1$ and $u_2$, such that over the domain $\Omega$:
$$
\begin{align}
    \Delta{u_1} &= \nabla \cdot \vec{M}; \, \frac{\partial{u_1}}{\partial{n}} = \vec{M} \cdot \hat{n} \text{ on d} \Omega\\
    u_2(x') &= \oint_{\text{d}\Omega} u_1(x) \cdot \frac{\partial{G(x, x')}}{\partial{n}} \, \text{d}S + \big( \frac{\Psi(x')}{4\pi} - 1 \big) u_1(x'); \, x' \in \text{d}\Omega \\
    \Delta{u_2} &= 0; \, \text{Dirichlet BC from above.} \\
    u &= u_1 + u_2.
\end{align}
$$

The second step is obtained through BEM formulation. The goal here is to go over each step and solve for $u_1$ and $u_2$, and thus, ultimately $u$.
