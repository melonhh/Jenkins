# 基础概念
我们需要理解Jenkins为流水线所提供的开发环境。（运行流水线系统的系统以及用于创建、执行、监视流水线的接口）

## [流水线的两种语法类型](/toType)
## 流水线运行系统
## 流水线的基本结构
## Jenkins为流水开发和运行提供的支持环境

## 两种语法类型
脚本式和声明式

```shell script
// 脚本式流水线
node('worker_node1') {
  stage('Source') {
    // 从Git上获取代码
    git '...'    
  }
}
```
受限于Groovy语言和环境  

```shell script
// 声明式流水线
pipeline {
  agent {label 'worker_node1'}
  stages {
    stage('Source') {
      steps {
            // 从Git上获取代码  
      }
    }
    stage('Compile') {
      steps {
            // 运行Gradle进行编译和单元测试
      }
    }
  }
}
```
声明式流水的优点：  
    1. 结构化
    2. 可读性高
    3. 可通过Blue Ocean自动生成
    
## 流水线运行系统
主节点（master）、节点（node）、代理节点（agent）、执行器（executor）

### 主节点  
主节点是一个Jenkins实例（instance）的主要控制系统。能够访问所有Jenkins配置选项和任务（job）列表。如果没有指定其他系统（system），它也是默认的任务执行节点。

注意点：
* 不推荐在主节点上执行高负载任务
* 但凡在主节点上执行的任务，都有权限访问所有的数据、配置、操作，这存在潜在安全风险
* 也不应该执行任何包含潜在阻塞的操作

### 节点
节点是一个基础概念，代表了任何可以执行Jenkins任务的系统。(包括主节点和代理节点) 节点也可以是容器（比如docker）

### 代理节点
早期版本的Jenkins中，代理节点被称为从节点（slave），其代表了所有非主节点的系统。这类系统由主系统管理，按需分配执行特定任务。  

代理节点在节点上运行。在脚本式流水线中，节点特指一个运行了代理节点的系统，在声明式流水线中，指代一个特定的代理节点来分配节点。

### 执行器
执行器的数量定义了节点并发任务数量。

![Jenkins环境](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.qingtingip.com%2Fh_261979.html&psig=AOvVaw155qCSdaazoLr_5c3SDf9D&ust=1597921762791000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCLDkr82Qp-sCFQAAAAAdAAAAABAX)

## 流水线基本结构
```shell script
node('worker') {
  stage('Source') {
    // 从Git上获取代码
    git '...'    
  }
}
```
1. 节点node  
```shell script
node('worker')

这一行告诉jenkins应该在哪个节点上运行这部分流水线

node() 或 agent any
# 若master配置为默认的执行节点，则会在master上执行任务
# 否则，任意

node('linux && east')
```

2. 阶段
```shell script
stage("Source")

可以将个人设置, DSL命令, 逻辑组合在一个stage闭包中

必须执行name
```

3. 步骤
```shell script
git '...'   

包含了实际的Jenkins DSL命令
```

## 支持环境
