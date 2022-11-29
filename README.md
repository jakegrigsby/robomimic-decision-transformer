<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
    </li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>


# Decision Transformer for Robomimic


<!-- ABOUT THE PROJECT -->
## About The Project
>

Decision Transformer reformulates offline reinforcement learning as a sequence modeling problem that can be effectively solved with large Transformer models. The most related work to our project is the original Decision Transformer. 

<!-- GETTING STARTED -->
## Getting Started
This project requires a working version Pytorch, a working version of robomimic, and Mujoco.


## Get the Datasets:
Dataset download link: https://drive.google.com/drive/folders/1dHMUOSLUr6AwW3PETn1DQO9CWMklT

```bash
cd ~/robomimic/robomimic/scripts # or wherever robomimic was git cloned ...
python download_datasets.py --tasks sim --dataset_types all --hdf5_types low_dim
```
should save to `robomimic/datasets`


## Dataset Types

### Machine-Generated (MG)
Mixture of suboptimal data from state-of-the-art RL agents

### Proficient-Human (PH) and Multi-Human (MH)
500 total, with 200 proficient human and 300 multi-human.
Demonstrations from teleoperators of varying proficiency

### Our setting: ALL data
More challenging combination of MG, MH, and PH
Weighted towards lower-quality MG data

## Dataset Tasks
Lift: lift the cube

Can: pick up the can and place it in proper spot 

## Our Decision Transformer Architecutre

![new_arc_decision_transformer](https://user-images.githubusercontent.com/61725820/204408002-9ba41db7-3dd6-454d-a79c-bcfcf1398754.gif)

We input state, actions, and returns-to-go into a causal transformer to get our desired actions. We combine the states actions and return to go into one token. This shortens the sequence length and computational requirements. The original decision transformer uses deterministic policy, we train a multi-modal stochastic policy, which helps to better model continuous actions.

# The Semi-Sparse Reward Function:

We manually altered the reward function to add a semi-sparse success bonus that decreased on every time step, giving a wider distribution of target RTGs for the decision transformer than the default binary option of success in robomimic. The max sequence is 500, so if you go past 500 time steps you get nothing!

**The Function:** max(500 - success time, 0)

In future work, we hope that this function sees more iterations of development. We can also alter the dense reward in the robomimic dataset.

## Results


<!-- CONTACT -->
## Contact

Alex Chandler - alex.chandler@utexas.edu
Jake Grigsby grigsby@cs.utexas.edu
Omeed Tehrani omeed@cs.utexas.edu

