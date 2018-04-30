<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item to="/user/pedia"><i class="el-icon-date"></i>VR百科 </el-breadcrumb-item>
                <el-breadcrumb-item>目录浏览</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="about-w3-agile" id="about">
            <!--<div class="container">-->
                <el-row >
                    <el-col :span="6">
                    <div id="navigation">
                        <div class="collection">
                            <ul>
                            <a href="#/user/pedia2" class="collection-item"
                            v-for="onetype in typeList"
                            @click="selected(onetype)"
                            :class="{active: type == onetype}"><li>{{onetype}}</li></a>
                            </ul>
                        </div>
                    </div>
                    </el-col>
                    <el-col :span="18">
                    <div id="show-items">
                        <div v-for="item in items" class="sing-item" @click="encyclopediaClick(item)">
                            <a href="javascript:void(0);" class="link-tit" title=""></a>
                            <h2>{{item.keyword}}</h2>
                            <article>{{item.prop_keyword}}</article>
                        </div>
                    </div>
                    </el-col>
                </el-row>
          <!--</div>-->
    </div>


         <div class="pagination">
            <el-pagination
                @current-change="handleCurrentChange"
                :current-page.sync="pageNum"
                layout="prev, next">
            </el-pagination>
        </div>

    </div>
</template>


<script>

    export default {
        data: function(){
            return {
                pageNum:1,
                pageTotal:20,
                pageSize:1,
                hostURL:'/VR',
                
                items:[
                  {
                        id:1,
                        keyword:'fuuuuuuu',
                        type:"",
                        changenote:'11111111111111111111111111111111111111',
                        prop_keyword:'2222222222222222',
                    },
                    {
                        id:2,
                        keyword:'ffffffff',
                        type:"",
                        changenote:'1111111111111111111111111111111111',
                        prop_keyword:'222222222222',
                    }
                ],
                type:'VR简介',
                typeList: ['VR简介', 'VR应用', 'VR历史']
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
            getTypes(){
              var self=this;
              self.typeList=[];
              self.$axios({
                    url:'/encyclopediaDir',
                    method:'post',
                    baseURL:self.hostURL,
                    data:"notype"
                }).then((response)=>{
                    self.typeList = response.data;
                }).catch((error)=>{
                    self.$message({
                        type:'info',
                        message:'connect fail'
                    });
                });

            },
            search(onetype,pageNum){
              var self=this;
              //self.items=[];
              var dirtype={
                type:"",
                pageNum:1
              };
              dirtype.type=onetype;
              dirtype.pageNum=pageNum;
              self.$axios({
                    url:'/encyclopediaTypeDir',
                    method:'post',
                    baseURL:self.hostURL,
                    data:dirtype
                }).then((response)=>{
                    console.log("len="+response.data.length);
                    if(response.data.length==0){
                        if(self.pageNum!=1){
                            self.pageNum=self.pageNum-1;
                            self.$message({
                                type:'info',
                                message:'已经是最后一页了！'
                            });
                        }
                    }else{
                        self.items=[];
                        self.items = response.data;
                    }
                }).catch((error)=>{
                    console.log(error);
                    self.$message({
                        type:'info',
                        message:'connect fail'
                    });
                });

            },
            selected(onetype){
              var self=this;
              self.type=onetype;
              self.search(onetype,1);
            },
            handleCurrentChange(){
                var self = this;
                console.log("current-Page:");
                console.log(self.pageNum);
                var type=self.type;
                var pageNum=self.pageNum;
                self.search(type,pageNum);
            },
            encyclopediaClick(item){
                var self=this;
                console.log(item.id);
                self.$router.push('/user/encyclopedia-prop?'+item.id);
            }
        },
        

        mounted(){
            var self = this;
            var user_name=localStorage.getItem("ms_username");
            if(user_name==""){
                this.$router.replace('/login');
            }
            //$(function(){ $(window).scrollTop(0)});
            //self.getTypes();
            self.selected("VR简介");
        }
    }
</script>

<style>

</style>
