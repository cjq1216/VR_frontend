<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item to="/user/news-industry"><i class="el-icon-date"></i> <span class="bread">VR资讯</span></el-breadcrumb-item>
                <el-breadcrumb-item><span class="bread">产业</span></el-breadcrumb-item>
            </el-breadcrumb>
        </div>

        <div class="news-box">
            <h2 class="news-title">{{newsData.title}}</h2>
            <div class="borderline clearfix">
                <p class="artinfo" ><span class="author">来源：{{newsData.source}}</span> &nbsp;&nbsp;{{newsData.lastEditTime}}</p>
            </div>
            <div class="article">
                <p v-html="newsData.content">
                </p>
            </div>

             <div class="news-comment">
              <!--  <div class="comment">
                <el-button-group>
                <el-tooltip :content="upnumber" placement="top">
                  <el-button type="primary" icon="circle-check" @click="upNumber(newsData.id)">赞</el-button>
                  </el-tooltip>
                  <el-tooltip :content="downnumber" placement="top">
                  <el-button type="primary" @click="downNumber(newsData.id)">踩<i class="el-icon-circle-cross el-icon--right"></i></el-button>
                  </el-tooltip>
                </el-button-group>
                </div>
                <div>
                <div class="my-comment" >
                            <el-input type="textarea" :rows="3" placeholder="请输入您对该产品的评价" v-model.trim="textarea" resize=none style="width: 80%;">
                            </el-input>
                </div>
                <div class="comment">
                            <el-button type="primary" @click="refreshNewData()">发表评论</el-button>
                            </div>
                </div>-->
                    <div class="comment">
                        <div class="my-comment">
                            <el-input type="textarea" :rows="3" placeholder="请输入您对该产品的评价" v-model.trim="textarea" resize=none>
                                
                            </style>>
                            </el-input>
                        </div>
                        <div class="comment-btn">
                            <el-button-group style="margin-bottom:10px" >
                                <el-button icon="circle-check" size="small" @click="proLikeClick()" :disabled="news_thumbs.state==-1?true:false" :type="news_thumbs.state==1?successType:cancelType">
                                    赞{{news_thumbs.upvote}}</el-button>
                                <el-button size="small" @click="proUnlikeClick()" :disabled="news_thumbs.state==1?true:false" :type="news_thumbs.state==-1?successType:cancelType">
                                    踩{{news_thumbs.downvote}}<i class="el-icon-circle-cross el-icon--right"></i></el-button>
                            </el-button-group>
                            <el-button type="primary" @click="refreshNewData()">发表评论</el-button>
                        </div>
                    </div>

                </div>
                
                <div>
                <el-tabs v-model="activeName2" type="card" style="width:100%">
                <el-tab-pane label="最热评论" name="first">

                <div class="comment-content" v-for="(hot,index) in hot_data.hot_comments" v-show="display_hot">
                                <p class="user-name">{{hot.user.nickname}}</p>
                                <div class="comment-reply">
                                    <p class="user-comment">{{hot.message}}</p>
                                        <el-input style="{padding: 0 5px;}" v-show="hot_reply.replys[index].appear" type="textarea" placeholder="回复评论" v-model.trim="hot_reply.replys[index].textarea" resize=none>
                                        </el-input>
                                    <div class="reply-container" v-for="reply in hot.messageResponses">
                                        <div class="sub-reply-content">
                                            <p class="reply-name">{{reply.user.nickname}}：</p>
                                            <div class="reply-content">
                                                <p class="sub-reply-content">{{reply.messageResponse}}</p>
                                            </div>
                                        </div>
                                        <p class="reply-time">{{reply.releasetime}}</p>
                                    </div>
                                </div>
                                <div class="comment-right">
                                        <div class="comment-content-btn">
                                            <el-button type="primary" icon="circle-check" size="small" @click="commentLikeClick(hot)"
                                        :type="hot.state==1?successType:cancelType">赞{{hot.upvote}}</el-button>
                                            <div class="reply-btn">
                                                <el-button type="text" @click="replyHotClick(hot,index)">回复</el-button>
                                            </div>
                                        </div>
                                        <p class="comment-time">{{hot.releasetime}}</p>
                                </div>
                            </div>
                            </el-tab-pane>
                <el-tab-pane label="最新评论" name="second">
                    <div class="comment-content" v-for="(new_,index) in new_data.new_comments" v-show="display_new">
                                <p class="user-name">{{new_.user.nickname}}</p>
                                <div class="comment-reply">
                                    <p class="user-comment">{{new_.message}}</p>
                                        <el-input style="{padding: 0 5px;}" v-show="new_reply.replys[index].appear" type="textarea" placeholder="回复评论" v-model.trim="new_reply.replys[index].textarea" resize=none>
                                        </el-input>
                                    <div class="reply-container" v-for="reply in new_.messageResponses">
                                        <div class="sub-reply-content">
                                            <p class="reply-name">{{reply.user.nickname}}：</p>
                                            <div class="reply-content">
                                                <p class="sub-reply-content">{{reply.messageResponse}}</p>
                                            </div>
                                        </div>
                                        <p class="reply-time">{{reply.releasetime}}</p>
                                    </div>
                                </div>
                                <div class="comment-right">
                                        <div class="comment-content-btn">
                                            <el-button type="primary" icon="circle-check" size="small" @click="commentLikeClick(new_)"
                                        :type="new_.state==1?successType:cancelType">赞{{new_.upvote}}</el-button>
                                            <div class="reply-btn">
                                                <el-button type="text" @click="replyNewClick(new_,index)">回复</el-button>
                                            </div>
                                        </div>
                                        <p class="comment-time">{{new_.releasetime}}</p>
                                </div>
                            </div>
                </el-tab-pane>
              </el-tabs>
              </div>
            </div>
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
                successType:"success",
                cancelType:"primary",
                            news_id:null,
                // uid:'1',
                textarea:"",
                display_hot:false,
                display_new:false,
                newsData:{
                    id: 3,
                    tag: null,
                    title: "痛失手机江山的联想，想要在VR热潮中分一杯羹",
                    path: "D:\\VR新闻爬虫\\test2",
                    lastEditTime: "发布时间：2017-06-11 12:13:35",
                    source: "来源：POPPUR",
                    upvote: null,
                    downvote: null,
                    editor_id: null,
                    author: "作者：",
                    news_abstract: "在很多人眼里，联想手机似乎是一个比较“玄学”的存在。在PC领域拼搏多年，联想终于打进智能手机市场。只",
                    content: "在很多人眼里，联想手机似乎是一个比较“玄学”的存在。在PC领域拼搏多年，联想终于打进智能手机市场。只可惜初出茅庐的联想并没有什么经验，无奈在苹果和小米的拉锯战中成为了炮灰。虽然联想在2014年收购了手机界骨灰级元老摩托罗拉，但这条路始终还是没有想象中的那么好走。先是苹果傲视群雄，后来OPPO、vivo崛起，势单力薄的联想手机想要站稳脚跟还是有点难的。根据此前的消息，联想手机业务分支品牌ZUK已经暂停运营了。痛失手机江山之后，联想将落脚点放在了潜力巨大的虚拟现实领域。去年10月，联想与微软进行了合作，未来两者将会在混合现实领域(包括增强现实和虚拟现实)共同谋求发展，开辟新天地。随后联想也很快推出了一款主打入门的混合现实头显，斩获了不少用户的好评。而在最近的CES亚洲展上，联想还推出了首款背包VR PC——Avalon  VR。这款PC外观设计精致，虽然采用了金属机身的设计，但整机的重量也仅有2.5kg(包括电池)。配置方面，Avalon  VR最高支持英特尔酷睿i7处理器，32GB DDR4内存以及512GB PCI-E固态硬盘，同时内置AMD Radeon  RX480/580独立显卡，足以轻松应对市面上所有VR游戏。此外，该电脑还采用了双电池设计，并且支持热插拔，官方称其普通模式下的续航时间可达10小时，即使是在高性能下进行VR体验也能坚持1.5小时。而为了帮助主机散热，该机还配备了水冷散热系统，出风口也设置在了背对背部的位置，避免热风吹向人体。根据联想的介绍，之后张艺谋VR体验店会全线采用Avalon  VR背包电脑，可见联想在VR界的地位还是值得被肯定的。而随着与微软的合作更加深入之后，未来联想还会在增强现实乃至混合现实界混出一片天地。不知道未来的PC市场究竟会变成怎样呢?真是越来越期待了呀。Via：POPPUR",
                    pic_location: "http://www.vrrb.cn/d/file/kuaixun/2017-06-11/74ba6748a8a88ed7b083e961a7ff9da4.jpg",
                    is_push: null
                },
                news_thumbs:{
                    upvote:50,
                    downvote:10,
                    state:0
                },
                hot_data:{
                    hot_comments:[{
                        id:1,
                        user:{
                            id:1,
                            nickname:'lalala'
                        },
                        message:"1",
                        upvote:2,
                        belong:"8",
                        releasetime:'2017-8-17',
                        messageResponses:[{
                            user:{
                                id:10,
                                nickname:'heiheihei',
                            },
                            messageResponse:"555",
                            releasetime:'2017-8-17'
                        }],
                        state:0,
                    }],
                },
                new_data:{
                    new_comments:[{
                        id:2,
                        user:{
                            id:2,
                            nickname:null
                        },
                        message:"2",
                        upvote:3,
                        belong:"4",
                        releasetime:null,
                        messageResponses:[{
                            user:{
                                id:11,
                                nickname:null,
                            },
                            messageResponse:"5555",
                            releasetime:null
                        }],
                        state:0,
                    }],
                },
                hot_reply:{
                    replys:[{
                        appear:0,
                        textarea:""
                    }]
                },
                new_reply:{
                    replys:[{
                        appear:0,
                        textarea:""
                    }]
                }
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
            postProLike(val){
                var self=this;
                self.$axios({
                    url:'/Thumbs/upvote/'+localStorage.getItem('ms_userid'),
                    method:'post',
                    baseURL: self.hostURL,
                    data:{
                        name:"industry"+self.news_id,
                        value:val
                    }
                }).catch((error)=>{
                    console.log(error);
                });
            },
            proLikeClick(){
                var self=this;
                self.news_thumbs.state=(self.news_thumbs.state==1)?0:1;
                var value=(self.news_thumbs.state==1)?1:-1
                self.news_thumbs.upvote+=value;
                console.log(self.news_thumbs.state+" "+self.news_thumbs.upvote);
                self.postProLike(value);
            },
            postProUnlike(val){
                var self=this;
                self.$axios({
                    url:'/Thumbs/downvote/'+localStorage.getItem('ms_userid'),
                    method:'post',
                    baseURL: self.hostURL,
                    data:{
                        name:"industry"+self.news_id,
                        value:val
                    }
                }).catch((error)=>{
                    console.log(error);
                });
            },
            proUnlikeClick(){
                var self=this;
                self.news_thumbs.state=self.news_thumbs.state==-1?0:-1;
                var value=self.news_thumbs.state==-1?1:-1;
                self.news_thumbs.downvote+=value;
                //.log(self.news_thumbs.state+" "+self.news_thumbs.downvote);
                self.postProUnlike(value);
            },
            //留言
            postTextArea(){
                var self=this;
                self.$axios({
                    url:'/MessageLeaving/'+localStorage.getItem('ms_userid'), //{uid}
                    method:'post',
                    baseURL: self.hostURL,
                    data:{
                        content:self.textarea, //时间在后台取得
                        belong:"industry"+self.newsData.id
                    }
                }).catch((error)=>{
                    console.log(error);
                });
            },
            refreshNewData(){
                var self=this;
                var text=self.textarea;
                //console.log(text);
                if(text!=""){
                    self.$message('评论成功！');
                    self.postTextArea();
                    self.new_data.new_comments.reverse();
                    self.new_data.new_comments.push({
                        id:"",
                        user:{
                            id:"",
                            nickname:localStorage.getItem("ms_username")
                        },
                        message:text,
                        upvote:0,
                        belong:"0",
                        releasetime:"刚刚",
                        messageResponses:[],
                        state:0,
                    });
                    self.new_data.new_comments.reverse();
                    self.hot_data.hot_comments.push({
                        id:"",
                        user:{
                            id:"",
                            nickname:localStorage.getItem("ms_username")
                        },
                        message:text,
                        upvote:0,
                        belong:"0",
                        releasetime:"刚刚",
                        messageResponses:[],
                        state:0,
                    });
                    // console.log("self id:"+self.hot_data.hot_comments[0].id);
                    var len=self.new_data.new_comments.length;
                    self.new_reply.replys=[];
                    for(var i=0;i<len;i++)
                        self.new_reply.replys.push({
                            appear:0,
                            textarea:""
                        });
                    len=self.hot_data.hot_comments.length;
                    self.hot_reply.replys=[];
                    for(var i=0;i<len;i++)
                        self.hot_reply.replys.push({
                            appear:0,
                            textarea:""
                        });
                    //.log("textarea="+self.textarea);
                    self.textarea="";
                    //console.log("textarea="+self.textarea);
                }
            },
            //回复-热评
            replyHotClick(item,index){
                var self=this;
                if(!self.hot_reply.replys[index].appear){
                    self.hot_reply.replys[index].appear=1;
                }else{
                    if(self.hot_reply.replys[index].textarea=='') self.hot_reply.replys[index].appear=0;
                    else{
                        //发送数据给后台
                        self.$axios({
                            url:'/MessageResponse/'+localStorage.getItem('ms_userid'), //{uid}
                            method:'post',
                            baseURL: self.hostURL,
                            data:{
                                content:self.hot_reply.replys[index].textarea,
                                leavingid:item.id
                            }
                        }).catch((error)=>{
                            console.log(error);
                        });
                        self.$message('回复成功！');
                        self.hot_data.hot_comments[index].messageResponses.push({
                            user:{
                                id:"",
                                nickname:localStorage.getItem('ms_username'),
                            },
                            messageResponse:self.hot_reply.replys[index].textarea,
                            releasetime:"刚刚"
                        });
                        self.hot_reply.replys[index].textarea='';
                        self.hot_reply.replys[index].appear=0;
                    }
                }
            },
            //回复-最新评价
            replyNewClick(item,index){
                var self=this;
                if(!self.new_reply.replys[index].appear){
                    self.new_reply.replys[index].appear=1;
                }else{
                    if(self.new_reply.replys[index].textarea=='') self.new_reply.replys[index].appear=0;
                    else{
                        //发送数据给后台
                        self.$axios({
                            url:'/MessageResponse/'+localStorage.getItem('ms_userid'), //{uid}
                            method:'post',
                            baseURL: self.hostURL,
                            data:{
                                content:self.new_reply.replys[index].textarea,
                                leavingid:item.id
                            }
                        }).catch((error)=>{
                            console.log(error);
                        });
                        self.$message('回复成功！');
                        self.new_data.new_comments[index].messageResponses.push({
                            user:{
                                id:"",
                                nickname:localStorage.getItem('ms_username'),
                            },
                            messageResponse:self.new_reply.replys[index].textarea,
                            releasetime:"刚刚"
                        });
                        self.new_reply.replys[index].textarea='';
                        self.new_reply.replys[index].appear=0;
                    }
                }
            },
            /*获得评论*/
            //1.获得热评
            getHotComments(id){
                var self=this;
                self.hot_data={
                    hot_comments:[]
                };
                self.hot_reply={
                    replys:[
                        {
                        appear:0,
                        textarea:""
                    }]
                };
                self.$axios({
                    url:'/MessageLeaving/show/'+localStorage.getItem('ms_userid')+'?belong='+"industry"+id+'&condition=hot',
                    method:'get',
                    baseURL: self.hostURL,
                    // data:{
                        // condition:"hot",
                        // belong:localStorage.getItem("salesModel")
                    // }
                }).then((response)=>{
                    //.log("resp id:"+response.data[0].id);
                    //console.log("self id:"+self.hot_data.hot_comments[0].id);
                    self.display_hot=true;
                    self.hot_data.hot_comments= response.data;
                    self.hot_data.hot_comments.reverse();
                    //console.log("self id:"+self.hot_data.hot_comments[0].id);
                    var len=self.hot_data.hot_comments.length;
                    self.hot_reply.replys=[];
                    for(var i=0;i<len;i++)
                        self.hot_reply.replys.push({
                            appear:0,
                            textarea:""
                        });
                }).catch((error)=>{
                    console.log(error);
                });
            },
            getNewsData(id){
            var self = this;
            self.newsData = {};
            self.$axios({
                url:'/news/'+id,
                method:'get',
                baseURL: self.hostURL
            }).then((response)=>{
                self.newsData = response.data;
                self.news_thumbs.upvote = response.data.upvote;
                self.news_thumbs.downvote = response.data.downvote;
            }).catch((error)=>{
                console.log(error);
            });
        },
        //2.获得最新评论
            getNewComments(id){
                var self=this;
                self.new_data={
                    new_comments:[]
                };
                self.new_reply={
                    replys:[
                        {
                        appear:0,
                        textarea:""
                    }]
                };
                self.$axios({
                    url:'/MessageLeaving/show/'+localStorage.getItem('ms_userid')+'?belong='+"industry"+id+'&condition=time',
                    method:'get',
                    baseURL: self.hostURL,
                    // data:{
                        // condition:"time",
                        // belong:localStorage.getItem("salesModel")
                    // }
                }).then((response)=>{
                    //.log("resp id:"+response.data[0].id);
                    //console.log("self id:"+self.new_data.new_comments[0].id);
                    self.display_new=true;
                    self.new_data.new_comments= response.data;
                    self.new_data.new_comments.reverse();
                    //console.log("self id:"+self.new_data.new_comments[0].id);
                    var len=self.new_data.new_comments.length;
                    self.new_reply.replys=[];
                    for(var i=0;i<len;i++)
                        self.new_reply.replys.push({
                            appear:0,
                            textarea:""
                        });
                }).catch((error)=>{
                    console.log(error);
                });
            },
            getProThumbs(){
                var self=this;
                self.news_thumbs={};
                //console.log("proname: "+localStorage.getItem("proName"));
                self.$axios({
                    url:'/Thumbs/'+localStorage.getItem('ms_userid'),
                    method:'post',
                    baseURL: self.hostURL,
                    data:{
                        belong:"industry"+self.news_id
                    }
                }).then((response)=>{
                    // self.pro_thumbs={};
                    ///console.log("response.data:upvote "+response.data.upvote);
                    //console.log("self data:upvote "+self.pro_thumbs.upvote);
                    self.news_thumbs= response.data;
                    //console.log("self data:upvote "+self.pro_thumbs.upvote);
                }).catch((error)=>{
                    console.log(error);
                });
            },
        //评论点赞（用于后台存储被点赞的评论）
            postCommLike(item,val){
                var self=this;
                self.$axios({
                    url:'/MessageLeaving/upvote/'+localStorage.getItem('ms_userid'),
                    method:'post',
                    baseURL: self.hostURL,
                    data:{
                        id:item.id,
                        belong:"industry"+self.news_id,
                        value:val,
                    }
                }).catch((error)=>{
                    console.log(error);
                });
            },
            commentLikeClick(item){
                var self=this;
                item.state=(item.state==1)?0:1;
                var value=(item.state==1)?1:-1;
                item.upvote+=value;
                self.postCommLike(item,value);
            },
            upNumber(id) {
                var self = this;
                self.$axios({
                    url:'/news/upvote/' + id,
                    method:'get',
                    baseURL: self.hostURL
            }).then((response)=>{
                self.upnumber = response.data;
            }).catch((error)=>{
                console.log(error);
            });
        },
            downNumber(id) {
                var self = this;
                self.$axios({
                    url:'/news/downvote/' + id,
                    method:'get',
                    baseURL: self.hostURL
            }).then((response)=>{
                self.downnumber = response.data;
            }).catch((error)=>{
                console.log(error);
            });
            }
        },
        
        mounted(){
            var self= this;
            var user_name=localStorage.getItem("ms_username");
            if(user_name==""){
                this.$router.replace('/login');
            }
            //console.log("2222");
            var arr = location.href.split('?');
                            self.news_id = arr[1];
            //console.log(self.news_id);
            self.getProThumbs();
            self.getNewsData(self.news_id);
            self.getHotComments(self.news_id);
            self.getNewComments(self.news_id);
        },
    }
