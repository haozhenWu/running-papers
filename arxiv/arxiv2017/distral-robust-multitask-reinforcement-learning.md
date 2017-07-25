# Distral: Robust Multitask Reinforcement Learning

Yee Whye Teh, Victor Bapst, Wojciech Marian Czarneckit, John Quan, James Kirkpatrick, Raia Hadsell, Nicolas Heess, Razvan Pascanu

DeepMind, London, UK

# Intro

DQN中，使用DNN作为agent，但是训练需要大量的数据和时间，学习过程也可能是不稳定的。一个不同的方法是通过加入额外辅助的任务，和RL一起训练。

以前的multitask learning有这么几个弊端
+ gradient可能是negatively correlated，导致更加低效的训练
+ 不同的reward schema，很可能使得一个任务dominate其他任务

这里提出来了一个Distral的multitask训练方法，Distill & transfer learning。与共享参数不同，这里提出了使用“distilled” policy，来抓取不同任务之间的共同行为。

# Distral: Distill and Transfer Learning

Distral最核心的是一个shared policy，将task-specific policy中的共同行为和表达提取出来。然后关键步骤是，提取的distilled policy会通过使用KL divergence作为regularization，来指导task-specific policies。