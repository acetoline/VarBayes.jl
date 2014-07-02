## VarBayes.jl

This package implements both expectation-maximization (EM) and variational Bayes inference on graphical models. It is designed to be as general as possible, with the ability to work with arbitrary user-specified distributions, with minimal mathematical 'busywork'. At the moment, mixture models (finite and Dirichlet process) are fully supported.

### Background

The problem with most implementations of EM and variational methods is that the implementations are usually rigid and only have limited customizability. If one desires a model that is only slightly different from the one that was implemented, it often requires writing the whole model from scratch, re-deriving all the equations associated with the model. This is time-consuming and it makes model experimentation difficult. An even more important problem is ensuring that the resulting model is correct. Debugging graphical models can be very time-consuming due to their stochastic nature.

While packages exist for doing MCMC sampling from arbitrary models (such as OpenBUGS and JAGS), MCMC sampling is slow and often has issues with convergence. There have been various proposals to implement symbolic transformation of model specifications to executable code (e.g. a 'graphical model compiler' for a 'graphical model domain-specific language') but this approach, so far as has been implemented, continues to be fragile and has difficulties with extending to complex, real-world models.

The aim in this package is to take an entirely different approach. Instead of a program that writes your model for you, you still have to write your own model. However, the package gives you a set of general tools to make this far easier. By combining these tools together, very sophisticated models are possible. Importantly, these tools abstract away the details of the mathematics; it is possible to implement many graphical models without even solving a single equation. This makes experimenting with models easier, but more importantly, it is also easier to verify that the implementation of the model is correct. The resulting code is also fast and can be used for production purposes if so required.

Please note, however, that this package currently serves mainly as a development testbed for a more complete version in the future. As such, as the project matures many things are likely to change, including the 'big picture' architecture of the package itself.

### How to Use this Library

The interface builds on the interface developed in Distributions.jl. The general procedure for building a mixture model consists on defining two distributions: the component distribution (e.g. MvNormal) and the mixing distribution (e.g. Categorical). In the case of variational Bayes, these distributions must be wrapped together with a prior (e.g. NormalInverseWishart in the MvNormal case, and Dirichlet in the Categorical case). A small set of functions must be defined over these custom types (such as calculating the log-pdf). The functions provided in this package then do the rest; applying iterative updates to find the optimal parameters.

To faciliate model-building, several commonly-used distributions have already been wrapped in this way.