</script>

<style scoped>
.crumbs{
    text-decoration: none;
}
.bread{
    font-size: 16px;
}
.form-box{
    width:300px;
    margin-top:50px;
    margin-left:0px;
    box-shadow:0 0 8px 0
        rgba(232,237,250,.9),0 2px 4px 0
        rgba(232,237,250,.9);
    padding:50px 50px 50px 10px;  
}
.submit-btn{
    width:220px;
    margin-left:80px;
}
.submit-btn button{
    width:100%; 
}
.news-box{
    padding: 28px;
    width: 800px;
    border: 1px solid #E1E1E1;
    box-shadow: 0 0 2px rgba(0,0,0,.1);
}
.news-title{
        font-size: 22px;
    color: #287D7C;
    line-height: 50px;
    font-weight: normal;
        display: block;
        text-align: center;
}
.borderline{
    border-bottom: 1px dotted #BFBFBF;
}
.comment-content{
    display: flex;
    width: 100%;
    padding: 15px 5px 5px 5px;
    border-bottom: 1px solid #DDDDDD;
}
.comment-reply{
    /*flex:1;*/
    width: 70%;
}
.user-comment{
    font-size: 17px;
    text-align:left;
    /*font-weight: lighter; */
    width: 100%;
    padding-bottom: 15px;
}
.reply-container{
    /*background:#DDDDDD;*/
    width:80%;
    margin: 10px 0 0 50px;
    /*padding: 10px 5px 15px 20px;*/
    border-bottom: 1px solid #DDDDDD;
}
.sub-reply-content{
    width: 100%;
    display: flex;
}
.reply-name{
    color:#20A0FF;
    font-size: 13px;
    text-align: right;
    flex: 1;
}
.reply-content{
    float: right;
    width: 80%;
    /*margin-left: -20%;*/
    /*overflow: hidden;*/
}
.sub-reply-content{
    font-size: 13px;
    text-align: left;
    width: 100%;
}
.reply-time{
    color:#666666;
    width: 100%;
    text-align: right;
    font-size:10px;
    padding: 5px 0;
}
.comment-time{
    /*position: relative;*/
    color:#666666;
    font-size:10px;   
    width: 100%;
    text-align: left;
    margin-top: 10px;
    /*bottom: 0;*/
}
.comment-right{
    /*text-align: left;*/
    /*width: 15%;*/
    flex: 1;
    padding: 10px;
}
.user-name{
    font-size: 15px;
    color: #666666;
    width: 15%;
    text-align: center;
}
.artinfo{
    float: left;
    padding-bottom: 5px;
    color: #999;
        margin: 0;
    padding: 0;
    display: block;
    text-align: center;
    margin-top: 15px;
    width: 800px;
}
.author{
    color: #666;
}
.article p{
    padding: 30px 0 50px;
    font-size: 18px;
    color: #333;
    line-height: 200%;
    text-indent:2em;
}
.article {
    display: inline-block;
    line-height: 26px;
    padding-bottom: 25px;
}
.news-img{
    text-align: center;
}
.news-comment{
    margin-top: 20px;
    text-align: center;
    width: 100%;
}
.comment-btn{
    margin-left: 10px;
}
.item {
  margin-top: 10px;
  margin-right: 40px;
}
.comment{
    margin: 20px;
    width:100%;
}
.my-comment{
    float: left;
    width: 80%;
}
</style>