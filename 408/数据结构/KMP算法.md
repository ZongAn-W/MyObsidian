```c
int Index_KMP(SString S,SString T,int next[]){
    int i=1,j=1;
    //int next[T.length+1]; get_next(T, next);//求next数组
    while(i<=S.length && j<=T.length){
        if(j==0||S.ch[i]==T.ch[j]){
            ++i;++j; //继续比较后继字符
        }
        else{
            j=next[j];
        }
    }
    if(j>T.length) return i-T.length;
    else return 0;
}
```

在KMP算法中，最坏的时间复杂度为$O(m+n)$，其中求next数组的时间复杂度为$O(m)$。模式匹配过程最坏时间复杂度为$O(n)$。
