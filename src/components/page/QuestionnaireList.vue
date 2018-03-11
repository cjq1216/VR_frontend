<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item to="/user/questionnairelist"><i class="el-icon-date"></i>满意度调查</el-breadcrumb-item>
                <el-breadcrumb-item>问卷列表</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="questionnaire">
            <div class="my-container">
                <div class="survey-wrapper clearfix">
                    <div class="survey-left pull-left">
                        <div id="search" class="search pull-left">
                            <input name="txtName" type="text" id="txtName" class="searchbox" onfocus="if(value=='请输入问卷名进行搜索'){value='';}" onblur="if(value==''){value='请输入问卷名进行搜索’；}" value="请输入问卷名进行搜索" onkeypress="return searchQ(event);"/>
                            <input type="submit" name="btnSub" id="btnSub" class="search-icon"/>
                        </div>
                    </div>
                </div>
            </div>
            <br />
            <br />
            <div class="survey-folder"></div>

            <!--<p>问卷列表</p>-->
            <ul class="quest-list">
                <li class="quest-item" v-for="quest in questList" @click="questClick(quest)">
                    <a href="javascript:void(0);" class="link-tit" title="">
                        <span class="quest-name">{{quest.q_name}}</span>
                        <span class="quest-num">答卷：{{quest.q_num}}</span>
                    </a>
                </li>
            </ul>

            <!--<hr>-->
            <!--<div class="selector">-->
                <!--<div class="protype_selector">-->
                    <!--<el-select v-model="pro_type.value" placeholder="请选择问卷" @change="sendProType()">-->
                        <!--<el-option v-for="item in pro_type.opts" :key="item.value" :label="item.label" :value="item.value">-->
                        <!--</el-option>-->
                    <!--</el-select>-->
                <!--</div>-->
                <!--&lt;!&ndash;<div class="prosales_selector" >-->
                    <!--<el-select v-model="pro_sales.value" placeholder="请选择产品型号" :disabled="pro_sales_disable">-->
                        <!--<el-option v-for="item in pro_sales.opts" :key="item.value" :label="item.label" :value="item.value">-->
                        <!--</el-option>-->
                    <!--</el-select>-->
                <!--</div>&ndash;&gt;-->
            <!--</div>-->
            <!--<div class='questionbox' v-show="display_box">-->
                <!--<form id="questions">-->
                    <!--<div class="single_question" v-for="(a,index) in answ_data.answers">-->
                        <!--<p style="margin-bottom:10px">{{a.question}}</p>-->
                        <!--<div class="answer" v-if="ques_data.questions[index].questiontype=='single'">-->
                            <!--<label class="left_radio"><el-radio class="radio" v-model="a.answer" label="1">是</el-radio></label>-->
                            <!--<el-radio class="radio" v-model="a.answer" label="2">否 </el-radio>-->
                        <!--</div>-->
                        <!--<div class="essay-answer" v-else-if="ques_data.questions[index].questiontype=='essay'">-->
                            <!--<el-input type="textarea" :row="3" placeholder="请输入内容" v-model="a.answer" resize=none></el-input>-->
                        <!--</div>-->
                    <!--</div>-->
                <!--</form>-->
                <!--<div class="submit_btn">-->
                    <!--<el-button type="primary" @click="sendQuestionaire()">提交问卷</el-button>-->
                <!--</div>-->
            <!--</div>-->
        </div>
     
         
    </div>
</template>

