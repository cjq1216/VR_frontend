# 问卷

问卷所需数据及接口

持续更新

## 供参考表结构

### 问卷表

 问卷id | 问卷名 | 可用性 | 总作答人数 | 问卷描述
:------:|:------:|:------:|:----------:|:----:
1 | test 1 | 可用（1） | 18 | 这是问卷test1    
2 | test 2 | 不可用（0） | 13 | 这是问卷test2

### 题目表

 问卷id | 题目id | 题目类型 | 题目 | 选项 | 选择人数 
 :-----:|:------:|:--------:|:----:|:----:|:--------:
 1 | 1 | 单选题（1） | 选择一个 | A | 10 
 1 | 1 | 单选题（1） | 选择一个 | B | 8 
 1 | 2 | 多选题（2） | 选择多个 | A | 10 
 1 | 2 | 多选题（2） | 选择多个 | B | 10 
 1 | 2 | 多选题（2） | 选择多个 | C | 8 
 1 | 3 | 问答题（3） | 回答问题 |  |  
 2 | 1 | 单选题（1） | 选择一个 | A | 10 
 2 | 1 | 单选题（1） | 选择一个 | B | 3 
 2 | 2 | 多选题（2） | 选择多个 | A | 10 
 2 | 2 | 多选题（2） | 选择多个 | B | 10 
 2 | 2 | 多选题（2） | 选择多个 | C | 3 
 2 | 3 | 问答题（3） | 回答问题 |  |   
 
### 作答表

问卷id | 题目id | 选项 | 回答内容 | 用户名 | 用户IP地址
:-----:|:------:|:----:|:--------:|:------:|:----------:
1 | 1 | A | |  user1 | 1.1.1.1
... | ... | | ... | ... | ... | ...
1 | 1 | A | | user10 | 1.1.1.10
1 | 1 | B | | user11 | 1.1.1.11
... | ... | ... |  ... | ... | ...
1 | 1 | B | | user18 | 1.1.1.18
1 | 2 | A | | user1 | 1.1.1.1
... | ... | ... | ... | ... | ...
1 | 2 | A | | user10 | 1.1.1.10
1 | 2 | B | | user1 | 1.1.1.1
... | ... | ... | ... | ... | ...
1 | 2 | B | | user10 | 1.1.1.10
1 | 2 | C | | user1 | 1.1.1.1
... | ... | ... | ... | ... | ...
1 | 2 | C | | user8 | 1.1.1.8
1 | 3 | | 这是我的问答 | user1 | 1.1.1.1
1 | 3 | | 这是你的问答 | user2 | 1.1.1.2
... | ... | ... | ... | ... | ...
1 | 3 | | 这是他的问答 | user18 | 1.1.1.18
2 | 1 | A | |  user1 | 1.1.1.1
... | ... | | ... | ... | ... | ...
2 | 1 | A | | user10 | 1.1.1.10
2 | 1 | B | | user11 | 1.1.1.11
... | ... | ... |  ... | ... | ...
2 | 1 | B | | user13 | 1.1.1.13
2 | 2 | A | | user1 | 1.1.1.1
... | ... | ... | ... | ... | ...
2 | 2 | A | | user10 | 1.1.1.10
2 | 2 | B | | user1 | 1.1.1.1
... | ... | ... | ... | ... | ...
2 | 2 | B | | user10 | 1.1.1.10
2 | 2 | C | | user1 | 1.1.1.1
... | ... | ... | ... | ... | ...
2 | 2 | C | | user3 | 1.1.1.3
2 | 3 | | 这是我的问答 | user1 | 1.1.1.1
2 | 3 | | 这是你的问答 | user2 | 1.1.1.2
... | ... | ... | ... | ... | ...
2 | 3 | | 这是他的问答 | user13 | 1.1.1.13
 
## 接口
 
### 查看所有问卷
/question/getAlLQuestionaire

可用性即问卷的开启和关闭使用。

    入参（）

    出参（问卷id:id，问卷名:name，可用性:activated，总回答人数:number，描述:description）
    
### 搜索问卷
/question/getQuestionaire

通过问卷名模糊搜索。

    入参（问卷名的局部全部 q_name_part:string）
    
    出参（问卷id:id，问卷名:name，可用性:activated，总回答人数:number，描述:description）

### 根据问卷id找出所有题目
/question/getQuestions

    入参（问卷id q_id:int）
    
    出参（quests:[
                 {
                   q_id:1,
                   e_id:1,
                   question:'单选题1',
                   type:1,
                   subs:[
                     {choices:'A选项'},
                     {choices:'B选项'},
                     {choices:'C选项'}
                   ],
                 },
                 {
                   q_id:1,
                   e_id:2,
                   question:'多选题',
                   type:2,
                   subs:[
                     {choices:'A选项A'},
                     {choices:'B选项B'},
                     {choices:'C选项C'}
                   ],
                 },
                 {
                   q_id:1,
                   e_id:3,
                   question:'问答题1',
                   type:3,
                 },
               ],）
    
### 提交作答结果
/question/giveAnswer

每成功执行一次此方法，对应题目的“总作答人数”+1。若是单选/多选题，则作答表添加一条记录且题目表中选项所对应的“选择人数”+1；若是问答题，则新添加一条记录。

    入参（answers:[
                 {
                   "q_id": "1",
                   "e_id": 1,
                   "e_sel": "B",
                   "e_con": "",
                   "u_name": "Cjunqi",
                   "u_ip": "1.1.1.1"
                 },
                 {
                   "q_id": "1",
                   "e_id": 2,
                   "e_sel": [
                     "A",
                     "B"
                   ],
                   "e_con": "",
                   "u_name": "Cjunqi",
                   "u_ip": "1.1.1.1"
                 },
                 {
                   "q_id": "1",
                   "e_id": 3,
                   "e_sel": "",
                   "e_con": "人头还是让他",
                   "u_name": "Cjunqi",
                   "u_ip": "1.1.1.1"
                 }
               ]）
    
    出参（成功1/失败0）
    
### 创建/更新问卷
/question/createQuestionaire

/question/updateQuestionaire

    入参（问卷id q_id:int，问卷名 q_name:string，问卷描述 q_desc:string，可用性 q_ava:int，题目id e_id:int，题目类型 e_type:int，题目 e_tit:string，选项 e_sel:string）
    
    出参（成功1/失败0）
    
### 删除问卷
/question/deleteQuestionaire

    入参（问卷id q_id:int）
    
    出参（成功1/失败0）

### 查询问卷作答情况
/question/getAnswers

    入参（问卷id q_id:int)
    
    出参（answers:[
                   {
                       quesitonId:1,
                       type:1,
                       question:'单选题1',
                       choice:['a选项','b选项'],
                       ansNum:[30,45],
                       ansContent:[],
                   },
                   {
                       quesitonId:2,
                       type:2,
                       question:'多选题1',
                       choice:['a','b'],
                       ansNum:[25,50],
                       ansContent:[],
                   },
                   {
                       quesitonId:3,
                       type:3,
                       question:'问答题',
                       choice:[],
                       ansNum:[],
                       ansContent:['我的回答','你的回答','他的'],
                   },
               ],）
    
### 设置问卷可用性
/question/setQAva
    
    入参（问卷id：q_id，可用性：q_ava）
    
