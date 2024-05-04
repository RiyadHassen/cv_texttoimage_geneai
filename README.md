# Decompositional Approach: Text-to-Image Alignment Tool

This repository contains the implementation of the advanced text-to-image alignment tool, designed to accurately generate images from complex text prompts using iterative Visual Question Answering (VQA) feedback. This tool integrates technologies such as Stable Diffusion, CLIP, and BLIP to enhance the fidelity and semantic accuracy of generated images.

## Overview

The text-to-image alignment tool leverages a sophisticated framework that combines multiple AI technologies to ensure images generated from text prompts are semantically aligned and detailed. The tool uses:
- **Stable Diffusion**: For generating initial image outputs from text.
- **CLIP**: For decomposing text into manageable assertions.
- **BLIP**: For aligning images with text through Visual Question Answering.

## Features

- **High Accuracy**: Ensures generated images closely match the detailed descriptions in the text prompts.
- **Iterative Refinement**: Uses feedback from VQA to improve image details iteratively.
- **Quantitative and Qualitative Validation**: Includes mechanisms for both quantitative metrics and qualitative user feedback to assess image alignment.


### Setup
- Dependencies:  
To set up the environment, please run:
```
conda env create -f environment/environment.yml
conda activate dascore
```

## Demo
We provide a quickstart demo notebook `demo.ipynb` to get started with this approach. The demo notebook includes a step-by-step analysis including:

* Using DA-Score for evaluating text to image alignment.
* Using Eval&Refine for gradually improving the quality of generated image outputs.
* Analysing three main ways in which Eval&Refine improves over Stable diffusion and Attend&Excite.


Notes: 
 *  provided with two mechanisms for iterative refinement with Eval&Refine: 
    - Prompt-Weighting (PW)
    - Cross-Attention modulation (CA).
    
    The use of the above can be controlled through `use_pw` and `use_ca` keywords while calling the pipeline. For e.g. to use Prompt-Weighting (PW) but not Cross-Attention modulation (CA), please use:
    ```python
    outputs = pipe.eval_and_refine(parsed_input, vqa_model, use_pw=True, use_ca=False,  max_update_steps=5)
    ```
  * The maximum number of iterative-refinement steps can be controlled by `max_update_steps` parameter. More iterative refinement can potentially help get better image outputs. Eval&Refine can adaptively adjust the actual number of refinement steps by monitoring the DA-Score.

  * The threshold for what is considered as a "good enough output" is controlled by the `dascore_threshold=0.85` and `assertion_alignment_threshold=0.75`. The iterative refinement process considers the final output image to be good enough if the overall DA-Score is greator than the `dascore_threshold`, or, if individual alignment-scores for each assertion are greator than `assertion_alignment_threshold`

  * Reducing the above thresholds can lead to faster convergence at cost of poor T2I alignment and vice versa.

 * Finally, we can visualize how the iterative refinement process gradually improves the generated image outputs by setting `verbose=True` while calling the pipeline.
 ```python
 outputs = pipe.eval_and_refine(parsed_input, vqa_model, max_update_steps=5, verbose=True)
 ```

## References and Credits

The implementation of this text-to-image alignment tool is based on the methods and ideas presented in the following research paper:

- **Paper Title**: Divide, Evaluate, and Refine: Evaluating and Improving Text-to-Image Alignment with Iterative VQA Feedback
- **Authors**: Jaskirat Singh, Liang Zheng
- **Affiliations**: The Australian National University, Australian Centre for Robotic Vision
- **Published at**: 37th Conference on Neural Information Processing Systems (NeurIPS 2023)
- **DOI/Link**: [Visit Project Page](https://1jsingh.github.io/divide-evaluate-and-refine)

This tool leverages the concepts and methodologies developed in this paper, particularly focusing on improving the alignment between complex text prompts and the generated images using a decompositional approach and iterative VQA feedback.



