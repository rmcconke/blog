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

#### Forced HIT dataset
I ran this to see the folder structure:
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

As of right now (August 1 2025), the structure is a little inconsistent, but we get the general vibe. The key to understanding this structure, and what's available, is this table from the author's document:


<img src="/images/HIT_table.png"
     style="display: block; margin: 0 auto;"
     alt="Available data"/>

I'm going to download the whole dataset, but without the pre-combined time series files as I'd like to be able to work with the raw data myself, and merge it how I like.

```

```
