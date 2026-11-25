<h1 align="center">Attention ROI warp</h1>

<p align="center">
  A set of attention-network regions of interest, defined in MNI space, warped into an individual subject's T1 space with ANTs.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.168"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.168-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey.svg" alt="MIT license"></a>
</p>

## 🧠 At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Registers a T1-weighted brain image to MNI152 with ANTs (rigid, affine and SyN) and warps a bundled set of attention-network ROIs from Sani et al. into the subject's T1 space.**

This service aligns two sets of Regions of Interest (ROIs) from Sani et al., in preparation to the individual subject space. A first set of ROIs has been obtained by thresholding functional MRI data. A second set of ROIs has been obtained by creating sphere ROIs around the peak of functional MRI activation. The ROIs are stored here: https://github.com/brainlife/VisWMROIWarp/tree/master/rois

| | |
|---|---|
| 📥 **Inputs** | `neuro/anat/t1w` · `neuro/mask` |
| 📤 **Outputs** | `neuro/rois` |
| 🐳 **Container** | `brainlife/ants:2.2.0` |
| ⚙️ **Resources** | 8 CPUs · 18 h walltime |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.168](https://doi.org/10.25663/brainlife.app.168) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `t1` | `neuro/anat/t1w` | yes | Skull-stripped T1-weighted image of the subject. Registered to the MNI152 1 mm brain template, so it should be brain-extracted. |
| `mask` | `neuro/mask` (brain) | yes | Brain mask of the T1w image, used as the moving mask during registration. |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `output` | `neuro/rois` | The attention ROIs (fMRI-thresholded and 10 mm sphere ROIs shipped with the App) resampled into the subject's T1 space with the inverse ANTs transform. |

<!-- brainlife-app:end -->

## About

This custom App warps a set of [attention-related ROIs located in MNI space](rois/) to individual subject space. The ROIs come from Sani et al. and are of two kinds:

- ROIs obtained by thresholding functional MRI data (`ROI*.nii.gz`);
- 10 mm sphere ROIs around the peaks of functional MRI activation (`*_r10_sphere_bin.nii.gz`, `R_phPITd_sphere_bin.nii.gz`): FEF, LIP and phPITd in each hemisphere.

## 🚀 Running the App

### ☁️ On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.168](https://doi.org/10.25663/brainlife.app.168), choose the inputs below from your project and press **Execute**. The App writes a [`neuro/rois`](https://brainlife.io/datatypes/5be9ea0315a8683a39a1ebd9) dataset to your project.

- [`neuro/anat/t1w`](https://brainlife.io/datatypes/58c33bcee13a50849b25879a): the T1 anatomy of the subject. It is registered to the MNI152 1 mm *brain* template, so it should be skull-stripped.
- [`neuro/mask`](https://brainlife.io/datatypes/5a281aee2c214c9ba83ce620) tagged `brain`: the brain mask of the same T1.

### 🔬 How it works

1. The subject's T1 is registered to the MNI152 1 mm brain template with `antsRegistration` (rigid, then affine, then SyN), using the MNI and subject brain masks.
2. Each ROI in [`rois/`](rois/) is moved into the subject's T1 space with `antsApplyTransforms`, using the inverse warp and the inverse affine.

### 💻 Locally

You need [Singularity/Apptainer](https://apptainer.org/) and [`jq`](https://jqlang.github.io/jq/). The App runs in the `brainlife/ants:2.2.0` container, which also provides the MNI152 1 mm brain template and mask from the HCP pipelines.

1. Clone this repository.
2. Create `config.json` with the paths to your inputs:

   ```json
   {
       "t1": "/input/t1/t1.nii.gz",
       "mask": "/input/mask/mask.nii.gz"
   }
   ```

3. Run the App:

   ```bash
   ./main
   ```

Without Singularity, run [`CoordTransform.sh`](CoordTransform.sh) directly. It needs [ANTs](https://github.com/ANTsX/ANTs) and an MNI152 1 mm T1 brain template and brain mask (see, for example, the [ICBM 152 templates](http://www.bic.mni.mcgill.ca/ServicesAtlases/ICBM152NLin2009) or the [FSL atlases](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/Atlases)); edit the `MNI` and `MSK` paths at the top of the script to point to them.

## 📦 Outputs

```text
output/
└── rois/            every ROI from rois/, resampled into the subject's T1 space (same file names)
t1_to_mni_*          ANTs transforms and the T1 warped to MNI space
```

## 📚 Citing

> [!TIP]
> If you use this App in your work, please cite the App, the software it builds on and brainlife.io (below). GitHub's **Cite this repository** button gives the App citation in APA and BibTeX.

- Sani I, Stemmann H, Caron B, Bullock D, Stemmler T, Fahle M, Pestilli F, Freiwald WA. The human attentional control network includes a ventro-temporal cortical node. (Under review at the time this App was released.)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)
- [ANTs](http://picsl.upenn.edu/software/ants/)
- Attention ROI warp. brainlife.io App. [doi:10.25663/brainlife.app.168](https://doi.org/10.25663/brainlife.app.168)
- Avants et al. A reproducible evaluation of ANTs similarity metric performance in brain image registration. *NeuroImage* 54:2033-2044 (2011). [doi:10.1016/j.neuroimage.2010.09.025](https://doi.org/10.1016/j.neuroimage.2010.09.025)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)

## 👥 Authors

- [Daniel Bullock](https://github.com/DanNBullock)
- [Ilaria Sani](https://github.com/IlariaSani) (primary user)
- [Franco Pestilli](https://github.com/francopestilli) (project director)

**Contributors:** [Soichi Hayashi](https://github.com/soichih)

## 💰 Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io is publicly funded, and acknowledging that support helps keep it running. This App was developed with support from Wellcome, the U.S. National Institutes of Health (including the BRAIN Initiative BRAIN CONNECTS program: The Axonal Projectome EXchange, U24NS140384, and the Center for Mesoscale Connectomics, UM1NS132207) and the U.S. National Science Foundation.</sub>

## ⚖️ License

MIT.
