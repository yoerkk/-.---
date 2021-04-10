#include <bits/stdc++.h>
using namespace std;
int n,ans;
string a[22];
int vis [22];

//关于“回溯到某点之后，自动试探下一个方向”这件事，由i和p循环自动决定，无需操作。
void dfs(string x,int s){//带着“上一个元素”，和“目前拼接长度”传递
    ans=max(ans,s);//所有拼接中找出最长的（每一次递归中寻找长度最大值）

    //回溯break之后，从下一个i（单词）开始比对（新的路径），而ans在for之前，即仍保留上一路径的最大长度，
    //所以ans最后结果应当是所有路径，所有动作的最大长度
    //迷宫问题我们希望保留最短路径，但是是“某一路径的”，所以每次回溯，要讲步数减1（同时修改访问标志）
    for(int i=1;i<=n;i++){//枚举所有a[i]和x比对
        int p=1;
        int la=x.length(),lb=a[i].length();
        for(int p=1;p<min(la,lb);p++){
            if(x.substr(la-p)==a[i].substr(0,p)&&vis[i]<2){
                vis[i]++;
                dfs(a[i],s+lb-p);
                vis[i]--;//注意！回溯状态要改回来
                break;
            }
        }
    }
return;
}

int main(){
    cin>>n;
    for(int i=1;i<=n;i++)
        cin>>a[i];
    char t;
    cin>>t;
    for(int i=1;i<=n;i++){
        if(a[i][0]==t){
            vis[i]++;
            dfs(a[i],a[i].length());//第一个字符串开始搜索。（有很多个可以作为第一个单词的，每个遍历）
            vis[i]--;//注意！回溯状态要改回来
        }
    }
    cout<<ans;
    return 0;
}
