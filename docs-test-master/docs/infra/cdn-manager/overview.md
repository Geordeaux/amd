---
id: overview
title: Overview
---
import Mermaid from '@theme/Mermaid'

## 进入每级目录

 
<Mermaid chart={`
graph TD
  A[输入路径获取目录信息] --> B{当前目录是否是根目录}
  B --> |Y| D[返回列表信息]
  B -->|N| F[返回列表信息, 返回上级路径信息]
  D --> E[显示列表]
  F --> E[显示列表]
`}/>


## 新增文件


 
<Mermaid chart={`
  graph TD
    A[输入新增文件信息] --> B{当前目录是否存在该文件}
    B --> |Y| D[调用接口,新增文件]
    B -->|N| F[提示用户文件已存在,建议改名]
    F --> D
    D --> O[更新显示的列表]
`}/>



## 更新文件

 
<Mermaid chart={`
  graph TD
    A[输入更新文件信息] --> B{当前目录是否存在该文件}
    B --> |Y| D[向用户确认是否更新]
    B -->|N| F[走新增逻辑]
    D --> E{用户是否确认更新}
    E --> |N| Z[取消操作]
    E --> |Y| G[调用接口,更新文件]
    F --> R[返回接口操作结果]
    G --> R
    R --> O[更新显示的列表]
`}/>


## 删除文件

 
<Mermaid chart={`
  graph TD
    A[输入删除文件信息] --> B{当前目录是否存在该文件}
    B --> |Y| D[向用户确认是否删除]
    D --> E[用户是否确认删除]
    E --> |N| Z[取消操作]
    E --> |Y| G[调用接口,更新文件]
    G --> R[返回接口操作结果]
    R --> O[更新显示的列表]
`}/>





## 显示

- 上级列表, 显示 .. 可点击跳转
- 图片,显示略缩图
- 其它文件, 显示文件基础信息
- 文件夹, 显示目录名, 可点击跳转



## github 接口

1. 获取 path 内的文件
    `GET /repos/:owner/:repo/contents/:path`
    
2. 新增文件,更新文件
    `PUT /repos/:owner/:repo/contents/:path`
    ```
        {
          "message": "my commit message",
          "committer": {
            "name": "Monalisa Octocat",
            "email": "octocat@github.com"
          },
          "content": "bXkgbmV3IGZpbGUgY29udGVudHM=",
          "sha": "329688480d39049927147c162b9d2deaf885005f"
        }
    ```


3. 删除文件
    `DELETE /repos/:owner/:repo/contents/:path`
    ```
        {
          "message": "my commit message",
          "committer": {
            "name": "Monalisa Octocat",
            "email": "octocat@github.com"
          },
          "sha": "329688480d39049927147c162b9d2deaf885005f"
        }
    ```
