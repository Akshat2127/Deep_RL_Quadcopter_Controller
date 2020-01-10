# Reinforcement Learning

## Deep_RL_Quadcopter_Controller
A deep Reinforcement Learning Project to teach quadcopter to fly

## Project Overview
The Quadcopter or Quadrotor Helicopter is becoming an increasingly popular aircraft for both personal and professional use. Its maneuverability lends itself to many applications, from last-mile delivery to cinematography, from acrobatics to search-and-rescue.

Most quadcopters have 4 motors to provide thrust, although some other models with 6 or 8 motors are also sometimes referred to as quadcopters. Multiple points of thrust with the center of gravity in the middle improves stability and enables a variety of flying behaviors.

But it also comes at a price–the high complexity of controlling such an aircraft makes it almost impossible to manually control each individual motor's thrust. So, most commercial quadcopters try to simplify the flying controls by accepting a single thrust magnitude and yaw/pitch/roll controls, making it much more intuitive and fun.

The next step in this evolution is to enable quadcopters to autonomously achieve desired control behaviors such as takeoff and landing. You could design these controls with a classic approach (say, by implementing PID controllers). Or, you can use reinforcement learning to build agents that can learn these behaviors on their own. This is what you are going to do in this project!

## Project Highlights

In this project, you will design an agent that can fly a quadcopter, and then train it using a reinforcement learning algorithm of your choice! Try to apply the techniques you have learnt about Reinforcement Learning to find out what works best, but also feel free to come up with innovative ideas and test them.

 Note that getting a reinforcement learning agent to learn what you actually want it to learn can be hard, and very time consuming.
 
 ### Agent Design
 for this project we will use Deep Deterministic Policy Gradients or DDPG algorithm to work with continuous state and action spaces. It is actually an actor-critic method, but the key idea is that the underlying policy function used is deterministic in nature, with some noise added in externally to produce the desired stochasticity in actions taken.
 The two main components of the algorithm, the actor and critic networks can be implemented using most modern deep learning libraries, such as Keras or TensorFlow.
 
Note that we will need two copies of each model(actor and critic) - one local and one target. This is an extension of the "Fixed Q Targets" technique from Deep Q-Learning, and is used to decouple the parameters being updated from the ones that are producing target values

Note2 - After training over a batch of experiences, we could just copy our newly learned weights (from the local model) to the target model. However, individual batches can introduce a lot of variance into the process, so it's better to perform a soft update, controlled by the parameter tau

Finally we need for all this to work properly is an appropriate noise model explained below.
 
### Ornstein–Uhlenbeck Noise
We'll use a specific noise process that has some desired properties, called the Ornstein–Uhlenbeck process. It essentially generates random samples from a Gaussian (Normal) distribution, but each sample affects the next one such that two consecutive samples are more likely to be closer together than further apart. In this sense, the process in Markovian in nature.

 Why is this relevant to us? We could just sample from Gaussian distribution, couldn't we? Yes, but remember that we want to use this process to add some noise to our actions, in order to encourage exploratory behavior. And since our actions translate to force and torque being applied to a quadcopter, we want consecutive actions to not vary wildly. Otherwise, we may not actually get anywhere! Imagine flicking a controller up-down, left-right randomly!

Besides the temporally correlated nature of samples, the other nice thing about the OU process is that it tends to settle down close to the specified mean over time. When used to generate noise, we can specify a mean of zero, and that will have the effect of reducing exploration as we make progress on learning the task.

## Project Instructions

1. Clone the repository and navigate to the downloaded folder.
```
git clone https://github.com/Akshat2127/Deep_RL_Quadcopter_Controller.git
cd Deep_RL_Quadcopter_Controller
```

2. Make sure to provide requirements.txt(depending on the enviornment you're working in like mac, linux or windows or Aws EC2, and whether using GPU or not) file to include a complete list of pip packages needed to run your project.

3. Take a look at the files in the directory to better understand the structure of the project.

   task.py: Task (environment) that defines the goal and provides feedback to the agent.

    agents/: Folder containing reinforcement learning agents.
  
        policy_search.py: file where the optimal policy will be developed.
 
        agent.py: file where agent is developed.
  
        DDPGActor.py - here we will develop our DDPG Actor
  
        DDPGAgent.py - here we will develop our DDPG Agent
  
        DDPGCritic.py - here we will develop our DDPG Critic
  
        OUNoise.py - file with a sample implementation of the Ornstein-Uhlenbeck process
  
        ReplyaBuffer.py - file to develop a replay memory or buffer to store and recall experience tuples.
  
    physics_sim.py: This file contains the simulator for the quadcopter. DO NOT MODIFY THIS FILE.

    For this project, you will define your own task in task.py. 

    You will also design a reinforcement learning agent in agent.py to complete your chosen task.

4. (Optional) __If you plan to install TensorFlow with GPU support on your local machine__, follow [the guide](https://www.tensorflow.org/install/) to install the necessary NVIDIA software on your system.  If you are using an EC2 GPU instance, you can skip this step.

5. Create and activate a new environment.

    conda create -n quadcop python=3.6 matplotlib numpy pandas
    
    source activate quadcop
    
    Create an IPython kernel for the quadcop environment.
    
    python -m ipykernel install --user --name quadcop --display-name "quadcop"
    
    install your requirements from the requirements.txt

6. Switch [Keras backend](https://keras.io/backend/) to TensorFlow.
	- __Linux__ or __Mac__: 
		```
		KERAS_BACKEND=tensorflow python -c "from keras import backend"
		```
	- __Windows__: 
		```
		set KERAS_BACKEND=tensorflow
		python -c "from keras import backend"
		```
    
7. Open the notebook.

    jupyter notebook Quadcopter_Project.ipynb
    
    Before running code, change the kernel to match the quadcop environment by using the drop-down menu (Kernel > Change kernel > quadcop). Then, follow the instructions in the notebook.

You may likely need to install more pip packages to complete this project. 
