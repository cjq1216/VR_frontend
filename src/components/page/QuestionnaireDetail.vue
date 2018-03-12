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
            <br />


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
        data: function () {
            return {
                hostURL:'/VR',
                q_name:'test',
                examsData:[
                    {
                        e_id:1,
                        e_type:1,
                        e_tit:'这是一道单选题',
                        e_sel:'A选项',
                    },
                    {
                        e_id:1,
                        e_type:1,
                        e_tit:'这是一道单选题',
                        e_sel:'B选项',
                    },
                    {
                        e_id:1,
                        e_type:1,
                        e_tit:'这是一道单选题',
                        e_sel:'C选项',
                    },
                    {
                        e_id:2,
                        e_type:2,
                        e_tit:'这是一道多选题',
                        e_sel:'A选项',
                    },
                    {
                        e_id:2,
                        e_type:2,
                        e_tit:'这是一道多选题',
                        e_sel:'B选项',
                    },
                    {
                        e_id:2,
                        e_type:2,
                        e_tit:'这是一道多选题',
                        e_sel:'C选项',
                    },
                    {
                        e_id:3,
                        e_type:3,
                        e_tit:'这是一道问答题',
                        e_sel:'',
                    },
                    {
                        e_id:4,
                        e_type:3,
                        e_tit:'这是一道问答题',
                        e_sel:'',
                    },
                ]
            }
        },

        mothed:{
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
            getData(str){
                var self = this;
                //self.questList=[];
                self.$axios({
                    url:'/getExamsById' + str,
                    method:'get',
                    baseURL:self.hostURL
                }).then((response)=>{
                    self.examsData = [];
                    self.examsData= response.data;
                }).catch((error)=>{
                    self.$message({
                        type:'info',
                        message:'connect fail'
                    });
                });
            },
            getQuest(str){
                var self = this;
                self.q_name = str;
            }
        },

        mounted(){
            var self= this;
            var user_name=localStorage.getItem("ms_username");
            if(user_name==""){
                this.$router.replace('/login');
            }
            var arr1 = location.href.split('?');
            var arr2 = location.href.split('#');
            var q_id = arr1[1];
            var q_name = arr2[1];
            console.log(q_id);
            self.getData(q_id);
            self.getQuest(q_name);
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