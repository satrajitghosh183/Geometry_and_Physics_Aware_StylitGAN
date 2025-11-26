# Physics-Guided Latent Relighting with Geometry-Aware StyLitGAN

This repository contains the code and report for a Machine Vision project on **physics-guided latent relighting** in a StyleGAN2 generator. The goal is to learn latent “lighting directions” that change illumination while keeping scene layout and materials approximately fixed, using **approximate geometry** and a **physically based image-formation model** as supervision.

Two implementations are included:

- **Implementation 1** – Prototype with Sobel-based normals, log-domain intrinsic optimisation, and a simple Lambertian/Phong renderer. It demonstrates feasibility but produces very weak edits and unstable training.
- **Implementation 2** – Final system with MiDaS depth + perspective-correct normals, a neural intrinsic decomposer, spherical harmonics lighting, and a Cook–Torrance microfacet BRDF. This version yields strong, geometry-aware relighting with much better training behaviour.

---

## Key Ideas

- Use a frozen **StyleGAN2** (LSUN Bedrooms) backbone and learn **latent directions in \(W^+\)** that act as lighting controls.
- Factor generated images into **albedo × shading** with an intrinsic decomposer and tie shading to:
  - **Geometry:** normals from either Sobel gradients (Impl.1) or MiDaS/DPT depth (Impl.2).
  - **Physics:** Lambertian/Phong (Impl.1) or a **Cook–Torrance microfacet BRDF + spherical harmonics environment lighting** (Impl.2).
- Learn a **multi-scale, light-conditioned latent basis** in Implementation 2, so directions are modulated by explicit lighting parameters and spread across StyleGAN layers.
- Train with a mix of **albedo consistency, geometry-aware shading, photometric**, and **perceptual (VGG/LPIPS)** losses, plus distinction and diversity terms on the latent basis.

