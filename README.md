
# COOL.AI
## Cooling System Control Using Deep Reinforcement Learning


## Abstract



Buildings consume 30% of total energy worldwide. 40% of this enery is consumed by cooling 
and heating systems. 

There is a trade-off between energy consumption and comfort inside buildings (Temperature). 

Existing systems use approximate models with static parameters. These systems exhibit a suboptimal performance (i.e optimum energy does not often imply optimum emperature and vice versa). These systems also fail to account for changes in environment (occupancy and load)


Our approach is to use deep reinforcement learning to control cooling system. This approach does not assume any specific model for the system. Cooling control policy is learned and derived from data. An Agent, via trial-and-error, can make optimal actions even for very complex environments. This system can adapt to changes in environment (inside and outside the facility's/building. 

Please find below a link to the full presentation for this project: (https://docs.google.com/presentation/d/1yqvlOii1ajwODI5cSZdmXYTSz7KpY64holnhCF_5GHo/edit?usp=sharing)

In this project, I present an end-to-end process to design, train and deploy deep reinforcement learning system 

![alt text](https://github.com/qatshana/Cool.AI/blob/master/images/End-to-End-System.png)

## Installation

1) create env file and install requirement.txt as below. The file contains the python package dependencies for this project. Installation can be performed via

```
pip install -r requirement.txt
```

2) copy the following custom OpenAI Gym env to your newly created gym env (myNewEnv in this example):

```
cp python/envs/__init__.py ~/anaconda3/envs/myNewEnv/lib/python3.6/site-packages/gym/envs

cp -R python/envs/ControlRL/ ~/anaconda3/envs/myNewEnv/lib/python3.6/site-packages/gym/envs
```

## Input Data
I used EnergyPlus to generate the data for a single zone data center in San Francisco with one cooling system and 3 setpoint for control. Below are the charts for distribution of outside temp and data center server load

![alt text](https://github.com/qatshana/Cool.AI/blob/master/images/OutsideTemp.png)
![alt text](https://github.com/qatshana/Cool.AI/blob/master/images/ITU_Load.png)

## Training

Training can be performed on new data set by executing the following command

```
python python/main_DDPG -rRL True

```

Currently, number of samples for training data is set at 30,000. This parameter is training_steps and is set in config/config.yml 


## Inference
DDPG system weights are saved in results/weights. User can run two types of tests: Linear and DDPG

1) Linear test:

```
python python/main_MVP.py
```

2) DDP test:

```
python python/main_DDPG.py
```

## Performance & Results

Table below depicts the performance of two models: Linear model and DDPG model. DDPG converged for all samples (10,000 samples) with an average convergence rate at 9.9 steps whereas Linear model failed to converge in 56% of cases with an average convergence rate of 115 steps 

![alt text](https://github.com/qatshana/Cool.AI/blob/master/images/results_table.png)

The charts below depict the typical performance for a Linear model vs. DDPG model. As shown in the graph, DDPG system sometimes overshoot but always converge to target. However, Linear system fluctuate around target temp and fails to coverage - error is function of step size used. In the case below, target temperature was set to 22 C and Linear system output temperature fluctuates between 20 and 24 C

![alt text](https://github.com/qatshana/Cool.AI/blob/master/images/ddpg_overshoot.png)
![alt text](https://github.com/qatshana/Cool.AI/blob/master/images/linear_fluctuate_1.png)

Distribution graphs below show the distribution for steps needed to coverage for two systems (Linear vs. DDPG) for 10,000 samples. DDPG system converged for all samples with a maximum number of convergence steps at 20 with mean at 9.9 steps and standard deviation at 3.9. Linear system performance was poor as the system failed to coverage in 56% of cases

![alt text](https://github.com/qatshana/Cool.AI/blob/master/images/distribution.png)

## Next Steps

1. Compare system performance with existing nonlinear models (thermo-mechanical models)
2. Test system using more sophisticated building structure (multi-zones with multi-cooling systems)
3. Compare DDPG performance with other deep learning solutions such as Proximal Policy Optimization (PPO)


