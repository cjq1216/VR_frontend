<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item to="/user/questionnairelist"><i class="el-icon-date"></i>满意度调查</el-breadcrumb-item>
                <el-breadcrumb-item>问卷</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="questionnaire-box">
            <div class="questionnaire-title">{{q_name}}</div>


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
        <br/>

        <div class="question">
            <form id="questions">
                <div class="quest" v-for="(a,index) in answer" >
                    <div :id="a.e_id">
                        <h4>{{a.e_id}}.&nbsp;{{examsData[index].question}}</h4>
                        <div class="single" v-if="examsData[index].type===1">
                            <div v-for="tmpExam in examsData">
                                <div v-if="tmpExam.questionId===a.e_id">
                                    <input style="width: 20px;" type="radio" :value="tmpExam.choices" :name="a.e_id">&nbsp;{{tmpExam.choices}}
                                </div>
                            </div>
                        </div>
                        <div class="plural" v-if="examsData[index].type===2">
                            <div v-for="tmpExam in examsData">
                                <div v-if="tmpExam.questionId===a.e_id">
                                    <input style="width: 20px;" type="checkbox" :value="tmpExam.choices" :name="a.e_id">&nbsp;{{tmpExam.choices}}
                                </div>
                            </div>
                        </div>
                        <div class="essay" v-if="examsData[index].type===3">
                            <br/>
                            <textarea rows="4" cols="50" v-model="a.e_con" placeholder="请输入内容"></textarea>
                        </div>
                    </div>
                    <br/>
                </div>
            </form>
            <div class="submit_btn">
            <el-button type="primary" @click="sendQuestionaire()">提交问卷</el-button>
            </div>


        </div>

    </div>
</template>


<script>
    export default {
        data: function () {
            return {
                hostURL:'/VR',
                q_name:'test',
                examsData:[
                    {
                        questionId:1,
                        type:1,
                        question:'这是一道单选题',
                        choices:'A选项',
                    },
                    {
                        questionId:1,
                        type:1,
                        question:'这是一道单选题',
                        choices:'A选项',
                    },
                    {
                        questionId:1,
                        type:1,
                        question:'这是一道单选题',
                        choices:'A选项',
                    },
                    {
                        questionId:2,
                        type:2,
                        question:'这是一道多选题',
                        choices:'A选项',
                    },
                    {
                        questionId:2,
                        type:2,
                        question:'这是一道多选题',
                        choices:'B选项',
                    },
                    {
                        questionId:2,
                        type:2,
                        question:'这是一道多选题',
                        choices:'C选项',
                    },
                    {
                        questionId:3,
                        type:3,
                        question:'这是一道问答题',
                        choices:'',
                    },
                    {
                        questionId:4,
                        type:3,
                        question:'这是一道问答题',
                        choices:'',
                    },
                ],
                answer:[
                    {
                        q_id:1,
                        e_id:1,
                        e_sel:'A',
                        e_con:'lalala',
                        u_name:localStorage.getItem('ms_username'),
                        u_ip:'1.1.1.1',
                    },
                ],
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
            getData(q_id){
                var self = this;
                //self.questList=[];
                self.$axios({
                    url:'/question/getQuestions',
                    method:'post',
                    baseURL:self.hostURL,
                    data:{
                        q_id:q_id,
                    }
                }).then((response)=>{
                    self.examsData = [];
                    self.examsData= response.data;
                    self.answer = [];
                    //console.log(returnCitySN["cip"]+','+returnCitySN["cname"]);
                    for(var i=0;i<self.examsData.length;i++){
                        self.answer.push({
                            q_id:q_id,
                            e_id:self.examsData[i].questionId,
                            u_name:localStorage.getItem('ms_username'),
                            e_sel:self.examsData[i].choices,
                            e_con:'',
                            u_ip:'1.1.1.1',
                        })
                    };
                }).catch((error)=>{
                    self.$message({
                        type:'info',
                        message:'无法获取问卷，请重试'
                    });
                });
            },

            sendQuestionaire(){
                var self = this;
                var send=true;
                for(var i=0;i<self.answer.length;i++){
                    if(self.answer[i].answer==""){
                        send=false;
                        break;
                    }
                }
                if(send){
                    for(var i=0;i<self.answer.length;i++){
                        self.answer[i].u_name=localStorage.getItem('ms_username');
                        self.answer[i].u_ip='1.1.1.2';
                    }
                    console.log(self.answer);
                    self.$axios({
                        url:'/question/giveAnswer',
                        method:'post',
                        baseURL: self.hostURL,
                        data: self.answer
                    }).then((response)=>{
                        //localStorage.setItem('pro_type',self.pro_type.value);
                        // localStorage.setItem('pro_sale',self.pro_sales.value);
                        self.$router.replace('/user/questionnairelist');
                    }).catch((error)=>{
                        console.log(error);
                    });
                }else{
                    self.$message('还有内容未填写！');
                }
            },

        },

        mounted(){
            var self= this;
            var user_name=localStorage.getItem("ms_username");
            if(user_name==""){
                this.$router.replace('/login');
            }
            var tmp1 = location.href.split('?');
            var tmp2 = tmp1[1].split('&');
            var q_id = tmp2[0];
            self.q_name = tmp2[1];
            console.log(q_id);
            console.log(self.q_name);
            self.getData(q_id);
            //console.log(examsData);
            //self.getQuest(q_name);
        },

        name: "questionnaire-detail"
    }
</script>

<style scoped>
.questionnaire-box{
    padding: 28px;
    width: 800px;
    border: 1px solid #E1E1E1;
    box-shadow: 0 0 2px rgba(0,0,0,.1);
}
.questionnaire-title{
    font-size: 22px;
    color: #287D7C;
    line-height: 50px;
    font-weight: normal;
    display: block;
    text-align: center;
}
</style>