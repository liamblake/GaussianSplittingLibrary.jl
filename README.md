# GaussianSplittingLibrary.jl
[![Build Status](https://github.com/liamblake/GaussianSplittingLibrary.jl/actions/workflows/CI.yml/badge.svg?branch=main)](https://github.com/liamblake/GaussianSplittingLibrary.jl/actions/workflows/CI.yml?query=branch%3Amain)

This package provides pre-generated solutions to the following optimisation problem. Given the univariate standard Gaussian density $q(x)$ with mean $\mu$ and variance $\sigma^2$, we wish to find the optimal Gaussian mixture

$$\tilde{q}(x) = \sum_{i=1}^L{\omega_i \mathcal{N}(\mu_i, \sigma)},$$

where $\mu_i = \epsilon L \left(\frac{i-1}{L-1} - \frac12\right)$ for a spacing parameter $\epsilon$ to be optimised for. This amounts to solving the optimisation problem

$$\min_{\epsilon, \omega_1, ..., \omega_L}\left[L_2(q \parallel \tilde{q}) + \lambda\left(1 - \sum_{i=1}^{L}{\omega_i \mu_i^2}\right)\right]$$

$$\text{s.t.} \sum_{i=1}^{L}{\omega_i} = 1$$

$$ \omega_i > 0 \,\forall\, i = 1, ..., L $$ 

$$ \sum_{i=1}^{L}{\omega_i \mu_i^2} < 1 $$

where $L_2$ is the $L_2$ norm and $\lambda > 0$ is a parameter encouraging small-variance mixands with far apart means. 

This is based on the methods of 

- DeMars, Kyle J., Robert H. Bishop, and Moriba K. Jah. 2013. “Entropy-Based Approach for Uncertainty Propagation of Nonlinear Dynamical Systems.” Journal of Guidance, Control, and Dynamics 36 (4): 1047–57. https://doi.org/10.2514/1.58987.

- Kulik, Jackson, and Keith A. LeGrand. 2024. “Nonlinearity and Uncertainty Informed Moment-Matching Gaussian Mixture Splitting.” arXiv. https://doi.org/10.48550/arXiv.2412.00343.



## Usage
A solution to the optimisation problem is stored as struct:
```julia
struct SplitSolution
  L::Int
  epsilon::Int
  variance::Float64
  means::Vector{Float64}
end
```

For fast look-up, the solutions are stored in a Dictionary keyed by the number of samples `L`.

## Optimisation code 
This package also includes the code that was used to generate the hash map of optimal solutions, which allows you to regenerate the library with a different cost function, penalty parameter $\lambda$, and $L_{\mathrm{max}}$. 

