Miguel Xochicale

# 

<div style="background: rgba(10,10,14,0.72);   backdrop-filter: blur(6px);   border: 1px solid rgba(255,255,255,0.08);   border-radius: 18px;   text-align:center;   padding: 1.1em 1.8em;   max-width: max-content;   margin-left: auto;   margin-right: auto;   line-height: 1.5em!important;   box-shadow: 0 20px 60px rgba(0,0,0,0.45);">

<span style="color:#f5f5f7; font-size:1.75em; text-align:center; display:block;">

<!-- TODO: replace with the real talk title -->

<span style="color:#ffffff; font-size:1.65em; display:block; font-weight:800;">Speed
vs Structure: Building Reproducible AI Workflows with the UCL Unified AI
Research Platform</span>

</span>

<span style="font-size:0.55em; color:#b7bdc9;">[**Miguel
Xochicale**](http://mxochicale.github.io/) ·
[UCL-ARC](https://www.ucl.ac.uk/advanced-research-computing/)</span>

<div class="badge-row">

<a href="https://github.com/mxochicale/rseconf26-walkthrough213"
class="badge">⭐ GitHub repo</a> <a
href="https://github.com/mxochicale/rseconf26-walkthrough213/blob/main/LICENSE"
class="badge">📄 License: Apache 2.0</a>
<a href="#TO_ADD_ZENODO_DOI" class="badge">🔗 DOI: 10.xxxx/xxxxx</a>

</div>

</div>

<div class="footer">

<span class="dim-text" style="text-align:left;">RSECon26, 9–11 September
2026, Sheffield, UK · [web-animations 2025 by
mxochicale](https://mxochicale.github.io/web-animations/)</span>

</div>

<div class="notes">

<!-- TODO: add opening speaker notes -->

</div>

<!-- ============================================================
     OVERVIEW
     ============================================================ -->

## Overview

<div class="columns">

<div class="column" width="50%">

### What We’ll Cover

<!-- TODO: link items 3-4 to real slide anchors once those sections exist,
     e.g. add `{#sectag_demos}` / `{#sectag_future}` to their section headers -->

1.  [**Introduction**](#sectag_title_1) - <add details>
2.  [**Rapid Prototyping Workflow**](#sectag_title_2) - <add details>
3.  [**Structured Production Workflow**](#sectag_title_3) -
    <add details>
4.  [**Conclusions, Future Work & Next Steps**](#sectag_title_4) -
    <add details>

</div>

<div class="column" width="50%">

### Key Themes

<!-- TODO: replace placeholder keywords -->

> [!NOTE]
>
> ### :cloud: Cloud keywords
>
> keyword1, keyword2, keyword3

> [!TIP]
>
> ### :robot: Robotics keywords
>
> keyword1, keyword2, keyword3

> [!IMPORTANT]
>
> ### :busts_in_silhouette: Collaborative keywords
>
> keyword1, keyword2, keyword3

</div>

</div>

<!-- ============================================================
     SECTION: Section title 1
     ============================================================ -->

# Introduction

**Containerising AI workflows**

<div class="notes">

<!-- TODO: notes specific to Section title 1 -->

Walk through the three layers: cloud VMs managed via Terraform/k8s, the
campus network, and physical hardware (sensors, robots).

</div>

<!-- *********************** NEW SLIDE *********************** -->

## Unified AI Platform for Research

Scalable, GPU-accelerated infrastructure enabling UCL researchers to
develop, train, evaluate, and deploy AI and machine learning models.

<div id="fig-template-section1">

<img src="figures/uai_platform.svg" data-fig-align="center" />

Figure 1: Unified AI Platform

</div>

<div style="font-size: 55%;">

An overview of Kubeflow Trainer:
<https://www.ucl.ac.uk/advanced-research-computing/platforms-services/unified-ai-platform-research/>

</div>

<div class="notes">

</div>

<!-- *********************** NEW SLIDE *********************** -->

## UAI: Kubeflow Trainer capabilities

<div id="fig-template-section1">

<img src="figures/uai_kubeflow.svg" data-fig-align="center" />

Figure 2: User Personas in Kubeflow Trainer

</div>

<div style="font-size: 55%;">

An overview of Kubeflow Trainer:
<https://www.kubeflow.org/docs/components/trainer/overview/>

</div>

<div class="notes">

</div>

<!-- *********************** NEW SLIDE *********************** -->

## UAI: GitHub Container Registry

<div id="fig-template-section1">

<img src="figures/uai_docker_images.svg" data-fig-align="center" />

Figure 3: Worflow for GitHub Container Registry

</div>

<div style="font-size: 55%;">

An overview of Kubeflow Trainer:
<https://www.kubeflow.org/docs/components/trainer/overview/>

</div>

<div class="notes">

</div>

<!-- *********************** NEW SLIDE *********************** -->

## Dockerfiles

<div class="panel-tabset">

### STRUCTURE: Dockerfile -\>

<div class="code-with-filename">

**Dockerfile**

``` python

# syntax=docker/dockerfile:1.7
FROM docker.io/pytorch/pytorch:2.9.1-cuda12.8-cudnn9-devel

RUN mkdir -p /workspace && chmod -R 777 /workspace
WORKDIR /workspace

COPY requirements.txt .

RUN /opt/conda/bin/python -m pip install --upgrade pip && \
    /opt/conda/bin/python -m pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["/opt/conda/bin/python"]
```

</div>

### \<- requirements.txt

<div class="code-with-filename">

**requirements.txt**

``` python

# core dependencies
pillow
loguru
notebook
numpy
omegaconf
pandas
pyyaml
wandb

# test dependencies
black
codespell
detect-secrets
isort
pre-commit
pylint
pytest

# learning dependencies
accelerate
basicsr
diffusers
einops
scikit-learn
torch
torchvision
```

</div>

### SPEED: Dockerfile-scratch-volume

<div class="code-with-filename">

**Dockerfile-scratch-volume**

``` python

# syntax=docker/dockerfile:1.7
FROM docker.io/pytorch/pytorch:2.9.1-cuda12.8-cudnn9-devel

RUN mkdir -p /workspace && chmod -R 777 /workspace
RUN mkdir -p /.cache/pip /.local && chmod -R 777 /.cache/pip /.local

WORKDIR /workspace
```

</div>

</div>

<div class="notes">

Speaker notes go here. {.scrollable}

</div>

<!-- *********************** NEW SLIDE *********************** -->

## Training EDM2 Model (kubeflow 0.3.0)

<div class="panel-tabset">

### STRUCTRE: training-edm2-model-ghcr

<div class="code-with-filename">

**training-edm2-model-ghcr.ipynb**

``` python

# https://github.com/xfetus/fetal-ultrasound-edm2/blob/main/unified-ai/training-edm2-model-ghcr.ipynb

## Set how many PyTorch nodes you want to use for distributed training.
NUM_NODES = 1

# Set the resources for each PyTorch node.
RESOURCES_PER_NODE = {
    "cpu": "4",           # CPUs per node
    "memory": "64Gi",     # Memory in GiB per node (tried 2Gi CrashLoopBackOff/OOMKilled), 64Gi works
    "nvidia.com/gpu": 1,  # GPUs per node (the number will depend on the available resources)
}

GITHUB_CONTAINER_REGISTRY = "ghcr.io/xfetus/fetal-ultrasound-edm2/fetal-ultrasound-edm2-distributed-learning:v0.1.1"

command = TrainerCommand(
    command=[
        "torchrun",
        f"--nnodes={NUM_NODES}",
        "train_edm2.py", #path of script in scratch 
        "--outdir", "/scratch-volume/FETAL_PLANES_DB/OUTPUT_DIRECTORY", # pragma: allowlist secret
        "--data", "/scratch-volume/FETAL_PLANES_DB", # pragma: allowlist secret
        "--batch", "4",
        "--preset", "edm2-img512-s",
        "--batch-gpu", "4",
    ]
)



job_id = trainer.train(
    runtime=torch_runtime,
    trainer=CustomTrainerContainer(
        image=GITHUB_CONTAINER_REGISTRY,
        num_nodes=NUM_NODES,
        resources_per_node=RESOURCES_PER_NODE,
        env=ENV_VARS        
    ),
    options=[command, pod_template_overrides],
)

```

</div>

### SPEED: training-edm2-model-scratch-volume

<div class="code-with-filename">

**training-edm2-model-scratch-volume.ipynb**

``` python

# https://github.com/xfetus/fetal-ultrasound-edm2/blob/main/unified-ai/training-edm2-model-scratch-volume.ipynb


## Set how many PyTorch nodes you want to use for distributed training.
NUM_NODES = 1

# Set the resources for each PyTorch node.
RESOURCES_PER_NODE = {
    "cpu": "4",           # CPUs per node
    "memory": "64Gi",     # Memory in GiB per node (tried 2Gi CrashLoopBackOff/OOMKilled), 64Gi works
    "nvidia.com/gpu": 1,  # GPUs per node (the number will depend on the available resources)
}

GITHUB_CONTAINER_REGISTRY = "ghcr.io/xfetus/fetal-ultrasound-edm2/fetal-ultrasound-edm2-distributed-learning:v0.0.1"
# VERSION_ID=v0.0.1 #FROM docker.io/pytorch/pytorch:2.9.1-cuda12.8-cudnn9-devel / RUN mkdir -p /workspace && chmod -R 777 /workspace 
#                    RUN mkdir -p /.cache/pip /.local && chmod -R 777 /.cache/pip /.local


command = TrainerCommand(
    command=[
        "bash", "-c",
        (
            # Create writable dirs
            "mkdir -p /scratch-volume/pip-packages "
            "/scratch-volume/torch-inductor-cache "
            "/scratch-volume/home && "
            # Install deps exclude torch/torchvision (already in base image)
            # Use --upgrade to overwrite stale packages from previous runs
            "pip install "
            "pandas "
            "accelerate "
            "basicsr "
            "diffusers "
            "einops "
            "scikit-learn "
            "--target=/scratch-volume/pip-packages "
            "--upgrade "
            "--no-cache-dir "
            "--quiet && "
            # Set cache env vars inline to guarantee they're set before torchrun
            "export HOME=/scratch-volume/home && "
            "export TORCHINDUCTOR_CACHE_DIR=/scratch-volume/torch-inductor-cache && "
            "export PYTHONPATH=/scratch-volume/pip-packages:$PYTHONPATH && "          
            "torchrun /scratch-volume/fetal-ultrasound-edm2/train_edm2.py "
            "--outdir /scratch-volume/FETAL_PLANES_DB/OUTPUT_DIRECTORY "
            "--data /scratch-volume/FETAL_PLANES_DB "
            "--batch 4 "
            "--preset edm2-img512-s "
            "--batch-gpu 4"
        )
    ]
)



job_id = trainer.train(
    runtime=torch_runtime,
    trainer=CustomTrainerContainer(
        image=GITHUB_CONTAINER_REGISTRY,
        num_nodes=NUM_NODES,
        resources_per_node=RESOURCES_PER_NODE,
        env=ENV_VARS        
    ),
    options=[command, pod_template_overrides],
)

```

</div>

</div>

<div style="font-size: 55%;">

Jupyter Notebooks:
<https://github.com/xfetus/fetal-ultrasound-edm2/blob/main/unified-ai/training-edm2-model-ghcr.ipynb>\
<https://github.com/xfetus/fetal-ultrasound-edm2/blob/main/unified-ai/training-edm2-model-scratch-volume.ipynb>

</div>

<div class="notes">

Speaker notes go here. {.scrollable}

</div>

<!-- *********************** NEW SLIDE *********************** -->

## Diffusion-based synthesis (512×512)

<div id="fig-template-section2">

<img src="figures/miua_fig1.svg" data-fig-align="center" />

Figure 4: Representative fetal ultrasound images from real data, Tian et
al. \[22\], and our proposed high-resolution (512×512) diffusion-based
synthesis approach.

</div>

<div style="font-size: 55%;">

**Mannering et al. 2026** in MIUA 2026
(https://arxiv.org/abs/2608.05471)

</div>

<div class="notes">

Speaker notes go here.

</div>

<!-- *********************** NEW SLIDE *********************** -->

##  Github: Getting started docs

<div id="fig-template-section2">

<img src="figures/00_template-vector-images/drawing-v00.svg"
data-fig-align="center" />

Figure 5: Getting started documentation provide with a range of links to
setup, use, run and debug application including github workflow.

</div>

<div style="font-size: 55%;">

**Sciortino et al. 2017** in Computers in Biology and Medicine
<https://doi.org/10.1016/j.compbiomed.2017.01.008> **He et al. 2021** in
Front. Med. <https://doi.org/10.3389/fmed.2021.729978>

</div>

<div class="notes">

Speaker notes go here.

</div>

<!-- ============================================================
     SECTION: Section title 2
     ============================================================ -->

# Rapid Prototyping Workflow

**Add Subtitle**

<div class="notes">

<!-- TODO: notes specific to Section title 2 (was previously a duplicate
     of Section title 1's notes — make sure this describes section 2) -->

</div>

<!-- *********************** NEW SLIDE *********************** -->

##  Github: Getting started docs

<div id="fig-template-section2">

<img src="figures/00_template-vector-images/drawing-v00.svg"
data-fig-align="center" />

Figure 6: Getting started documentation provide with a range of links to
setup, use, run and debug application including github workflow.

</div>

<div style="font-size: 55%;">

**Sciortino et al. 2017** in Computers in Biology and Medicine
<https://doi.org/10.1016/j.compbiomed.2017.01.008> **He et al. 2021** in
Front. Med. <https://doi.org/10.3389/fmed.2021.729978>

</div>

<div class="notes">

Speaker notes go here.

</div>

<!-- *********************** NEW SLIDE *********************** -->

## Title of the slide

- Bullet point 1
- Bullet point 2
- **Bullet point** 3
  - Bullet point 3.1
  - Bullet point 3.2

<div style="font-size: 55%;">

**Sciortino et al. 2017** in Computers in Biology and Medicine
<https://doi.org/10.1016/j.compbiomed.2017.01.008> **He et al. 2021** in
Front. Med. <https://doi.org/10.3389/fmed.2021.729978>

</div>

<div class="notes">

Notes go here

</div>

<!-- ============================================================
     SECTION: Section title 3
     ============================================================ -->

# Structured Production Workflow

**Add Subtitle**

<div class="notes">

<!-- TODO: notes specific to Section title 2 (was previously a duplicate
     of Section title 1's notes — make sure this describes section 2) -->

</div>

<!-- *********************** NEW SLIDE *********************** -->

##  Github: Getting started docs

<div id="fig-template-section2">

<img src="figures/00_template-vector-images/drawing-v00.svg"
data-fig-align="center" />

Figure 7: Getting started documentation provide with a range of links to
setup, use, run and debug application including github workflow.

</div>

<div style="font-size: 55%;">

**Sciortino et al. 2017** in Computers in Biology and Medicine
<https://doi.org/10.1016/j.compbiomed.2017.01.008> **He et al. 2021** in
Front. Med. <https://doi.org/10.3389/fmed.2021.729978>

</div>

<div class="notes">

Speaker notes go here.

</div>

<!-- ============================================================
     SECTION: Section title 4
     ============================================================ -->

# Conclusions, Future Work & Next Steps

<div class="notes">

<!-- TODO: notes specific to Section title 2 (was previously a duplicate
     of Section title 1's notes — make sure this describes section 2) -->

</div>

<!-- *********************** NEW SLIDE *********************** -->

##  Github: Getting started docs

<div id="fig-template-section2">

<img src="figures/00_template-vector-images/drawing-v00.svg"
data-fig-align="center" />

Figure 8: Getting started documentation provide with a range of links to
setup, use, run and debug application including github workflow.

</div>

<div style="font-size: 55%;">

**Sciortino et al. 2017** in Computers in Biology and Medicine
<https://doi.org/10.1016/j.compbiomed.2017.01.008> **He et al. 2021** in
Front. Med. <https://doi.org/10.3389/fmed.2021.729978>

</div>

<div class="notes">

Speaker notes go here.

</div>

<!-- ============================================================
     EXTRA SLIDES (appendix)
     ============================================================ -->

# Appendix

Extra slides for Q&A

<!-- *********************** NEW SLIDE *********************** -->

## Title of the slide

- Bullet point 1
- Bullet point 2
- **Bullet point** 3
  - Bullet point 3.1
  - Bullet point 3.2

<div style="font-size: 55%;">

**Sciortino et al. 2017** in Computers in Biology and Medicine
<https://doi.org/10.1016/j.compbiomed.2017.01.008> **He et al. 2021** in
Front. Med. <https://doi.org/10.3389/fmed.2021.729978>

</div>

<div class="notes">

Notes go here

</div>

<!-- *********************** NEW SLIDE *********************** -->

##  Github: Getting started docs

<div id="fig-template-section1">

<img src="figures/00_template-vector-images/drawing-v00.svg"
data-fig-align="center" />

Figure 9: Getting started documentation provide with a range of links to
setup, use, run and debug application including github workflow.

</div>

<div style="font-size: 55%;">

**Sciortino et al. 2017** in Computers in Biology and Medicine
<https://doi.org/10.1016/j.compbiomed.2017.01.008> **He et al. 2021** in
Front. Med. <https://doi.org/10.3389/fmed.2021.729978>

</div>

<div class="notes">

Speaker notes go here.

</div>

<!-- ============================================================
     CALL TO ACTION: Reproduce this work
     ============================================================ -->

## Reproduce this work

<div class="columns">

<div class="column" width="55%">

#### Get running in three steps

1.  **Clone** the repo — everything here (code, data, this deck) lives
    in one place
2.  **Build** the environment — `Dockerfile` / `environment.yml`
    included
3.  **Run** the notebook or pipeline — outputs regenerate the figures in
    this talk

``` bash
git clone https://github.com/<org>/<repo>.git
cd <repo>
# TODO: add your one-line setup command
```

</div>

<div class="column" width="45%">

<div class="card">

<span class="card-icon">📦</span> \#### What’s included - Source code +
tests - Reproducible environment - Example data / demo notebook - This
slide deck (Quarto)

</div>

<a href="#" class="cta">View on GitHub →</a>

</div>

</div>

<div class="notes">

Emphasise licensing and how to cite — point people to the CITATION.cff
if the repo has one.

</div>
