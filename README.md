# Stable Diffusion Workflows

This repo contains my workflow files for Stable Diffusion with [ComfyUI](https://github.com/comfyanonymous/ComfyUI).
I provided notebooks for both Paperspace and Google Colab, simply click the link to start running SD.

[![Run on Gradient](https://assets.paperspace.io/img/gradient-badge.svg)](https://console.paperspace.com/github/noxouille/stable-diffusion-workflows?container=noxouille/comfyui-stable-diffusion:latest)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/noxouille/stable-diffusion-workflows/blob/main/notebooks/custom_comfyui_colab.ipynb)

Use my Paperspace referral code [3NZ590H](https://console.paperspace.com/signup?R=3NZ590H) to receive $10 in credit.

## Why multiple workflows ?

There are a lot of cool ultimate workflows out there, and workflows that tries to arrange the UI as compact as possible, and that's totally fine, if your only goal is to generate quality images.
I prefer modular workflow because it's way easier to visualize, reroute the flow and try new things to come up with different concepts while experimenting.
I find it counterproductive when a workflow is arranged as a monolith of control panel interface.

ComfyUI is a great interface to do exactly that, with only drag-and-drop modules instead of coding the custom python functions yourself, which speeds things up a lot!
It seems to be more efficient performance-wise as well, as ComfyUI only loads and process the nodes that you use.
The extensions I used also provide better optimization and quality of life.

Each of the workflows here are aimed at a single objective to facilitate that.

## Dockerfile

The docker image to run ComfyUI is hosted on Docker Hub: [noxouille/comfyui-stable-diffusion](https://hub.docker.com/r/noxouille/comfyui-stable-diffusion)

## Noise

The different noise in the Noise folder is a reupload of attachment files of [this mindblowing 4 parts-blog of Mitsuki Nozomi](https://note.com/mitsukinozomi/n/n3b9fb82f2ed6).
Full credit goes to the blog author.
After reading the blog, I always incorporate additive noises to enhance details and color transfer.
Sometimes I replace the details noise with PPF noise to see if I get interesting results.
