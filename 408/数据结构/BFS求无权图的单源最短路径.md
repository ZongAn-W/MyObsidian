## BFS求无权图的单源最短路径

> [!warning] 注意
> BFS广度优先算法只适用于无权图的最短路径求法，或所有边的权值都相等的图。

```c
void BFS_MIN_Distance(Graph G, int u){
	for(i=1;i≤G.vexnum;++i) {
		d[i]=∞; //初始化路径长度，d[i]表示从u到i结点的最短路径
		path[i]=-1;//最短路径从哪个顶点过来
	}
	d[u]=0;
	visited[u]=TRUE;
	Enqueue(Q,u);
	while(!isEmpty(Q)){//BFS算法
		DeQueue(Q,u); //顶点u出队列
		for(w=FirstNeighbor(G,u); w>=0; w=NextNeighbor(G,u,w))
		//检测u的所有邻接结点
			if(!visited[w]){ //w为u的尚未访问的邻接顶点
				d[w]=d[u]+1; //路径长度加1
				path[w]=u;//最短路径应从u到w
				visited[w]=TRUE; //对w做已访问标记
				EnQueue(Q,w); //顶点w入队列
			}
	}
}
```

## 相关
- [[最短路径]]
- [[广度优先遍历BFS]]
