---
title: 'Retraining-free Constraint-aware Token Pruning for Vision Transformer on Edge Devices'

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - Yun-Chia Yu
  - Mao-Chi Weng
  - Min-Guang Lin
  - An-Yeu Wu

# Author notes (optional)
author_notes:
  - 'Equal contribution'
  - 'Equal contribution'

date: '2024-01-16T00:00:00Z'
# doi: ''

# Schedule page publish date (NOT publication's date).
publishDate: '2024-01-06T00:00:00Z'

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ['paper-conference']

# Publication name and optional abbreviated publication name.
publication: Submitted to IEEE International Symposium on Circuits and Systems 2024
publication_short: IEEE ISCAS 2024, to appear

abstract: Vision transformer (ViT) has shown great potential in computer vision tasks. However, intensive computation requirements with respect to the token size hinder ViT from being deployed on edge devices with diverse computation resources. Recently, token pruning has been a promising method to exploit the redundancy of tokens. However, it often requires a laborious retraining process to meet different resource constraints. In this paper, we introduce Fisher information (FI) from tokens to evaluate token importance across different transformer blocks and propose a Retraining-free Constraint-aware Token Pruning (RCTP) framework. RCTP employs a two-step process to obtain the optimal pruning thresholds without retraining under different FLOPs constraints. Firstly, a candidate threshold table and a FLOPs-Fisher table are constructed through a three-stage pipeline to record the trade-off between FLOPs and FI loss of each candidate threshold. Secondly, a modified Viterbi algorithm determines optimal threshold sets with minimum overall FI loss under various FLOPs-constraints in one shot. Our experiment shows that RCTP attains better accuracy-FLOPs trade-off than prior pruning-based approaches.

# Summary. An optional shortened abstract.
summary:  In this paper, we introduce Fisher information (FI) from tokens to evaluate token importance across different transformer blocks and propose a Retraining-free Constraint-aware Token Pruning (RCTP) framework. RCTP employs a two-step process to obtain the optimal pruning thresholds without retraining under different FLOPs constraints.

tags: 
 - Vision Transformer
 - Hardware-aware Machine Learning
 - Token Pruning

# Display this page in the Featured widget?
featured: True 

# Custom links (uncomment lines below)
links:
# - name: Custom Link
#   url: http://example.org
url_pdf: 'https://drive.google.com/file/d/1oEraoejefcmSlQKYe6XS5XxVSqlObkZE/view'
# url_code: 'https://github.com/HugoBlox/hugo-blox-builder'
# url_dataset: 'https://github.com/HugoBlox/hugo-blox-builder'
# url_poster: ''
# url_project: ''
# url_slides: ''
# url_source: 'https://github.com/HugoBlox/hugo-blox-builder'
# url_video: 'https://youtube.com'

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
image:
  caption: ''
  focal_point: ''
  preview_only: false
# post: 
#   - RCTP
# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
# projects:
#   - example

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.

# slides: example
---
<!-- 
{{% callout note %}}
Click the _Cite_ button above to demo the feature to enable visitors to import publication metadata into their reference management software.
{{% /callout %}}

{{% callout note %}}
Create your slides in Markdown - click the _Slides_ button to check out the example.
{{% /callout %}}

Add the publication's **full text** or **supplementary notes** here. You can use rich formatting such as including [code, math, and images](https://docs.hugoblox.com/content/writing-markdown-latex/). -->
