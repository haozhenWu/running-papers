# Distral: Robust Multitask Reinforcement Learning

Yee Whye Teh, Victor Bapst, Wojciech Marian Czarneckit, John Quan, James Kirkpatrick, Raia Hadsell, Nicolas Heess, Razvan Pascanu

DeepMind, London, UK

# Intro

以前的multitask learning有这么几个弊端
+ gradient可能是negatively correlated，导致更加低效的训练
+ 不同的reward schema，很可能使得一个任务dominate其他任务

这里提出来了一个Distral的multitask训练方法，Distill & transfer learning。与共享参数不同，这里提出了使用“distilled” policy，来抓取不同任务之间的共同行为。

DQN中，