<script>
    export default {
        data: function(){
            return {
                activeName2: 'first',
                allowSubmit:true,
                hostURL:"/VR",
                uid:"1",
                display_box:false,
                questList:[
                    {
                        q_id:1,
                        q_name:'问卷1',
                        q_ava:1,
                        q_num:20,
                    },
                    {
                        q_id:2,
                        q_name:'问卷2',
                        q_ava:1,
                        q_num:18,
                    },
                    {
                        q_id:3,
                        q_name:'问卷3',
                        q_ava:1,
                        q_num:22,
                    },
                ],

                // ques_data:{
                //     questions:[{
                //         id:1,
                //         question:"1.你是谁？",
                //         questiontype:"single"
                //     },{
                //         id:3,
                //         question:"3.你对某个产品有什么建议？",
                //         questiontype:"essay"
                //     }]
                // },
                // answ_data:{
                //     answers:[{
                //         question:1,
                //         user:self.uid,
                //         answer:"1",
                //         producttype:"",
                //         productname:""
                //     },{
                //         question:3,
                //         user:self.uid,
                //         answer:"",
                //         producttype:"",
                //         productname:""
                //     }]
                // },
                //
                // pro_type:{
                //     opts: [{
                //         value: 'AllInOnePc',
                //         label: '一体机'
                //         }, {
                //         value: 'PcheadSet',
                //         label: 'PC头显'
                //         }, {
                //         value: 'MobileBox',
                //         label: '手机盒子'
                //         }],
                //         value: ''
                // },
                // pro_sales:{
                //     opts: [{
                //         value: '',
                //         label: ''
                //         }],
                //         value: ''
                // },
                // pro_sales_disable:true
            }
        },
    
        methods:{
            codeParsing(code) {
                let self = this;
                var msg = (err_title, err_message)=> {
                    self.$notify({
                        title: err_title,
                        message: err_message,
                        type: 'error'
                    });
                };
                switch (code) {
                    case -1:
                        msg('系统错误', '未知错误，请上报管理员');
                        break;
                    case 201:
                        msg('输入域错误', '验证码错误');
                        break;
                    case 300:
                        msg('输入域错误', '邮箱或密码错误');
                        break;
                    case 301:
                        msg('权限问题', '用户已禁用，请联系管理员');
                        break;
                    case 302:
                        msg('权限问题', '用户未激活，请去邮箱激活用户');
                        break;
                    case 303:
                        msg('注册问题', '邮箱已占用，请更改邮箱');
                        break;
                    case 304:
                        msg('注册问题', '昵称已占用，请更改昵称');
                        break;
                    case 401:
                        msg('权限问题', '用户无权访问，请联系管理员');
                        break;
                    case 402:
                        msg('操作错误', '删除错误,请刷新重试');
                        break;
                    case 415:
                        msg('操作错误', '文件类型错误，请上传正确文件类型');
                        break;
                    case 500:
                        msg('系统错误', '未知错误，请上报管理员');
                        break;
                    case 600:
                        msg('TIME_OUT', '访问超时，请检查网络连接');
                        break;
                    case 700:
                        msg('激活错误', '非法激活链接，请联系管理员');
                        break;
                    case 800:
                        msg('激活错误', '用户已被激活，请直接登录');
                        break;
                    case 1000:
                        msg('系统错误', '参数错误，上报管理员');
                        break;
                    case 1001:
                        msg('权限问题', '用户未登录，请重新登录');
                        break;
                    case 1002:
                        msg('系统错误', '参数错误，上报管理员');
                        break;
                    default:
                        break;
                }
            },
            getQuest(){
                var self = this;
                //self.questList=[];
                self.$axios({
                    url:'/getAllQuestionnaire',
                    method:'get',
                    baseURL:self.hostURL
                }).then((response)=>{
                    self.questList = [];
                    self.questList= response.data;
                }).catch((error)=>{
                    self.$message({
                        type:'info',
                        message:'connect fail'
                    });
                });
            },
            questClick(quest){
                var self=this;
                console.log("go to questionnaierdetail!");
                console.log(quest);
                self.$router.push('/user/questionnairedetail?'+quest.q_id);
            }
            // getData(id){
            //     var self = this;
            //     self.ques_data.questions=[];
            //     self.answ_data.answers=[];
            //     self.pro_type.opts=[];
            //     self.$axios({
            //         url:'/Question',
            //         method:'get',
            //         baseURL: self.hostURL
            //     }).then((response)=>{
            //         console.log(response.data);
            //         self.pro_type.opts=[];
            //         for(var i=0;i<response.data.length;i++){
            //             self.pro_type.opts.push({value:response.data[i],label:response.data[i]});
            //         }
            //     }).catch((error)=>{
            //         console.log(error);
            //     });
            // },
            // sendQuestionaire(){
            //     var self = this;
            //     var send=true;
            //     for(var i=0;i<self.answ_data.answers.length;i++){
            //         if(self.answ_data.answers[i].answer==""){
            //             send=false;
            //             break;
            //         }
            //     }
            //     if(send&&self.pro_type.value!=""){
            //         for(var i=0;i<self.answ_data.answers.length;i++){
            //             self.answ_data.answers[i].producttype=self.pro_type.value;
            //             // self.answ_data.answers[i].productname=self.pro_sales.value;
            //         }
            //
            //         self.$axios({
            //             url:'/Answer',
            //             method:'post',
            //             baseURL: self.hostURL,
            //             data: self.answ_data.answers
            //         }).then((response)=>{
            //             localStorage.setItem('pro_type',self.pro_type.value);
            //             // localStorage.setItem('pro_sale',self.pro_sales.value);
            //             self.$router.replace('/user/questionaire/statistic');
            //         }).catch((error)=>{
            //             console.log(error);
            //         });
            //     }else{
            //         self.$message('还有内容未填写！');
            //     }
            //
            // },
            // sendProType(){
            //     var self=this;
            //     self.ques_data.questions=[];
            //     self.answ_data.answers=[];
            //     self.$axios({
            //         url:'/Question/type?type='+self.pro_type.value,
            //         method:'get',
            //         baseURL:self.hostURL
            //     }).then(response=>{
            //         self.ques_data.questions= response.data;
            //         self.answ_data.answers=[];
            //         for(var i=0;i<self.ques_data.questions.length;i++){
            //             self.answ_data.answers.push({
            //                 question:self.ques_data.questions[i].question,
            //                 user:localStorage.getItem('ms_userid'),
            //                 answer:"",
            //                 producttype:""
            //             })
            //         }
            //         self.display_box=true;
            //     });
            // }
        },
        mounted(){
            var self= this;
            var user_name=localStorage.getItem("ms_username");
            if(user_name==""){
                this.$router.replace('/login');
            }
            // var arr = location.href.split('?');
            // var id = arr[1];
            // console.log(id);
            //self.getData();
            self.getQuest();
        }
    }
