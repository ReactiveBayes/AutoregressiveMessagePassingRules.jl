# AutoregressiveMessagePassingRules

[![CI](https://github.com/ReactiveBayes/AutoregressiveMessagePassingRules.jl/actions/workflows/CI.yml/badge.svg?branch=main)](https://github.com/ReactiveBayes/AutoregressiveMessagePassingRules.jl/actions/workflows/CI.yml)
[![Docs: stable](https://img.shields.io/badge/docs-stable-blue.svg)](https://reactivebayes.github.io/AutoregressiveMessagePassingRules.jl/stable/)
[![Docs: dev](https://img.shields.io/badge/docs-dev-blue.svg)](https://reactivebayes.github.io/AutoregressiveMessagePassingRules.jl/dev/)
[![Coverage](https://codecov.io/gh/ReactiveBayes/AutoregressiveMessagePassingRules.jl/graph/badge.svg)](https://codecov.io/gh/ReactiveBayes/AutoregressiveMessagePassingRules.jl)
[![Code style: Runic](https://img.shields.io/badge/code_style-%E1%9A%B1%E1%9A%A2%E1%9A%BE%E1%9B%81%E1%9A%B2-black)](https://github.com/fredrikekre/Runic.jl)

The message passing rules of the autoregressive nodes, for Bayesian autoregressive processes of
order `p`, `yₜ ~ N(θᵀxₜ, γ⁻¹)` with `xₜ = (yₜ₋₁, …, yₜ₋ₚ)`, whose states, coefficients and
noise precision are inferred together:

- `AR` (alias `Autoregressive`): the coefficients `θ` and the precision `γ` on edges of their
  own;
- `ConjugateAR`: `(θ, γ)` joint on one normal-gamma edge `w`, for a conjugate parameter update.

Their rules are variational and run under `ARVMP(form, order, stype)`, which a model must give;
`stype` is `ARsafe()` or `ARunsafe()`.

```julia
using AutoregressiveMessagePassingRules, MessagePassingRulesBase, ExponentialFamily, BayesBase

result = @call_message_update_rule(
    node = AR, target = :y, algorithm = ARVMP(Univariate, 1, ARsafe()),
    m = (x = NormalMeanVariance(1.0, 1.0),),
    q = (θ = NormalMeanVariance(1.0, 1.0), γ = GammaShapeRate(1.0, 1.0)),
)
getresult(result)   # NormalMeanVariance(0.5, 1.5)
```

```julia
import Pkg; Pkg.add("AutoregressiveMessagePassingRules")
```

- Documentation: <https://reactivebayes.github.io/AutoregressiveMessagePassingRules.jl/stable/>.
  `make docs` builds it locally, into `docs/build`.
- Tests: `make test` runs the suite as CI does, ending with the rule-coverage gate: every rule
  must be selected by some test. `make test test_args="name:rules"` selects items by tag, by
  name or by file; `make help` lists the other targets.
- Julia 1.10 or later. MIT licence.
