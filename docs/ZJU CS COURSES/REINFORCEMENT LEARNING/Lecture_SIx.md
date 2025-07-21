# Lecture Six: Policy Gradient Methods

## 课程回顾

* 蒙特卡洛方法：
> $V(s_t)=V(s_t)+\alpha\left[G_t-V(s_t)\right]$

* 时序差分方法：
> $V(s_t)=V(s_t)+\alpha\left[R_{t+1}+\gammaV(s_{t+1})-V(s_t)\right]$

* SARSA方法：
> $Q(s_t,a_t)=Q(s_t,a_t)+\alpha\left[R_{t+1}+\gamma Q(s_{t+1},a_{t+1})-Q(s_t,a_t)\right]$

* Q-learning方法：
> $Q(s_t,a_t)=Q(s_t,a_t)+\alpha\left[R_{t+1}+\gamma \max_{a'}Q(s_{t+1},a')-Q(s_t,a_t)\right]$

