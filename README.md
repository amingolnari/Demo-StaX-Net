# [**StaX-Net: Stackelberg Interleaved Asymmetric Network for Cross-Modality Medical Image Translation**](https://arxiv.org/abs/[ARXIV-ID])  

---

**Amin Golnari**<sup>a,∗</sup>, **Saeid Sanei**<sup>a,b,c</sup>  

<sup>a</sup> College of Engineering & Computer Science, VinUniversity, Hanoi, Vietnam  
<sup>b</sup> College of Health Science, VinUniversity, Hanoi, Vietnam  
<sup>c</sup> Institute of Psychiatry, Psychology & Neuroscience, King’s College London, UK  

**Link to the preprint version:** [arXiv preprint](https://arxiv.org/abs/[ARXIV-ID])

**Run this implementation on Google Colab:**  
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/amingolnari/Demo-StaX-Net/blob/main/StaX-Net.ipynb)

**Compatibility:**
- TensorFlow: ≥ 2.15.0  
- Keras: ≥ 3.0.0  
- Python: ≥ 3.9  

---

## **Framework Overview**

**StaX-Net** introduces a novel **Stackelberg Interleaved Asymmetric Network** designed to address the critical challenge of anatomically inconsistent synthesis and hallucination in cross-modality medical image translation (e.g., CT-to-MRI). 

Traditional GANs often suffer from semantic misalignment and gradient conflicts. StaX-Net resolves this through three integrated components: 
1. A **partially coupled encoder-discriminator structure** that shares multi-scale computational blocks to enforce feature alignment.
2. A **Stackelberg-inspired asymmetric optimization scheme** that stabilizes training by enforcing explicit shared-parameter freezing and asymmetric learning rates, treating the discriminator as the "leader" and the generator as the "follower".
3. A **2D Feature Aggregation and Spatial Transformation (FAST) layer** operating under the **Learning Using Privileged Information (LUPI)** paradigm. This layer distills target-domain (MRI) structural priors into a dynamic, source-only (CT) feature transformation during training, acting as a structural regularizer that is entirely omitted at inference.

---

## **Key Features:**

1. **Interleaved Coupled Architecture**:  
   - Explicitly shares identical computational blocks between the generator and discriminator at multiple spatial resolutions.
   - Maintains pathway-specific activations and dedicated task layers, preventing the collapse into a single shared encoder.

2. **Stackelberg Asymmetric Optimization**:  
   - Replaces simultaneous minimax updates with a sequential leader-follower dynamic.
   - The discriminator (leader) updates the shared weights first. The generator (follower) then uses these exact weights as *fixed* feature extractors, updating only its dedicated synthesis modules. This eliminates adversarial gradient collisions.

3. **FAST Layer (LUPI Paradigm)**:  
   - Injects target-supervised spatial priors via a dynamic prior bank using FiLM modulation and AdaIN styling.
   - The privileged target (MRI) is used *exclusively* for computing the auxiliary FAST loss during training, ensuring the model learns a deterministic, source-only mapping for real-world inference.

4. **Superior Volumetric Fidelity**:  
   - Evaluated on the **SynthRAD 2025** dataset (Abdomen/Pelvis, Head & Neck, and Thorax).
   - Achieves competitive 3D consistency (low Z-MAE) and high-frequency texture preservation, outperforming standard 2D GAN baselines and rivaling computationally expensive 3D diffusion models.

5. **Clinically Safe Synthesis**:  
   - Combines adversarial feedback with deterministic anchors (L1 and SSIM losses) to ensure the transfer of source anatomy without inventing clinically implausible structures.

---

## **Usage of StaX-Net in Practice**

The **StaX-Net architecture** and its **asymmetric training loop** (Algorithm 1 in the paper) can be adapted for various cross-modality translation tasks beyond CT-to-MRI (e.g., PET-to-MRI, or cross-sequence MRI translation). 

The **FAST layer** can be integrated as a drop-in module within any generator's encoding pathway to provide target-informed regularization via the LUPI paradigm. The sequential training script ensures that shared weights are updated only by the discriminator, providing a stable and reproducible training dynamic without the need for complex hypergradient calculations.

---

If our work is helpful to you, please kindly cite our paper as:

```bibtex
@article{golnari2026staxnet,
  title={StaX-Net: Stackelberg Interleaved Asymmetric Network for Cross-Modality Medical Image Translation},
  author={Golnari, Amin and Sanei, Saeid},
  journal={arXiv preprint arXiv:[ARXIV-ID]},
  year={2026}
}
