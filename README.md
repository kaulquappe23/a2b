# Leveraging Anthropometric Measurements to Improve Human Mesh Estimation and Ensure Consistent Body Shapes

This repository contains only the inference code for the A2B models of our paper "[Leveraging Anthropometric Measurements to Improve Human Mesh Estimation and Ensure Consistent Body Shapes](https://arxiv.org/abs/2409.17671)". If you are interested in the full functionality, evaluation and IK code, please refer to the [full repository](https://github.com/kaulquappe23/a2b_human_mesh).

The paper is accepted for [CVsports'25](https://vap.aau.dk/cvsports/) at [CVPR 2025](https://cvpr.thecvf.com/Conferences/2025). 


## Installation

Create a new anaconda environment by running 
```bash
conda env create -f environment.yml
```
and activate the environment with
```bash
conda activate a2b_inference
```

## A2B Models

We provide the inference code for our A2B models. Since the models are very small, they are included in this repository in the folder 'a2b_models`, so no download is necessary. To use them, all anthropometric measurements should be given in a json file. This file needs to contain a dictionary mapping from subject names to anthropometric measurements. The measurements need to be given in a dictionary mapping from measurement key to the value in meters. The following measurements are expected:

`height length, shoulder width length, torso height from back length, torso height from front length, head length, midline neck length, lateral neck length, left hand length, right hand length, left arm length, right arm length, left forearm length, right forearm length, left thigh length, right thigh length, left calf length, right calf length, left footwidth length, right footwidth length, left heel to ball length, right heel to ball length, left heel to toe length, right heel to toe length, waist circumference, chest circumference, hip circumference, head circumference, neck circumference, left arm circumference, right arm circumference, left forearm circumference, right forearm circumference, left thigh circumference, right thigh circumference, left calf circumference, right calf circumference`

Therefore, the format of the json input file should look like the following. We further provide an example anthropometric measurements file `anthro/example_measurements.json`. The landmarks used for the measurements are given in the supplementary material of the paper.
```json
{
    "subject1": {
        "height length": 1.8,
        "shoulder width length": 0.4,
        "torso height from back length": 0.4,
        "torso height from front length": 0.4,
        "head length": 0.2,
        "midline neck length": 0.1,
        "lateral neck length": 0.1,
        "left hand length": 0.2,
        "right hand length": 0.2,
        "left arm length": 0.3,
        "right arm length": 0.3,
        "left forearm length": 0.2,
        "right forearm length": 0.2,
        "left thigh length": 0.5,
        "right thigh length": 0.5,
        "left calf length": 0.4,
        "right calf length": 0.4,
        "left footwidth length": 0.1,
        "right footwidth length": 0.1,
        "left heel to ball length": 0.2,
        "right heel to ball length": 0.2,
        "left heel to toe length": 0.3,
        "right heel to toe length": 0.3,
        "waist circumference": 0.8,
        "chest circumference": 1.0,
        "hip circumference": 0.9,
        "head circumference": 0.5,
        "neck circumference": 0.3,
        "left arm circumference": 0.2,
        "right arm circumference": 0.2,
        "left forearm circumference": 0.2,
        "right forearm circumference": 0.2,
        "left thigh circumference": 0.3,
        "right thigh circumference": 0.3,
        "left calf circumference": 0.2,
        "right calf circumference": 0.2
    },
    "subject2": {
        ...
    }
}
```
 In order to run the inference, execute

```bash
python -m inference --measurements <path_to_measurements_json> --save_path <path_to_save_beta_predictions> --name <measurement_name>
```

You can try the inference with the example file by executing:
```bash
python -m inference --measurements "example_measurements.json" --save_path "anthro_betas.json" --name example
```

The output then looks like the following:

```json
{
    "example_A2B_svr_male":
    {
        "gender": "male",
        "name": "example_A2B_svr_male",
        "subject_1":
        [
            1.5561325028191986,
            -0.8166642549933809,
            1.0529247639574861,
            -0.9301149961670858,
            -1.2445880571259105,
            0.3664527874039336,
            0.28154321636613916,
            3.45716809571207,
            -0.5160723521828383,
            -1.795204227512637,
            2.1629444945670566
        ],
        "subject_2": ...,
        "num_betas": 11
    },
    "example_A2B_nn_male":
    {
        "gender": "male",
        "name": "example_A2B_nn_male",
        "subject_1":
        [
            1.4699337482452393,
            -0.9374016523361206,
            0.8422941565513611,
            -0.5198159217834473,
            -5.681121349334717,
            -1.4253689050674438,
            0.390656977891922,
            2.2848150730133057,
            -0.3818075656890869,
            -1.983944296836853,
            1.1676679849624634
        ],
        "subject_2": ...,
        "num_betas": 11
    },
    "example_A2B_svr_neutral":...,
    "example_A2B_nn_neutral":...,
    "example_A2B_svr_female":...,
  

}
```
The output is given per evaluated A2B model (SVR, NN) and per gender (neutral, female, male). For each model output, you will find the name (of model and anthropometric measurements as given on the command line: `<given_name>_A2B_<svr/nn>_<gender>`), the gender, the number of beta parameters used and the results per subject.

It is further possible to pass a pickle-file that contains a dictionary mapping from subject names to a dictionary of anthropometric measurements, whereby the measurements should be a list. This functionality is used for the evaluation process (see Section *Evaluation* of the full repository). The list of measurements is reduced with a median operation and then passed to the A2B models. The output is then a single set of beta parameters per subject per A2B model. The final result is identical to the example output above.

# Citation

In case this work is useful for your research, please consider citing:
```bibtex
@article{ludwig2024leveraging,
  title={Leveraging Anthropometric Measurements to Improve Human Mesh Estimation and Ensure Consistent Body Shapes},
  author={Ludwig, Katja and Lorenz, Julian and Kienzle, Daniel and Bui, Tuan and Lienhart, Rainer},
  journal={arXiv preprint arXiv:2409.17671},
  year={2024}
}
```
