<template>
    <div  class="bji-content">
        <!--情报板-->
        <el-row>
            <el-col :span="23">
               <div class="grid-content bg-purple-dark" style="height:180px;">
                    <p class="bji-top" :style="{color:color,width:width,height:height,fontSize:fontSize,fontFamily:fontFamily}">{{textarea}}</p>
                    <ul class="bji-top-i">
                      <li> <i class="el-icon-edit-outline" style="color:#09f;"></i>新建信息</li>
                      <li><i class="el-icon-tickets" style="color:#09f;"></i>保存信息</li>
                    </ul>
               </div>
            </el-col>
        </el-row >

       <div>
         <el-row>
           <el-col :span="6">
               <!--左边-->
              <div class="grid-fsbottom">
                  <div class="grid-top">
                      <span>信息类型选择</span>
                  </div>
                  <el-form size="mini">
                  <el-form-item label="信息类型:" class="item item-fsleft">
                      <el-select v-model="value" placeholder="-请选择-" @change="currentSel" style="width:170px;">
                          <el-option
                              v-for="item in options"
                              :key="item.value"
                              :label="item.content"
                              :value="item.typeId">
                          </el-option>
                      </el-select>
                  </el-form-item>
                  </el-form>
                  <div class="grid-xh"> 
                      <ul class="grid-uli" v-if="disli.length!==0">
                            <li style=" list-style-type:none; border-bottom:1px solid #BFBDBD;padding-left:10px;color:#4D4E52;cursor:pointer" v-for="itli in disli" :key="itli.infoId">{{itli.display}}</li>
                      </ul>
                  </div>
              </div>
           </el-col>
           <el-col :span="17">
               <!--右边-->
              <div class="bjy">
                <!--图片编辑区-->
                  <el-row style="margin-left:0px;">
                    <el-col :span="24">
                      <div class="grid-content bg-purple">
                          <div class="bj-top"><span>图片编辑区</span></div>

                          <div>
                            <div class="bj-img-top" >图片:</div>
                            <div class="bj-img" >

                            </div>
                            <span style="color:#ff0000;margin-left:10px;font-size:12px;">(注:双击图片取消选择)</span>
                          </div>
                          <div class="bj-img-right">
                            <el-checkbox v-model="checked" @change="showimg">消息是否添加图片</el-checkbox>
                            <div class="bj-img-rbottom" >
                              <span v-show="isShow"  v-for="(gimg,index) in 63" :key="index" style="margin-left:10px;margin-right:5px;display:inline-block;">
                                <img  :src="require(`@/assets/img/qbbimg/0${gimg<10?'0'+gimg:gimg}.BMP`)" @click="handelimg(gimg)">
                                <span style="display:block;margin-left:6px;margin-bottom:15px;">{{(gimg &lt; 10) ?( '0'+gimg):gimg}}</span>
                              </span>
                        
                            </div>
                          </div>
                      </div></el-col>
                  </el-row>

                  <!--信息编辑区-->
                  <el-row style="margin-left:0px;">
                        <el-col :span="24">
                         <div class="grid-content bg-purple">
                          <div class="bj-top"><span>信息编辑区</span></div>
                              <el-form :inline="true" size="mini">
                                <el-form-item label="字体类型:" class="bj">
                                      <el-select v-model="ziti.lx" placeholder="黑体" class="select" @change="zitilx">
                                          <el-option label="黑体" value="黑体"></el-option>
                                          <el-option label="宋体" value="宋体"></el-option>
                                          <el-option label="楷体" value="楷体"></el-option>
                                      </el-select>
                                  </el-form-item>
                                  <el-form-item label="字体大小:" class="bj">
                                      <el-select v-model="ziti.size" placeholder="16px" class="select" @change="zitisize">
                                          <el-option label="16" value="16px"></el-option>
                                          <el-option label="24" value="24px"></el-option>
                                          <el-option label="32" value="32px"></el-option>
                                          <el-option label="48" value="48px"></el-option>
                                      </el-select>
                                  </el-form-item>
                                  <el-form-item label="字体颜色:" class="bj">
                                      <el-select v-model="ziti.cl" placeholder="红色" class="select" @change="ziticl">
                                          <el-option label="红色" value="#FF0000"></el-option>
                                          <el-option label="黄色" value="#EAF509"></el-option>
                                          <el-option label="绿色" value="#20F509"></el-option>
                                      </el-select>
                                  </el-form-item>
                              </el-form>

                              <div class="zifu">字符区域编辑</div>
                              <el-input
                                  type="textarea"
                                  :rows="4"
                                  placeholder="请输入内容"
                                  v-model="textarea" class="text">
                              </el-input>

                              <div style="float:right;marginRight:30px">
                                <span>情报板规格:</span>
                                
                              </div>
                          </div></el-col>
                  </el-row>
              </div>
           </el-col>
         </el-row>
          
          
          
       </div>

      
    </div>
</template>

