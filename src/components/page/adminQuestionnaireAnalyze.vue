<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item to="/admin/adminQuestionnairelist"><i class="el-icon-edit"></i>问卷管理</el-breadcrumb-item>
                <el-breadcrumb-item>问卷数据分析</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <el-row>
            <el-button class="refresh-btn" type="success" @click="goTolist()">返回</el-button>
        </el-row>
        <div class="questionnaire-box">
            <div class="questionnaire-title">{{questionaireName}}</div>
            <div class="questionnaire-info">总作答人数：{{allNum}}</div>
        </div>
        <br/>
        <div>
            <div v-for="answer in answers">
                <h4 class="question-title">{{answer.quesitonId}}.&nbsp;{{answer.question}}&nbsp;&nbsp;
                    <span v-if="answer.type===1" class="question-type">[单选题]</span>
                    <span v-if="answer.type===2" class="question-type">[多选题]</span>
                    <span v-if="answer.type===3" class="question-type">[问答题]</span>
                </h4>
                <div v-if="answer.type===2 || answer.type===1" :id="answer.quesitonId" style="width:80%;height:400px;"></div>
                <div v-else class="ansCon">
                    <el-table :data="answer.con" stripe style="width: 80%">
                        <el-table-column prop="cont" label="回答"></el-table-column>
                    </el-table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    // 引入 ECharts 主模块
    var echarts = require('echarts/lib/echarts');
    // 引入柱状图
    require('echarts/lib/chart/bar');
    require("echarts/lib/chart/pie");
    require("echarts/lib/chart/line");
    // 引入提示框和标题组件
    require('echarts/lib/component/tooltip');
    require('echarts/lib/component/title');
    require('echarts/lib/component/toolbox');
    require('echarts/lib/component/dataset');

    export default {
        name: "analyze",
        data() {
            return {
                qId:'',
                questionaireName:'问卷1',
                allNum:10,
                hostURL: '/VR',
                answers:[
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
                ],
            }
        },

        methods: {
            goTolist(){
                var self = this;
                console.log("go to list!");
                self.$router.push('/admin/adminQuestionnairelist');
            },
            getQuestionnaireInfo(qid){
                var self = this;
                self.$axios({
                    url:'/question/getAlLQuestionaire',
                    method:'get',
                    baseURL:self.hostURL
                }).then((response)=>{
                    for(let i=0;i<response.data.length;i++){
                        if(response.data[i].id == qid){
                            self.questionaireName = response.data[i].name;
                            self.allNum = response.data[i].number;
                        }
                    }
                    this.drawLine();
                }).catch((error)=>{
                    self.$message({
                        type:'info',
                        message:'无法获取问卷，请重试'
                    });
                });
            },
            getAns(qid){
                this.$axios({
                    url:'/question/getAnswers',
                    method:'post',
                    baseURL:this.hostURL,
                    data:{
                        q_id:qid
                    }
                }).then((response)=>{
                    this.answers=[];
                    this.answers=response.data;
                }).catch((error)=>{
                    this.$message({
                        type:'info',
                        message:'无法获取问卷作答情况，请重试'
                    });
                });
            },
            drawLine() {
                for (let i=0;i<this.answers.length;i++){
                    if(this.answers[i].type === 1 || this.answers[i].type === 2){
                        let mychart = echarts.init(document.getElementById(this.answers[i].quesitonId.toString()),'light');
                        let option1 = {
                            title: {},
                            legend: {},
                            tooltip: {trigger: 'axis'},
                            toolbox: {
                                show: true,
                                feature: {
                                    dataZoom: {
                                        yAxisIndex: 'none'
                                    },
                                    //dataView: {readOnly: true},
                                    magicType: {type: ['bar']},
                                    restore: {},
                                    saveAsImage: {}
                                }
                            },

                            xAxis: {
                                data: []
                            },
                            yAxis: {},
                            series: [{
                                type: 'bar',
                                data: []
                            }]
                        };
                        // 绘制图表
                        mychart.setOption(option1);
                        mychart.setOption({
                            xAxis: {
                                data: this.answers[i].choice
                            },
                            series: [{
                                data: this.answers[i].ansNum
                            }]
                        });
                    } else {
                        this.answers[i].con =[];
                        for (let j=0;j<this.answers[i].ansContent.length;j++){
                            this.answers[i].con.push(
                                {
                                    cont:this.answers[i].ansContent[j]
                                }
                            )
                        }
                        //console.log(this.answers[i]);
                    }
                }
            }

        },

        mounted() {
            var tmp1 = location.href.split('?');
            this.qId = tmp1[1];
            if(this.qId){
                this.getAns(this.qId);
                this.getQuestionnaireInfo(this.qId);
            }
        },
    }
</script>

<style scoped>
    .crumbs{
        margin-bottom: 20px;
    }
    .refresh-btn{
        margin-top: -39px;
        float: right;
        margin-bottom: 10px;
    }
    .questionnaire-box{
        padding: 28px;
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
    .questionnaire-info{
        font-size: 11px;
        color: #287D7C;
        line-height: 50px;
        font-weight: normal;
        display: block;
        text-align: center;
    }
    .question-type{
        color: #0066FF;
        font-weight: normal;
        font-size: 14px;
    }
</style>
