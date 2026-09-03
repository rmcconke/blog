---
layout: page
---

# Research

I build machine learning methods for fluid mechanics and turbulence. Most of what I do sits at the point where a machine learning model interacts with a numerical solver.

---

## Turbulence closure modelling for RANS

RANS is the workhorse of industrial CFD, and its primary weakness is the closure model. In my PhD, I developed a framework for injecting anisotropy predictions into RANS simulations in a way that is stable and well-conditioned.

<img src="/images/turb_modelling_rans.png" alt="Modified TBNN architecture which promotes stability, conditioning, and realizability" style="width: 100%; max-width: 560px; height: auto;">

*Modified TBNN architecture which promotes stability, conditioning, and realizability. From [Realisability-informed machine learning for turbulence anisotropy mappings](https://www.cambridge.org/core/journals/journal-of-fluid-mechanics/article/realisabilityinformed-machine-learning-for-turbulence-anisotropy-mappings/0FADE317B80F7962EA990D1D9EA3C24A), Journal of Fluid Mechanics 1019, A49 (2025).*

I made three primary contributions to data-driven turbulence modelling during my PhD.

1. An orthogonal decomposition of the anisotropy tensor used as an inductive bias in the network architecture, together with a non-negativity constraint on the optimal eddy viscosity that promotes numerical stability ([Physics of Fluids, 2022](https://doi.org/10.1063/5.0083074)).

2. A systematic study of generalization. While they transfer well to new parametric variations of a flow they have already seen, they transfer poorly to genuinely new flow types ([IJCFD, 2022](https://arxiv.org/abs/2206.05226)). This is a pessimistic result :( However, I think it is an important one, because it suggests these methods are better used to build specialized models for a given flow class than to chase a universal closure. It's related to the no free lunch theorem. It's effectively a new way to sensitize general RANS models to specific flows using domain-specific data.

3. A physics-based loss function that penalizes non-realizable predictions during training instead of correcting them afterwards, inside a formulation that keeps eddy-viscosity conditioning while embedding equivariance ([Journal of Fluid Mechanics, 2025](https://www.cambridge.org/core/journals/journal-of-fluid-mechanics/article/realisabilityinformed-machine-learning-for-turbulence-anisotropy-mappings/0FADE317B80F7962EA990D1D9EA3C24A)).

<img src="/images/realizability_informed.png" alt="Realizability-informed predictions of the Reynolds stress anisotropy tensor" style="width: 100%; max-width: 560px; height: auto;">

*Non-realizable predictions are penalized during training rather than corrected afterwards. From [Realisability-informed machine learning for turbulence anisotropy mappings](https://www.cambridge.org/core/journals/journal-of-fluid-mechanics/article/realisabilityinformed-machine-learning-for-turbulence-anisotropy-mappings/0FADE317B80F7962EA990D1D9EA3C24A), Journal of Fluid Mechanics 1019, A49 (2025).*

I tried these techniques on an industrial dataset, and they worked! I trained a corrective closure on LES data for architectural/wind engineering flows at [RWDI](https://rwdi.com/). We were able to achieve accuracy comparable to LES at only 1.5 to 2 times the cost of the baseline RANS simulation. For steady-state RANS flows, I believe the anisotropy tensor decomposition and injection technique I developed during my PhD is industrially viable.

Another line of work I pursued during my PhD was calibration of existing turbulence models using data. I developed **turbo-RANS** (yes, we love the name), a black-box Bayesian optimization procedure for turbulence model coefficients. I love the vibes of Bayesian optimization. It's possible to use turbo-RANS with integral (e.g. lift coefficient), sparse (e.g., probe measurements), or dense (e.g., field measurements) reference data ([IJNMHFF, 2024](https://www.emerald.com/hff/article-abstract/34/8/2986/1234503/Turbo-RANS-straightforward-and-efficient-Bayesian?redirectedFrom=fulltext)). Nikhila Kalia and I applied turbo-RANS to the generalized $k$-$\omega$ ([GEKO](https://arc.aiaa.org/doi/10.2514/1.J065393)) model in our [preprint](https://arxiv.org/abs/2502.11218). I was surprised at how good the results from two equation models can be when you calibrate a few of their global coefficients! The plot below shows the GEKO model, but we see similar agreement with the good old fashioned $k$-$\omega$ model too.

<img src="/images/geko_turbo_rans.png" alt="Comparison of mean streamwise velocity vertical profiles in a converging-diverging channel" style="width: 100%; max-width: 560px; height: auto;">

*Comparison of mean streamwise velocity vertical profiles at different x-locations in a converging-diverging channel at Re = 12,600. From our [preprint](https://arxiv.org/abs/2502.11218).*

## Benchmarking and evaluation

Machine learning for turbulence modelling has produced about a decade of papers, but without a public benchmark! Because every study selects its own evaluation flows, incremental progress is impossible to measure. Standardized/public benchmarks have been so important in many domains of machine learning, like computer vision.

**The Closure Challenge** came out of a conversation I had with Paola Cinnella and Richard Dwight at a conference, where I raised the missing benchmark as the thing holding the field back. Paola brought it under [ERCOFTAC Special Interest Group 54](https://www.ercoftac.org/special_interest_groups/54-machine-learning-for-fluid-dynamics/new-sig54-news-and-events-/), where it now runs continuously as a field-wide benchmark. Test cases include periodic hills, a square duct, and the NASA wall-mounted hump, chosen to probe generalization across Reynolds number and geometry. It is a deliberately difficult benchmark.

Tyler Buchanan and I developed it as equal contributors. I built the evaluation and scoring package, the metrics, and the submission process, and Tyler contributed much of the data and organization. Together we provide a huge amount of sample training data in OpenFOAM format on the [Closure Challenge github page](https://github.com/rmcconke/closure-challenge-benchmark). I serve as benchmark steward, which means I evaluate every submission and maintain the leaderboard. Six international research groups are on the leaderboard so far. Details are on the [software page](/software/#the-closure-challenge) and in our [preprint](https://arxiv.org/abs/2603.28884).

## Subgrid-scale modelling for LES

LES resolves the large scales and models the small ones, which makes the subgrid-scale closure the natural target for a learned model. My current work asks how general purpose machine learning architectures and training techniques can be adapted for this domain-specific task.

<img src="/images/LES_sgs.png" alt="Subgrid-scale stress prediction for large eddy simulation" style="width: 100%; max-width: 560px; height: auto;">

*Learning the subgrid-scale stress, where the question is how much spatial context the model needs.*

How much spatial context does a subgrid scale model need? A classical SGS model is purely local (AKA pointwise). A convolutional network is not: it has a kernel which passes over nearby points. Locality is cheaper, easier to parallelize, and easier to reason about, but it is not obvious how much accuracy it costs in the context of SGS modelling.

Also, is rotational equivariance worth its computational cost in this setting? While it is a symmetry of the physics, it is an expensive and often unwieldy architectural constraint. In our [preprint](https://arxiv.org/abs/2607.26850), we study the data efficiency and generalization of equivariant and non-equivariant SGS models.

## Equivariance and symmetry

Since I first learned what equivariance was while studying predictive models for tensors during my PhD, I've been interested in how to embed it into a machine learning architecture. It's why I joined Tess Smidt's lab for my postdoc.

One result from my postdoc I did not expect was that superresolution models trained on turbulence learn a substantial amount of rotational equivariance on their own, with no augmentation, purely because the data is rotationally rich. Kolmogorov's hypothesis of small-scale isotropy predicts the effect should be stronger at smaller scales, and it is ([arXiv, 2026](https://arxiv.org/abs/2602.04695)). We showed this first for 2D forced turbulence, then extended it to 3D homogeneous isotropic turbulence and turbulent channel flow ([NeurIPS ML4PS, 2025](https://arxiv.org/abs/2509.20683)).

<img src="/images/equivariance.png" alt="Test set equivariance error fields for near-wall and mid-plane channel flow data" style="width: 100%; max-width: 560px; height: auto;">

*Test-set equivariance error for a CNN trained on 1500 examples from a single box with no explicit data augmentation, for (a) near-wall and (b) mid-plane data. The transformation is a 90 degree rotation about the z axis, which leaves the z velocity component itself unchanged, so the transformed field appears as a plain rotation. Hatching indicates the direction of the top wall, not the wall boundary. The model trained on the more isotropic mid-plane data has a substantially lower equivariance error. From [Turbulence teaches equivariance to neural networks](https://arxiv.org/abs/2602.04695).*

Clearly, there are domain-specific effects on the training dynamics of these models that need to be studied in the context of turbulence.

## Ocean fluid dynamics

<img src="/images/ocean.png" alt="Ocean turbulence simulation" style="width: 100%; max-width: 560px; height: auto;">

*Ocean turbulence from the Oceananigans simulation dataset underlying this work.*

I lead the collaboration between Tess Smidt's equivariance group and Abigail Bodner's ocean modelling group at MIT, since I'm somewhat fluent in both fluids and machine learning languages. The work targets subgrid-scale and stochastic parameterization for ocean turbulence, including quasi-geostrophic dynamics and submesoscale closures. On the infrastructure side, I'm working on the machine learning data pipeline for a 500 TB Oceananigans dataset that arrived fragmented into roughly a thousand pieces per timestep. The package handles chunking and format conversion and launches and tunes its own SLURM jobs.

## Constitutive modelling for atmospheric re-entry

I lead the machine learning effort on constitutive modelling within [**CHEFSI**](https://chefsi.mit.edu/), the MIT Center for the Exascale Simulation of Coupled High-Enthalpy Fluid-Solid Interactions. It was selected in September 2025 as a DOE/NNSA PSAAP-IV Predictive Simulation Center. The problem we're focused on is learning material response for thermal protection systems under atmospheric re-entry conditions, where the fluid and solid are strongly coupled and the material is changing as it ablates. I direct a team of four to five graduate students, run weekly meetings, and set technical directions. What's a fluids person doing in ML for solid mechanics? Well, it turns out a lot of the problems are similar in these two domains. Constitutive modelling for continuum mechanics translates well.

---
# Publications

See also [Google Scholar](https://scholar.google.com/citations?user=GcAsX_EAAAAJ&hl=en).

## Preprints and manuscripts under review

4. **R. McConkey**, J. Balla, E. Hofgard, T. Smidt, A. Bodner, "Rotational equivariance and locality in data-driven subgrid-scale closures," *arXiv*:2607.26850 (2026). Under review, *Computer Methods in Applied Mechanics and Engineering*. [arXiv](https://arxiv.org/abs/2607.26850)

3. **R. McConkey**, T. Buchanan, T. Smidt, A. Bodner, R. Dwight, P. Cinnella, "The Closure Challenge: a benchmark task for machine learning in turbulence modelling," *arXiv*:2603.28884 (2026). Under review, NeurIPS. [arXiv](https://arxiv.org/abs/2603.28884)

2. **R. McConkey**, J. Balla, J. Bailey, A. Backour, E. Hofgard, T. Jaakkola, A. Bodner, T. Smidt, "Turbulence teaches equivariance to neural networks," *arXiv*:2602.04695 (2026). Under review, *Physical Review Fluids*. [arXiv](https://arxiv.org/abs/2602.04695)

1. N. Kalia, **R. McConkey**, E. Yee, F. S. Lien, "Bayesian optimization of the GEKO turbulence model for predicting flow separation over a smooth surface," *arXiv*:2502.11218 (2025). [arXiv](https://arxiv.org/abs/2502.11218)

## Peer-reviewed journal articles

9. **R. McConkey**, N. Kalia, E. Yee, F. S. Lien, "Realisability-informed machine learning for turbulence anisotropy mappings," *Journal of Fluid Mechanics* **1019**, A49 (2025). [Journal](https://www.cambridge.org/core/journals/journal-of-fluid-mechanics/article/realisabilityinformed-machine-learning-for-turbulence-anisotropy-mappings/0FADE317B80F7962EA990D1D9EA3C24A) · [arXiv](https://arxiv.org/abs/2406.11603)

8. N. Kalia, **R. McConkey**, E. Yee, F. S. Lien, "Kolmogorov-Arnold networks for turbulence anisotropy mapping," *Physics of Fluids* **37**, 085140 (2025). [arXiv](https://arxiv.org/abs/2505.19366)

7. **R. McConkey**, N. Kalia, E. Yee, F. S. Lien, "Turbo-RANS: straightforward and efficient Bayesian optimization of turbulence model coefficients," *International Journal of Numerical Methods for Heat and Fluid Flow* **34**(8), 2986-3016 (2024). [Journal](https://www.emerald.com/hff/article-abstract/34/8/2986/1234503/Turbo-RANS-straightforward-and-efficient-Bayesian?redirectedFrom=fulltext)

6. **R. McConkey**, E. Yee, F. S. Lien, "On the generalizability of machine-learning-assisted anisotropy mappings for predictive turbulence modelling," *International Journal of Computational Fluid Dynamics* **36**(7), 555-577 (2022). [arXiv](https://arxiv.org/abs/2206.05226)

5. Z. Cheng, **R. McConkey**, E. Yee, F. S. Lien, "Numerical investigation of noise suppression and amplification in forced oscillations of single and tandem cylinders in high Reynolds number turbulent flows," *Applied Mathematical Modelling* **117**, 652-686 (2023).

4. M. Cann, **R. McConkey**, F. S. Lien, W. Melek, E. Yee, "A data-driven approach for generating vortex-shedding regime maps for an oscillating cylinder," *Energies* **16**(11), 4440 (2023).

3. **R. McConkey**, E. Yee, F. S. Lien, "Deep structured neural networks for turbulence closure modelling," *Physics of Fluids* **34**, 035110 (2022). [Journal](https://doi.org/10.1063/5.0083074) · [arXiv](https://arxiv.org/abs/2201.01710)

2. Y. Wu, Z. Cheng, **R. McConkey**, F. S. Lien, E. Yee, "Modelling of flow-induced vibration of bluff bodies: a comprehensive survey and future prospects," *Energies* **15**(22), 8719 (2022).

1. **R. McConkey**, E. Yee, F. S. Lien, "A curated dataset for data-driven turbulence modelling," *Nature Scientific Data* **8**, 255 (2021). [Journal](https://doi.org/10.1038/s41597-021-01034-2) · [arXiv](https://arxiv.org/abs/2103.11515) · [Dataset](https://doi.org/10.34740/kaggle/dsv/2637500)

## Invited talks

5. "Machine learning for turbulence modelling," Community Seminar, Center for Computational Science and Engineering, MIT (2026).

4. "Accelerating fluid simulations with machine learning," Black Hole Initiative, Harvard University (2025).

3. "Rotational equivariance as an inductive bias in machine learning for fluids," Data Science and Artificial Intelligence Seminar Series, Chalmers University of Technology (2025). [Slides](/docs/2025.04.14%20Talk%20at%20Chalmers%20University%20of%20Technology.pdf)

2. "Turbulence modelling using machine learning: key challenges and recent progress," Modelling and Simulation Seminar Series, University of Manchester (2022).

1. "Hands-on data-driven turbulence modelling with PyTorch," Modelling and Simulation Seminar Series, University of Manchester (2022).

## Conference papers, posters, and presentations

13. J. Balla, J. Bailey, A. Backour, E. Hofgard, T. Jaakkola, T. Smidt, **R. McConkey** (presenter), "Implicit augmentation from distributional symmetry in turbulence super-resolution," Machine Learning and the Physical Sciences, NeurIPS, San Diego, USA (2025). [Paper](https://arxiv.org/abs/2509.20683)

12. **R. McConkey** (presenter), J. Balla, E. Hofgard, T. Smidt, "Equivariant machine learning of sub-grid scale closure models for large eddy simulation," APS Division of Fluid Dynamics Annual Meeting, Houston, USA (2025). [Poster](/docs/2025.11.23_McConkey_Ryley_APS_DFD_Interact.pdf)

11. **R. McConkey** (presenter), S. Peng, S. Snider, S. Silvestri, T. Smidt, A. Bodner, "Multi-scale ocean turbulence with Euclidean neural networks," Gordon Research Conference: Machine Learning for Actionable Climate Science, Rhode Island, USA (2025).

10. **R. McConkey** (presenter), A. Backour, J. Balla, E. Hofgard, J. Nigam, T. Smidt, "The role of local rotational symmetries and equivariance in data-driven fluid mechanics," 33rd Annual Conference of the CFD Society of Canada, Montreal, Canada (2025).

9. **R. McConkey** (presenter), A. Backour, J. Balla, E. Hofgard, J. Nigam, T. Smidt, "On rotational equivariance as an inductive bias in machine learning for fluids," ERCOFTAC Workshop on Data-Driven Fluid Mechanics, London, UK (2025). [Slides](/docs/2025.04.02%20ERCOFTAC%20Workshop%20Presentation.pdf)

8. F. S. Lien, E. Yee, D. Orchard, C. Li, Z. Cheng, Y. Wu, H. H. Huang, **R. McConkey** (presenter), J. Wang, L. H. Chen, J. Yi, N. Kalia, "Development of a simulation environment for the assessment of urban air mobility vehicles: energy efficiency, noise, and icing," Sustainable Aeronautics Summit, Waterloo Institute for Sustainable Aeronautics, Canada (2023).

7. **R. McConkey** (presenter), A. Mole, A. Skillen, A. Revell, E. Yee, F. S. Lien, "Machine learning augmented turbulence modelling for massively separated three-dimensional flows," Computational Fluids Conference, Cannes, France (2023).

6. **R. McConkey**, A. Mole (presenter), A. Skillen, A. Revell, E. Yee, F. S. Lien, "XGBoost-augmented RANS closure modelling of complex 3D flows," Workshop on Data-Driven Methods for Fluid Mechanics, Leeds Institute for Fluid Dynamics, Leeds, UK (2023).

5. **R. McConkey**, E. Yee, F. S. Lien, "Deep learning-based turbulence closure with improved optimal eddy viscosity prediction," Proceedings of the 29th Annual Conference of the CFD Society of Canada (2021). **Best Student Paper.**

4. M. Cann, **R. McConkey**, F. S. Lien, W. Melek, E. Yee, "Mode classification for vortex shedding from an oscillating wind turbine using machine learning," *Journal of Physics: Conference Series* **2141**(1), 012009 (2021).

3. **R. McConkey**, L. Long, A. Komrakova, J. G. Wong, "Evaluation of RANS turbulence models and boundary conditions for CFD simulations of a finite wing in ground effect," Okanagan Fluid Dynamics Meeting, Canmore, Canada (2019).

2. **R. McConkey** (presenter), L. Long, A. Komrakova, J. G. Wong, "3D simulations of a finite wing in ground effect," Undergraduate Research Symposium, University of Alberta (2019).

1. **R. McConkey** (presenter), L. Long, A. Komrakova, J. G. Wong, "Simulations of a 2D NACA 0012 airfoil in ground effect," Undergraduate Research Symposium, University of Alberta (2018).

## Patent applications

5. M. Curial, C. Terriff, W. Comeau, W. Comeau, **R. McConkey**, "Devices, systems and methods for medicament delivery," WIPO No. WO2023077225A1 (2022).

4. M. Curial, C. Terriff, W. Comeau, W. Comeau, **R. McConkey**, "Devices, systems and methods for medicament delivery," WIPO No. WO2021087607A1 (2020).

3. M. Curial, C. Terriff, W. Comeau, **R. McConkey**, "Devices, systems and methods for medicament delivery," U.S. Patent No. US-20220370718-A1 (2022).

2. M. Curial, C. Terriff, W. Comeau, **R. McConkey**, "Portable negative pressure isolation unit," U.S. Patent No. US-20220104982-A1 (2022).

1. M. Curial, C. Terriff, W. Comeau, B. Koravankudi, W. Comeau, **R. McConkey**, "IMSAFE: a novel large-volume intramuscular autoinjector," WIPO PCT No. PCT/CA2022/050057 (2022).

## Peer review

I also serve as a reviewer for journals and funding agencies. See my [CV](/docs/McConkey_Ryley_CV.pdf) for the full list.