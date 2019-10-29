# AlphaGo Zero 中蒙特卡洛树搜索伪代码

```python
def search(s, game, nnet):
	#判断是否为叶子节点，是则返回1或-1
    if game.gameEnded(s): return -game.gameReward(s)  
	#节点不在蒙特卡洛树中
    if s not in visited:
        visited.add(s)              #将该节点加入到MCTS树中
        P[s], v = nnet.predict(s)   #使用神经网络返回 P,V
        return -v
  
    max_u, best_a = -float("inf"), -1
    for a in range(game.getValidActions(s)):
        u = Q[s][a] + c_puct*P[s][a]*sqrt(sum(N[s]))/(1+N[s][a])
        if u>max_u:
            max_u = u
            best_a = a
    a = best_a                   #选择最好的动作
    
    sp = game.nextState(s, a)    #得到下一个状态节点
    v = search(sp, game, nnet)   #递归调用搜索方法

    Q[s][a] = (N[s][a]*Q[s][a] + v)/(N[s][a]+1) #如果直接(Q+V)/N+1的话应该在上面求U时Q/N
    N[s][a] += 1
    return -v
```


