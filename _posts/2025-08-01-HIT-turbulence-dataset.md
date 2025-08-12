---
layout: post
title: A nice 3D homogeneous isotropic turbulence dataset
date: 2025-07-29 15:00:00-0500
description: 
tags: computational-fluid-dynamics direct-numerical-simulation turbulence dataset
toc:
  sidebar: left
background: '#FFFFFF' 
color: black
---

## Introduction
I recently came across a great [3D homogeneous isotropic turbulence (HIT) dataset](https://publications.rwth-aachen.de/record/981830) from RWTH Aachen. I really like this dataset, so I'm putting together this blog post to show how to access the data, and what it looks like. [This document](https://publications.rwth-aachen.de/record/981830/files/DataAccess-and-DataDescription_981830.pdf) from Ludovico Nista et al. contains more details; the point of this post is to be a friendly introduction to this dataset.

## Accessing the dataset
You need AWS CLI to access the dataset. Here's how I installed AWS CLI on Linux, based on the [install instructions](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
There are two datasets: forced and decaying 3D homogeneous isotropic turbulence. Let's set up the profiles for both:

#### Forced 3D HIT profile

Run:
```
aws configure --profile subboxes-forced
```
When prompted for the AWS Access Key ID, enter

```
read_a9ba76c4-2bd9-4f96-9758-0999a0fe87a4
```
When prompted for the secret access key, enter
```
CvTGQJ5pbCgtQJ6zbbP6TBSYBtqM1lCw
```
Leave default region name and default output format as empty.

#### Decaying 3D HIT profile
Run:
```
aws configure --profile subboxes-decaying
```
When prompted for the AWS Access Key ID, enter

```
read_a4d61e08-ba64-4bc8-b790-d6cf5ca6ebd9
```
When prompted for the secret access key, enter
```
jsLa4DjRKn7YkJmItx4qJvJR7I7gB3ks
```
Leave default region name and default output format as empty.

### Looking around the dataset
As of right now (August 1 2025), the structure is a little inconsistent, but we get the general vibe. The key to understanding this structure, and what's available, is this table from the author's document:


<img src="/images/HIT_table.png"
     style="display: block; margin: 0 auto;"
     alt="Available data"/>

#### Forced HIT dataset
I ran this to see the folder structure, without the `.h5` data files:
```
aws s3 ls s3://a9ba76c4-2bd9-4f96-9758-0999a0fe87a4/   --endpoint-url=https://coscine-s3-01.s3.fds.rwth-aachen.de:9021   --profile subboxes-forced --recursive |   awk '{print $4}' | sed 's|^|./|' | tree --fromfile -I "*.h5"
```
And we can see:
```
.
└── .
    ├── Delta2
    │   ├── Boxes32
    │   │   ├── CombineSnapshots.py
    │   │   ├── DNS
    │   │   │   ├── stage_reduced_1
    │   │   │   ├── TEST
    │   │   │   └── VALID
    │   │   └── LES
    │   │       ├── stage_reduced_1
    │   │       ├── TEST
    │   │       └── VALID
    │   ├── Boxes_Gaussian32
    │   │   ├── CombineSnapshots.py
    │   │   ├── DNS
    │   │   │   ├── stage_reduced_1
    │   │   │   ├── TEST
    │   │   │   └── VALID
    │   │   └── LES
    │   │       ├── stage_reduced_1
    │   │       ├── TEST
    │   │       └── VALID
    │   ├── Boxes_Spectral32
    │   │   ├── CombineSnapshots.py
    │   │   ├── DNS
    │   │   │   ├── stage_reduced_1
    │   │   │   ├── stage_reduced_2
    │   │   │   └── VALID
    │   │   └── LES
    │   │       ├── stage_reduced_1
    │   │       ├── stage_reduced_2
    │   │       └── VALID
    │   └── MakeBoxes_HIT.py
    ├── Delta4
    │   ├── Box
    │   │   ├── Boxes16
    │   │   │   ├── CombineSnapshots.py
    │   │   │   ├── DNS
    │   │   │   │   ├── stage_reduced_1
    │   │   │   │   ├── TEST
    │   │   │   │   └── VALID
    │   │   │   └── LES
    │   │   │       ├── stage_reduced_1
    │   │   │       ├── TEST
    │   │   │       └── VALID
    │   │   └── MakeBoxes_HIT.py
    │   ├── Gaussian
    │   │   ├── CombineSnapshots.py
    │   │   ├── DNS
    │   │   │   ├── stage_1
    │   │   │   ├── stage_reduced_1
    │   │   │   ├── stage_reduced_2
    │   │   │   ├── stage_reduced_3
    │   │   │   ├── TEST
    │   │   │   └── VALID
    │   │   └── LES
    │   │       ├── stage_1
    │   │       ├── stage_reduced_1
    │   │       ├── stage_reduced_2
    │   │       ├── stage_reduced_3
    │   │       ├── TEST
    │   │       └── VALID
    │   └── Spectral
    │       └── Boxes16
    │           ├── CombineSnapshots.py
    │           ├── DNS
    │           │   ├── stage_reduced_1
    │           │   ├── TEST
    │           │   └── VALID
    │           └── LES
    │               ├── stage_reduced_1
    │               ├── TEST
    │               └── VALID
    └── Delta8
        ├── Boxes8
        │   ├── CombineSnapshots.py
        │   ├── DNS
        │   │   ├── stage_reduced_1
        │   │   ├── TEST
        │   │   └── VALID
        │   └── LES
        │       ├── stage_reduced_1
        │       ├── TEST
        │       └── VALID
        └── MakeBoxes_HIT.py
```
#### Decaying HIT dataset
Here's the equivalent command for the decaying HIT dataset:
```
aws s3 ls s3://a4d61e08-ba64-4bc8-b790-d6cf5ca6ebd9/   --endpoint-url=https://coscine-s3-01.s3.fds.rwth-aachen.de:9021   --profile subboxes-decaying --recursive |   awk '{print $4}' | sed 's|^|./|' | tree --fromfile -I "*.h5"
```

```
.
└── .
    └── Delta8
        ├── Box
        │   ├── Boxes8
        │   │   ├── CombineSnapshots.py
        │   │   ├── DNS
        │   │   │   ├── stage_reduced_1
        │   │   │   └── VALID
        │   │   └── LES
        │   │       ├── stage_reduced_1
        │   │       └── VALID
        │   └── MakeBoxes_HIT.py
        ├── Gaussian
        │   ├── Boxes8
        │   │   ├── CombineSnapshots.py
        │   │   ├── DNS
        │   │   │   ├── stage_reduced_1
        │   │   │   └── VALID
        │   │   └── LES
        │   │       ├── stage_reduced_1
        │   │       └── VALID
        │   └── MakeBoxes_HIT.py
        └── Spectral
            ├── Boxes8
            │   ├── CombineSnapshots.py
            │   ├── DNS
            │   │   ├── stage_reduced_1
            │   │   └── VALID
            │   └── LES
            │       ├── stage_reduced_1
            │       └── VALID
            └── MakeBoxes_HIT.py

```


## Downloading the dataset
I'm going to download the dataset without the pre-combined time series files as I'd like to be able to work with the raw data myself, and merge it how I like.


Here are some useful commands: 
1. Download the entire dataset
- Forced HIT (**300 GB download**)
```
aws s3 sync s3://a9ba76c4-2bd9-4f96-9758-0999a0fe87a4/ ./forced_hit_complete/ --endpoint-url=https://coscine-s3-01.s3.fds.rwth-aachen.de:9021 --profile subboxes-forced --exclude "stage_reduced_*/*"
```
  - Decaying HIT (**100 GB Download**)
```
aws s3 sync s3://a4d61e08-ba64-4bc8-b790-d6cf5ca6ebd9/ ./decaying_hit_complete/ --endpoint-url=https://coscine-s3-01.s3.fds.rwth-aachen.de:9021 --profile subboxes-decaying --exclude "stage_reduced_*/*"
```

2. Download just the FS8 Box filtered data
- Forced HIT (**XXX download**)
```
```

- Decaying HIT (**XXX download**)
```
```

## Visualizing the dataset
All code for visualization is availalble in the [git repo](https://github.com/rmcconke/HIT3D_dataset) for this blog post. 

I'm going to visualize the DNS data that comes with the box filtered data, for the FS8 task.

Let's look at the velocity magnitude for all 64 boxes from the forced HIT DNS dataset.
<div style="max-width: 600px; margin: 0 auto;">
    <img src="/images/grid_of_boxes_forced_boxfilter_fs8_highres.gif"
         style="width: 100%; height: auto; display: block;"
         alt="Available data"/>
    <img src="/images/colourbar_forced_boxfilter_fs8_highres.png"
         style="width: 100%; height: auto; display: block;"
         alt="Available data"/>
</div>
We can see that generally, the velocity magnitude remains the same throughout the dataset. It is, in fact, *forced* HIT. That's kind of the point!


Similarly, here are all 32 boxes from the decaying HIT dataset:

<div style="max-width: 600px; margin: 0 auto;">
    <img src="/images/grid_of_boxes_decaying_boxfilter_fs8_highres.gif"
         style="width: 100%; height: auto; display: block;"
         alt="Available data"/>
    <img src="/images/colourbar_decaying_boxfilter_fs8_highres.png"
         style="width: 100%; height: auto; display: block;"
         alt="Available data"/>
</div>

The velocity magnitude decays, because this is a *decaying* HIT dataset. 

The $Q$ criterion is used to visualize vortex structures in a turbulent flow. It's calculated by

$$
Q = \frac{1}{2}\left(||R||^2-||S||^2 -\right) \ ,
$$



where $||\cdot||^2$ is the [Frobenius norm](https://mathworld.wolfram.com/FrobeniusNorm.html) of a tensor. To take the Frobenius norm, you just individually square each element, and then sum them up. More information about the $Q$-criterion can be found in these references:
- https://www.cambridge.org/core/journals/journal-of-fluid-mechanics/article/on-the-relationships-between-local-vortex-identification-schemes/E1DE98BE2DACF2A1724F62322C7FAFEB
- https://pubs.aip.org/aip/pof/article/31/12/121701/957670/Comparison-between-the-Q-criterion-and-Rortex-in

$Q$-criterion measures excess strain to rotation rate. When $Q>0$, it indicates


Box 0
U magnitude
Q criterion
Slices of means/std, animated/moving through domain



## Computing the turbulent kinetic energy spectrum
There are different ways and formulas to compute the turbulent kinetic energy spectrum. 
