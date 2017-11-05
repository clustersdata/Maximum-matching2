# Maximum-matching2

Maximum matching2

//此KM算法，坐标从1开始，记住

#include <iostream>
  
#include <vector>
  
#include <math.h>

using namespace std;

#define M 110

int n;                // X 的大小

int lx[M], ly[M];        // 标号

bool sx[M], sy[M];    // 是否被搜索过

int match[M];        // Y(i) 与 X(match [i]) 匹配

// 从 X(u) 寻找增广道路，找到则返回 true

bool path(int u,int weight[M][M]) {

    sx [u] = true;
    
    
    for (int v = 0; v < n; v ++)
    
        if (!sy [v] && lx[u] + ly [v] == weight [u] [v]) {
        
            sy [v] = true;
            
            if (match [v] == -1 || path(match [v],weight)) {
            
                match [v] = u;
                
                return true;
                
            }
            
        }
        
    return false;
    
}

// 参数 Msum 为 true ，返回最大权匹配，否则最小权匹配

int km(bool Msum,int weight[M][M]) {

    int i, j;
    
    
    if (!Msum) {
    
        for (i = 0; i < n; i ++)
        
            for (j = 0; j < n; j ++)
            
                weight [i] [j] = -weight [i] [j];
                
    }
    
    // 初始化标号
    
    for (i = 0; i < n; i ++) {
    
        lx [i] = -0x1FFFFFFF;
        
        ly [i] = 0;
        
        for (j = 0; j < n; j ++)
        
            if (lx [i] < weight [i] [j])
            
                lx [i] = weight [i] [j];
                
    }  
    
    memset(match, -1, sizeof (match));
    
    for (int u = 0; u < n; u ++)
    
        while (1) {
        
            memset(sx, 0, sizeof (sx));
            
  memset(sy, 0, sizeof (sy));
  
            if (path(u,weight))
            
                break;      
                
            // 修改标号
            
            int dx = 0x7FFFFFFF;
            
            for (i = 0; i < n; i ++)
            
                if (sx [i])
                
                    for (j = 0; j < n; j ++)
                    
                        if(!sy [j])
                        
                            dx = min(lx[i] + ly [j] - weight [i] [j], dx);
                            
            for (i = 0; i < n; i ++) {
            
                if (sx [i])
                
                    lx [i] -= dx;
                    
                if (sy [i])
                
                    ly [i] += dx;
                    
            }
            
        }
        
    
    int sum = 0;
    
    
    for (i = 0; i < n; i ++)
    
        sum += weight [match [i]] [i];
        
    
    if (!Msum) {
    
        sum = -sum;
        
        for (i = 0; i < n; i ++)
        
        
            for (j = 0; j < n; j ++)
            
                weight [i] [j] = -weight [i] [j]; // 如果需要保持 weight [ ] [ ] 原来的值，这里需要将其还原
                
    }
    
    return sum;
    
}


struct Point{

    int r,c;
    
};

int main(){

    int i,j,m;
    
    freopen("in","r",stdin);
    
    int w[M][M];
    
    char c; Point pt;
    
    while(cin >> n >> m && (n!=0 || m!=0)){
    
        vector<Point> vh,vm;
        
        for(i=0;i<n;i++){
        
            getchar();
            
            for(j=0;j<m;j++){
            
                scanf("%c",&c);
                
                if(c=='H'){
                
                    pt.r=i; pt.c=j;
                    
                    vh.push_back(pt);
                    
                }
                
                if(c=='m'){
                
                    pt.r=i;pt.c=j;
                    
                    vm.push_back(pt);
                    
                    
                    
                }
                
            }
            
        }
        
        for(i=0;i<vm.size();i++) for(j=0;j<vh.size();j++) w[i][j]=abs(vm[i].r-vh[j].r)+abs(vm[i].c-vh[j].c);
        
        n=vm.size();
        
        cout << km(false,w)<< endl;
        
    }
    
}