<script>
import axios from 'axios'
    export default {
        data(){
          return{
            
             standard: "32*32",
             textarea:'',
             width:'80px',
             height:'80px',
             color:'#FF0000',
             fontSize:'16px',
             fontFamily:'黑体',
             options:[],
             disli:[],
             value:'',
             checked: false,//复选框是否勾选
             isShow:false,//图片是否显示
             ziti:{
               lx:'',
               size:'',
               cl:''
             },

          }
        },
        methods:{
          //进入页面     
          handlebj(){
            axios.get('http://127.0.0.1:8080/static/mock/data.json').then(res=>{
                    console.log('leixin',res.data.leixinxianshi)
                    this.options=res.data.leixinxianshi;
                    //判断设备型号
                    console.log(this.options[0].standard)
                    this.standard=this.options[0].standard
                      console.log('1',this.standard)
                      switch(this.standard){
                        case "48*48":
                          this.width='96px',this.height='96px';
                          break;
                        case "320*32":
                          this.width='640px',this.height='64px';
                          break;
                        case "96*48":
                          this.width='192px',this.height='98px';
                          break;
                        case "192*112":
                          this.width='192px',this.height='112px';
                          break;
                        case "576*48":
                          this.width='576px',this.height='48px';
                          break;
                      }
                       
            })
          },
          currentSel(value){
                console.log('zziti',value)
                axios.get('http://127.0.0.1:8080/static/mock/data.json').then(res=>{
                    console.log("disli",res.data.xinxileixin)
                    this.disli=res.data.xinxileixin
                })
            }, 
            //复选框选中图片显示
            showimg(checked){
               if(checked){
                 this.isShow=true;
               }else{
                 this.isShow=false;
               }
            },
            //点击图片
            handelimg(gimg){
                console.log('gimg',gimg)
            },
            //字体选择
            zitilx(value){
               this.fontFamily=value;
            }, 
            ziticl(value){
               this.color=value;
            },
            zitisize(value){
              // this.fontSize=value
               switch(this.standard){
                 case "192*112":
                    this.fontSize=value.replace(/px/g,'')*2+'px';
                    break;
                 case "576*48":
                    this.fontSize=value.replace(/px/g,'')*2+'px';
                    break;
               }
            }
        },
        mounted(){
          this.handlebj();
        },
    }
</script>

<style scoped>
.grid-content {
    border-radius: 4px;
    border:1px solid #BFBDBD;   
  }
 .bji-content{
     height: 700px;
     margin-left:10px;
     margin-right: 10px;
 }
 
 .bji-top{
   background:#000;
   width:100px;
   height:100px;
   color:#fff;
   line-height: 100px;
   text-align: center;
   position: relative;
   left:200px;
   top:30px;  
 }
 .bji-top-i{
   float: right;
   margin-top:-86px;
   margin-right: 5px;

 }
 .bji-top-i li{
   float: left;
   cursor: pointer;
   margin-right: 10px;
 }
 .bji-top-i li:hover{
   color: #09f
 }
 .item{
   margin-left:10px;
   margin-top:10px;
 }
 .grid-top{
      line-height: 40px;
      padding-left: 10px;
      height: 40px;
      border-bottom: 1px solid #828181;
  }
  .grid-fsbottom{
     border: 1px solid #000;
     border: 1px solid #BFBDBD;
     margin-right: 10px;
     border-radius: 4px;
     width:270px;
     height:490px;
 }
.bjy{
  /* border-radius: 4px; */
  height: 490px;
}
.bj-top{
  height: 30px;
  line-height: 30px;
  padding-left:10px;
  border-bottom:1px solid #BFBDBD;
}
.bj-img-top{
  margin-left:10px;
  padding-top:20px;
}
.bj-img{
  width:80px; 
  height:80px;
  border:1px solid #FF0000;
  margin-top:20px;
  margin-left:10px;
}
.bj-img-right{
  float:right;
  width:80%;
  margin-top:-140px;
  }
  .bj-img-rbottom{
    width:100%;
    height:120px;
    border:1px solid #BFBDBD;
    margin-top:10px;
    overflow-x:auto;
    }
    .bj{
      margin-left:10px;
      margin-top:10px;
    }
    .zifu{
      margin-left: 10px;
      font-size: 12px;
    }
    .grid-xh{
     /* border: 1px solid #BFBDBD; */
     border-radius: 4px;
    }
    .grid-uli{
      line-height: 40px;
      height: 40px;
      border-top: 1px solid #BFBDBD;
    }
 .el-row {
    margin-bottom: 10px;
    margin-left:30px;
  }
  .select{
    width:170px;
  }
  .text{
    margin-left:10px;
    width: 70%;
    margin-top:20px;
  }
  .el-col {
    border-radius: 4px;
    /* border: 1px solid #BFBDBD; */
  }
  .bg-purple-dark {
    background: #fff;
  }
  .bjy .bg-purple {
    background: #fff;
    padding-bottom: 35px
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
</style>

<style>
.bj .el-form-item__label {
    color:#000;
    font-size: 12px;
}
.el-form--inline .el-form-item{
  margin-right: 0px;
}
</style>
