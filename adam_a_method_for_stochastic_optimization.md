# Adam: A Method For Stochastic Optimization

Diederik Pm. Kingma, University of Amsterdam

Jummy Lei Ba, University of Toronto

# Intro

Adam,是一个高效的stochastic优化方法，只使用了一阶导数。

算法流程大概是这样

$$\text{Require :}\\
\alpha \text{: step size}\\
\beta_1, \beta_2 \in [0, 1) \text{: exponential decay rates for the moment estimates}\\
f(\theta) \text{: stochastic objective function with parameters } \theta \\
\theta_0 \text{: initial parameter vector}\\
m_0 \leftarrow 0 \text{ (initialize 1st moment vector)}\\
v_0 \leftarrow 0 \text{ (initialize 2nd moment vector)}\\
t \leftarrow 0 \text{ (initial timestep)}\\
\\
\text{while } \theta_t \text{ not converged do}\\
t \leftarrow t + 1\\
g_t \leftarrow \bigtriangledown_\theta f_t(\theta_t -1)\\
m_t \leftarrow \beta_1 \cdot m_{t-1} + (1-\beta_1) \cdot g_t\\
v_t \leftarrow \beta_2 \cdot v_{t-1} + (1-\beta_2) \cdot g_t^2\\
\hat m_t \leftarrow m_t / (1-\beta_1^t)\\
\hat v_t \leftarrow v_t / (1-\beta_2^t)\\
\theta_t \leftarrow \theta_{t-1} - \alpha \cdot \hat m_t / (\sqrt{\hat v_t} + \epsilon)\\
\text{end while}\\
\text{return } \theta_t
$$