</script>

<style>
.crumbs{
    text-decoration: none;
}
.bread{
    font-size: 16px;
}
#questions{
    margin-left: 30px;
}
.questionnaire {
    position: relative;
}
.my-container {
    width: 964px;
    margin: 0 auto;
}
.survey-wrapper {
    position: relative;
    margin: 40px 0;
}
.clearfix {
    zoom: 1;
}
.survey-left {
    width: 890px;
}
.pull-left {
    float: left !important;
}
.search {
    position: relative;
}
input {
    width: 300px;
    height: 25px;
    line-height: 1.6;
    padding: 8px 0 8px 16px;
    font-size: 13px;
    color: #b2b2b2;
    border-radius: 100px;
    border: 1px solid #30a6f5;
}
.search-icon {
    position: absolute;
    top: 11px;
    right: 11px;
    width: 25px;
    height: 20px;
    margin: 0;
    border: none;
    cursor: pointer;
    background: url("https://www.wjx.cn/images/newimg/pic-1/search.png") no-repeat center;
}
.survey-folder {
    margin: 20px 0;
    border-top: 2px solid #f5f7fa;
}
ul {
    list-style: none;
    display: block;
    -webkit-margin-before: 1em;
    -webkit-margin-after: 1em;
    -webkit-margin-start: 0px;
    -webkit-margin-end: 0px;
    -webkit-padding-start: 40px;
}
li {
    list-style-type:none;
    padding-bottom:10px;
    border-bottom: 1px solid #ccc;
}
a {
    text-decoration: none;
}
.quest-item{
    margin-bottom: 24px;
    overflow: hidden;
}
.quest-list .quest-item .link-tit {
    color: #e9c06c;
}
.quest-list .quest-item img{
    display: block;
    float:left;
    margin-right: 20px;
    width:200px;

}
.quest-list .quest-item .quest-name{
    font-size: 22px;
    font-weight: lighter;
    color: #3e3e3e;
    line-height: 50px;
    height: 50px;
    overflow: hidden;
}
.quest-list .quest-item .quest-num {
    float: right;
    height: 60px;
    overflow: hidden;
    font-size: 14px;
    color: #898989;
    line-height: 1.5;
}
.questionnaire p{
    font-size: 18px;
    margin-top: 30px;
}
.questionnaire hr{
    width: 72.7%;
}
.left_radio{
    margin-right: 200px;
    margin-left: 30px;
}
.essay-answer{
    width: 400px;
    /*font-size: 15px;*/
}
.submit_btn{
    margin-left: 180px;
    margin-top: 30px
}
.selector{
    width:700px;
    margin-top: 20px;
    display: flex;
}
.protype_selector{
    margin-left: 50px;
    float:left;
}
.prosales_selector{
    margin-left: 40px;
    flex: 1;
}
.questionbox{
    padding: 28px;
    width: 800px;
    border: 1px solid #E1E1E1;
    box-shadow: 0 0 2px rgba(0,0,0,.1);
    margin-top: 20px;
}
</style>