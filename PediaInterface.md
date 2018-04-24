# 百科

百科所需数据及接口

持续更新

## 供参考表结构

### 分类表

 类别id | 类别名 
:------:|:------:
1 | 简介 
2 | 发展 
3 | 特征 
4 | 技术 
5 | 应用 

### 词条表

 类别id | 词条id |词条名 | 词条简述 | 词条内容 | 审核状态 |发布时间
 :------:|:------:|:------:|:----:|:----:|:----:|:---:
 1 | 1 | vr简介1 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 1 | 2 | vr简介1 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 2 | 3 | 第一阶段 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 2 | 4 | 第二阶段 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 3 | 5 | 多感知性 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 3 | 6 | 存在感 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 3 | 7 | 交互性 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 4 | 8 | 显示 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 4 | 9 | 声音 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 4 | 10 | 感觉反馈 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 5 | 11 | 医学 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 5 | 12 | 娱乐 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 5 | 13 | 军事 | 暂空 |暂空 | 通过（1） 待审核（0）| 2018年4月15日
 
 ### 报错表
 
  报错id | 词条id | 报错内容| 用户名 | 处理状态
  :------:|:------:|:------:|:----:|:----:
  1 | 2 | 有错 | user1 | 已处理(1) 待处理(0)

## 接口
 
### 查看所有词条
/wikipedia/findAllTerms

    入参（）

    出参（类别id：cId，类别名：className，词条id：id，词条名：termName，词条简述：termAbstract，词条内容：termContent，词条审核状态：verifyState，发布时间：time）
    
### 查看所有类别
/wikipedia/findAllClass
    
    入参（）
    
    出参（类别id：id，类别名：className）
    
### 搜索词条
/wikipedia/findTermsByName

暂时只对词条名进行搜索匹配

    入参（关键词 keyword:string）
    
    出参（类别名:cId，词条id:id，词条名:termName，词条简述:termAbstract，词条内容:termContent，词条审核状态:verifyState，发布时间:time）

### 根据词条id读取词条内容
/wikipedia/findTermById

    入参（词条id l_id:int）
    
    出参（词条id:id，词条名:termName，词条简述: termAbstract，词条内容: termContent，词条审核状态: verifyState，发布时间: time）
    
### 提交报错信息
/wikipedia/providePediaError

将处理状态设为0

    入参（词条id l_id:int，报错内容:string，用户名:string）
    
    出参（成功1/失败0）
    
### 查询报错信息
/wikipedia/findPediaError

    入参（）
    
    出参（报错id:eId，词条id:tId，词条名:termName，报错内容:errorName，用户名：username，处理状态:handleState）
    
### 完成报错处理
/wikipedia/processPediaError

将处理状态设为1

    入参（报错id error_id:int）
    
    出参（）
    
### 添加类别
/wikipedia/createPediaClass
    
    入参（类别名 c_name:string）
    
    出参（成功1/失败0）
    
### 删除类别
/wikipedia/deletePediaClass

将所有与之关联的词条类别设为空，状态设为待审核(0)

    入参（c_id:int）
    
    出参（成功1/失败0）

### 添加词条
/wikipedia/createPediaTerm

将审核状态设为0
    
    入参（类别id c_id：int，词条名 l_name：string，词条简述 l_abstract：string，词条内容 l_con：string）
    
    出参（成功1/失败0）
    
### 更新词条
/wikipedia/updatePediaTerm

将审核状态设为0

    入参（类别id c_id：int，词条id l_id：int，词条名 l_name：string，词条简述 l_abstract：string，词条内容 l_con：string）
        
    出参（成功1/失败0）
    
### 通过词条审核
/wikipedia/verifyTermState

将审核状态设为1
    
    入参（词条id l_id：int）
    
    出参（成功1/失败0）
    
### 删除词条
/wikipedia/deletePediaTerm
    
    入参（词条id l_id：int）
    
    出参（成功1/失败0）