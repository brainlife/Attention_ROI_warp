[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.168-blue.svg)](https://doi.org/10.25663/brainlife.app.168)

# Attention ROI warp
This is a custom made app designed to warp a set of [attention related ROIS located in MNI space](https://github.com/brainlife/Attention_ROI_warp/tree/master/rois) to individual subject space.

### Authors
- [Daniel Bullock](https://github.com/DanNBullock) (dnbulloc@iu.edu)

### Primary User
- [Ilaria Sani](https://github.com/IlariaSani) (ila.gina@virgilio.it)

### Contributors
- [Soichi Hayashi](https://github.com/soichih) (hayashis@iu.edu)

### Project Director
- [Franco Pestilli](https://github.com/francopestilli) (franpest@indiana.edu)

### Funding 
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NIMH-T32-5T32MH103213-05](https://img.shields.io/badge/NIMH_T32-5T32MH103213--05-blue.svg)](https://projectreporter.nih.gov/project_info_description.cfm?aid=9725739)
[SANI FUNDING GOES HERE]

### References 
**The human attentional control network includes a ventro-temporal cortical node**,
_Ilaria Sani, Heiko Stemmann, Bradley Caron, Daniel Bullock, Torsten
Stemmler, Manfred Fahle, Franco Pestilli, Winrich A. Freiwald_
**UNDER REVIEW**

[ANTs](http://picsl.upenn.edu/software/ants/)

## Running the App 

### Inputs

-[anat/t1w](https://brainlife.io/datatypes/58c33bcee13a50849b25879a) - the T1 anatomy for this subject

-[mask [brain]](https://brainlife.io/datatypes/5a281aee2c214c9ba83ce620) - the brain mask for this subjet

### On Brainlife.io

Visit [this site](https://doi.org/10.25663/brainlife.app.168) to run this app on the brainlife.io platform.  Minimally requires the aforementioned data inputs.

### Running Locally (on your machine) using singularity & docker

Because this is compiled code which runs on singularity, you can download the repo and [run it locally with minimal setup](https://github.com/brainlife/Attention_ROI_warp/blob/276bbc6210cf6dbc5b715e9327ad89e091634a8a/main#L9).  

### Running Locally (on your machine)

The primary script for this application is located [here](https://github.com/brainlife/Attention_ROI_warp/blob/master/CoordTransform.sh), but will require the [MNI T1 template](https://github.com/brainlife/Attention_ROI_warp/blob/276bbc6210cf6dbc5b715e9327ad89e091634a8a/CoordTransform.sh#L21) and [brain mask](https://github.com/brainlife/Attention_ROI_warp/blob/276bbc6210cf6dbc5b715e9327ad89e091634a8a/CoordTransform.sh#L22).  Examples of the template can be found [here](http://www.bic.mni.mcgill.ca/ServicesAtlases/ICBM152NLin2009) and [here](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/Atlases).  Futhermore, this application makes use of [ANTs](http://picsl.upenn.edu/software/ants/), which can be downloaded [here](https://github.com/ANTsX/ANTs).

### Sample Datasets

TBD

### Output

There is one output type generated by this application:
- [ROIS](https://brainlife.io/datatypes/5be9ea0315a8683a39a1ebd9)

[**ROIS**](https://brainlife.io/datatypes/5be9ea0315a8683a39a1ebd9) - a set of rois, corresponding to the MNI space ROIS found [here](https://github.com/brainlife/Attention_ROI_warp/tree/master/rois)

#### Product.json

Currently not implimented

### Dependencies

This App only requires [singularity](https://www.sylabs.io/singularity/), and [ANTs](http://picsl.upenn.edu/software/ants/).  
([Click here](https://singularity.lbl.gov/docs-installation) to learn more about installing singularity)

 
