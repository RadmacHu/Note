# Actor 生命周期



[原文链接](http://api.unrealengine.com/CHN/Programming/UnrealArchitecture/Actors/ActorLifecycle/index.html)



## Actor 生成路径

Actor四种生成路径：

1. 编辑器中运行启动
2. 磁盘中加载
3. SpawnActor生成
4. SpawActorDeferred生成



### 编辑器中运行

```flow
st=>start: 编辑器开始 
postDuplicateCall=>operation: PostDuplicateCall调用
end=>end: 结束

st->postDuplicateCall->end
```

编辑器运行的时候 ，Actor却并非从磁盘中加载，而是从编辑器中复制而来。







