---
layout: page
---

<img alt="Ryley McConkey" src="/images/prof_pic.jpg"
    style="float: right; max-width: 33%; margin: 0 0 1em 2em; border-radius: 50%">


# Ryley McConkey
PhD, P.Eng.

[Email](mailto:rmcconke@mit.edu) / [Github](https://github.com/rmcconke) / [Google Scholar](https://scholar.google.com/citations?user=GcAsX_EAAAAJ&hl=en) / [LinkedIn](https://www.linkedin.com/in/ryley-mcconkey/) / [ORCID](https://orcid.org/0000-0003-0674-1849)


I'm a postdoc working with the [Atomic Architects](https://atomicarchitects.com/) and [Multiscale Mariners](https://multiscalemariners.com/) research groups at MIT. I build machine learning methods for fluid mechanics and turbulence, and I work on applying them to practical problems in engineering. My current work includes subgrid-scale modelling for LES, closure modelling and benchmarking for RANS, equivariant network architectures, and applications in ocean modelling and atmospheric re-entry. I love fluid mechanics, and computational fluid dynamics (CFD)! Check out my [YouTube playlist](https://youtube.com/playlist?list=PLuV-XJJZrRRdv2KTVYH8mer53Q9pl31mK&si=dF3x-q-c1wbmNGwZ), [lectures](https://youtube.com/playlist?list=PLuV-XJJZrRRdR_fZkK2JFPJcnh6oagg20&si=8jwyXrmZDP60qn3p), and [blog posts](/writing/).

Alongside my PhD, I spent five years doing engineering design, simulation, and research in industry. I designed medical devices like autoinjectors and aerosol containment devices at [MACH 32](https://www.mach32.net/), automated CFD workflows at [Orbital Stack](https://orbitalstack.com/), and developed machine learning models for wind engineering at [RWDI](https://rwdi.com/en_ca/). I've been a licensed Professional Engineer in Ontario since 2024.

<img src="/images/ezgif.com-gif-maker_best_0.gif" alt="vortex shedding from a cube" style="width: 500px; height: auto;">

## Selected work

- **[The Closure Challenge](/software/#the-closure-challenge)**, a continuously running, field-wide benchmark for machine learning in RANS turbulence modelling, hosted under ERCOFTAC SIG 54. A decade of work in this area had produced no shared evaluation, so every study picked its own test flows. I started the benchmark and serve as benchmark steward. We have six international groups on the leaderboard so far. Check it out if you're interested in ML for RANS! [Preprint](https://arxiv.org/abs/2603.28884) / [Github](https://github.com/rmcconke/closure-challenge-benchmark)

- **[Realizability-informed machine learning](/research/#turbulence-closure-modelling-for-rans)**, an equivariant model formulation for predicting the Reynolds stress anisotropy tensor in a realizable way. I replaced ad-hoc postprocessing of predicted anisotropy tensors with a physics-based loss that penalises non-realizable predictions during training, inside a framework that keeps eddy-viscosity conditioning. [JFM Paper](https://www.cambridge.org/core/journals/journal-of-fluid-mechanics/article/realisabilityinformed-machine-learning-for-turbulence-anisotropy-mappings/0FADE317B80F7962EA990D1D9EA3C24A)

- **[A curated dataset for data-driven turbulence modelling](/software/#a-curated-dataset-for-data-driven-turbulence-modelling)**, the first open-source dataset built for immediate use in ML-augmented closure modelling, with collocated RANS and high-fidelity data for the same flows. 895,640 data points, five flow families, four turbulence models. [Scientific Data Paper](https://doi.org/10.1038/s41597-021-01034-2) / [Kaggle](https://doi.org/10.34740/kaggle/dsv/2637500)


- **Machine learning for constitutive modelling in CHEFSI**, a DOE/NNSA PSAAP-IV Predictive Simulation Center at MIT. I lead the machine learning team (4-5 graduate students) on learning material response for thermal protection systems under atmospheric re-entry conditions. [CHEFSI website](https://chefsi.mit.edu/)

## News
- **September 2026**: I'm on the job market this cycle, for both faculty positions and industry research roles. Please get in touch :)

- **July 2026**: We have a new preprint out on rotational equivariance and locality in data-driven subgrid-scale closures for LES. Check it out on [arXiv](https://arxiv.org/abs/2607.26850).

- **April 2026**: The Closure Challenge benchmark is fully live on [github](https://github.com/rmcconke/closure-challenge-benchmark). We also put a [preprint on arXiv](https://arxiv.org/abs/2603.28884) discussing this challenge, but the github page is the main source of up-to-date information for the benchmark. Ongoing submissions are encouraged!

- **February 2026**: We have a preprint out on how the rotational nature of turbulence teaches rotational equivariance to neural networks. This is a continuation of our work presented at NeurIPS ML for Physical Sciences. Check it out on [arXiv](https://arxiv.org/abs/2602.04695).

- **December 2025**: I'll be at the NeurIPS ML for Physical Sciences workshop. We're presenting a poster based on our accepted [workshop paper](https://arxiv.org/abs/2509.20683) on distributional symmetry in turbulence. 

- **November 2025**: I presented our [abstract](https://scholar.google.com/citations?view_op=view_citation&hl=en&user=GcAsX_EAAAAJ&sortby=pubdate&citation_for_view=GcAsX_EAAAAJ:UebtZRa9Y70C) on equivariance for subgrid scale closure modelling at an Interact session at the APS DFD 2025 meeting ([poster](/docs/2025.11.23_McConkey_Ryley_APS_DFD_Interact.pdf)).

- **November 2025**: Tyler Buchanan, Richard Dwight, Paola Cinnella, and I are putting together a continuously running, field-wide benchmark for RANS turbulence modelling. It's time we had a standardized benchmark for machine learning in RANS! The data and evaluation package is now public. See the description [here](https://github.com/rmcconke/closure-challenge-benchmark). It's being advertised as part of the 2026 ERCOFTAC ML for Fluids Workshop ([link](https://ml4fluids2026.github.io/challenges/)), but it will run beyond the conference. 
  
- **September 2025**: We have a [preprint out](https://arxiv.org/abs/2509.20683) on distributional symmetry in turbulence, and how superresolution models can learn equivariance just from the rotational nature of turbulence data.

- **April 2025**: I presented at the Chalmers University of Technology Data Science and Artificial Intelligence Seminar Series ([slides](/docs/2025.04.14%20Talk%20at%20Chalmers%20University%20of%20Technology.pdf)). I also went to IKEA in Sweden. What else?
  
- **March 2025**: I presented at the ERCOFTAC ML for Fluids Workshop in London ([slides](/docs/2025.04.02%20ERCOFTAC%20Workshop%20Presentation.pdf)). It was a great workshop, and I enjoyed my time in London! 




## About me


I graduated from the University of Alberta in 2019 with a Bachelor of Science in Mechanical Engineering (co-op). Pursuing an interest in turbulence and computational fluid dynamics (CFD), I then began a Master's degree at the University of Waterloo. In my Master's research, I was focused on simulating a new type of wind turbine which uses vortex induced vibration (VIV) to generate energy. Then, I direct transferred to a PhD in 2020. My PhD was focused on developing new turbulence models using machine learning. I completed a 6 month visit at the University of Manchester in 2022-2023, where I focused on data-driven turbulence modelling on complex 3D flows. After completing my PhD in 2024, I started a Postdoc at MIT, funded by an NSERC Postdoctoral Fellowship.

My diverse experience includes mechanical design, software implementation, and industrial research and development. At [MACH32](https://www.mach32.net/), a medical device startup company, I was the sole simulation engineer and an inventor on three devices. I designed and simulated novel autoinjectors, along with a portable negative-pressure isolation tent that went from an emergency physician describing the problem to a device on the market in about 40 days during the first year of the COVID-19 pandemic. I designed and ran the experimental validation campaign behind its published performance specification, and it was adopted by University Health Network in Toronto. I also worked on automating CFD simulations as a Software Developer at [Orbital Stack](https://orbitalstack.com/), a wind engineering startup company. In the Research and Development group (Labs) at [RWDI](https://rwdi.com/en_ca/), I developed and implemented machine learning based tools to augment simulations and wind tunnel experiments.

Outside work, I do landscape photography, play trombone in the MIT Concert Band, tutor at a Cambridge high school every week, and build hobby electronics.


## Personal records
 

| Record | |
|---|---|
| Largest simulation | ~600,000 CPU hours: 30M cells on 512 cores, one week per run, several runs |
| Largest dataset | 500 TB Ocean simulation on 1000 GPUs. Animation coming soon! |
| Largest model trained | > 30M parameters |
| Most GPUs at once | four H100s for a week, multi-node |
| Largest tabular training run | 56M rows across four A100s. XGBoost, of course |
| Deadlift | 365 lb × 5 |
| Squat | 315 lb × 5 |
| Bench press | 195 lb × 4 |
| Hot dogs consumed at a sporting event | 6 |

Here is a playlist with my favourite fluid mechanics videos:

<iframe width="560" height="315" src="https://www.youtube.com/embed/videoseries?si=bOF9VLedLKDqy2UO&amp;list=PLuV-XJJZrRRdv2KTVYH8mer53Q9pl31mK" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>