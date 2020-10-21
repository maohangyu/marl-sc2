# Install SMAC + StarCraftII + SMAC-Maps for cooperative MARL testing
refer https://github.com/oxwhirl/smac 

## Installing SMAC
SMAC is WhiRL's environment for research in the field of collaborative multi-agent reinforcement learning (MARL) based on Blizzard's StarCraft II RTS game. SMAC makes use of Blizzard's StarCraft II Machine Learning API and DeepMind's PySC2 to provide a convenient interface for autonomous agents to interact with StarCraft II, getting observations and performing actions. Unlike the PySC2, SMAC concentrates on decentralised micromanagement scenarios, where each unit of the game is controlled by an individual RL agent.   

You can install SMAC by using the following command:  
$ pip install git+https://github.com/oxwhirl/smac.git 

Alternatively, you can clone the SMAC repository and then install smac with its dependencies:  
$ git clone https://github.com/oxwhirl/smac.git  
$ pip install smac/  

You may also need to upgrade pip: pip install --upgrade pip for the install to work.  

## Installing StarCraftII
SMAC is based on the full game of StarCraft II (versions >= 3.16.1). To install the game, follow the commands bellow.  

By default, the game is expected to be in ~/StarCraftII/ directory, so run the following command in the ~ directory:  
$ wget http://blzdistsc2-a.akamaihd.net/Linux/SC2.4.10.zip  
$ unzip -P iagreetotheeula SC2.4.10.zip  
this will result in a ~/StarCraftII directory  

Note that do NOT run the rm command, we will use it in other machine:  
$ rm -rf SC2.4.10.zip  

## Adding SMAC maps
$ wget https://github.com/oxwhirl/smac/releases/download/v0.1-beta1/SMAC_Maps.zip   // if having errors, just download it manually  
$ unzip SMAC_Maps.zip  
$ mv SMAC_Maps ~/StarCraftII/Maps  

Note that do NOT run the rm command, we will use it in other machine:  
$ rm -rf SMAC_Maps.zip  





# Check the installation

## List the maps
To see the list of SMAC maps, together with the number of ally and enemy units and episode limit, run:  
$ python -m smac.bin.map_list  

## Testing SMAC
Please run the following command to make sure that smac and its maps are properly installed.  
$ python -m smac.examples.random_agents  

## Watch a replay
You can watch saved replays by running:  
$ python -m pysc2.bin.play --norender --rgb_minimap_size 0 --replay <path-to-replay>  
This works for any replay as long as the map can be found by the game.  
For more information, please refer to [PySC2 documentation](https://github.com/deepmind/pysc2).  





# Code Example
Below is a small code example which illustrates how SMAC can be used. Here, individual agents execute random policies after receiving the observations and global state from the environment.  

        from smac.env import StarCraft2Env
        import numpy as np

        def main():
            env = StarCraft2Env(map_name="8m")
            env_info = env.get_env_info()

            n_actions = env_info["n_actions"]
            n_agents = env_info["n_agents"]

            n_episodes = 10

            for e in range(n_episodes):
                env.reset()
                terminated = False
                episode_reward = 0

                while not terminated:
                    obs = env.get_obs()
                    state = env.get_state()

                    actions = []
                    for agent_id in range(n_agents):
                        avail_actions = env.get_avail_agent_actions(agent_id)
                        avail_actions_ind = np.nonzero(avail_actions)[0]
                        action = np.random.choice(avail_actions_ind)
                        actions.append(action)

                    reward, terminated, _ = env.step(actions)
                    episode_reward += reward

                print("Total reward in episode {} = {}".format(e, episode_reward))

            env.close()





# Documentation
For the detailed description of the environment, read the [SMAC documentation](https://github.com/oxwhirl/smac/blob/master/docs/smac.md).  
The initial results of our experiments using SMAC can be found in the accompanying paper.  





# Citing SMAC
If you use SMAC in your research, please cite the SMAC paper.  

M. Samvelyan, T. Rashid, C. Schroeder de Witt, G. Farquhar, N. Nardelli, T.G.J. Rudner, C.-M. Hung, P.H.S. Torr, J. Foerster, S. Whiteson. The StarCraft Multi-Agent Challenge, CoRR abs/1902.04043, 2019.  

In BibTeX format:  

        @article{samvelyan19smac,
          title = {{The} {StarCraft} {Multi}-{Agent} {Challenge}},
          author = {Mikayel Samvelyan and Tabish Rashid and Christian Schroeder de Witt and Gregory Farquhar and Nantas Nardelli and Tim G. J. Rudner and Chia-Man Hung and Philiph H. S. Torr and Jakob Foerster and Shimon Whiteson},
          journal = {CoRR},
          volume = {abs/1902.04043},
          year = {2019},
        }
