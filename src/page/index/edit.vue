
<template>
    <div class="root_box">
        <h5 class="big_title">频道</h5>
        <div class="wrap_bg">
            <div class="edit_item" v-for="(list,ind) in editList" :key="ind">
                <h6>
                    <span>{{list.SortName}}</span>
                    <span class="btu_edit" v-if='list.SortId=="1" && allStatus==0' @click="setEdit">编辑</span>
                    <span class="btu_edit" v-if='list.SortId=="1" && allStatus==1' @click="setFinish">完成</span>
                </h6>
                <div class="edits">
                    <ul>
                        <li v-for="(item,index) in list.SortList" :key="index">
                            <!-- 我的应用加拖曳事件 -->
                            <div v-if='list.SortId=="1"' class="e_items"   @dragover="preventOver($event)" @drop.prevent="drop($event)" id="dropBox">
                                <div class="e_bg" @dragstart="drag($event)" v-bind:data-id="index" draggable="true" @click="minusOrPlus(list,ind,item,index)" id="dragBox">
                                    <div class="e_status">
                                        <span class="e_minus" v-if='item.EditStatus =="minus" && allStatus==1'>-</span>
                                        <span class="e_plus" v-if='item.EditStatus =="plus" && allStatus==1'>+</span>
                                    </div>
                                    <span class="e_icon" v-bind:style="{background: item.background}"></span>
                                    <p>{{item.text}}</p>
                                </div>
                            </div>

                            <div v-else class="e_items">
                                <div class="e_bg" @click="minusOrPlus(list,ind,item,index)">
                                    <div class="e_status">
                                        <span class="e_minus" v-if='item.EditStatus =="minus" && allStatus==1'>-</span>
                                        <span class="e_plus" v-if='item.EditStatus =="plus" && allStatus==1'>+</span>
                                    </div>
                                    <span class="e_icon" v-bind:style="{background: item.background}"></span>
                                    <p>{{item.text}}</p>
                                </div>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
        <!-- 弹框提示 -->
        <!-- 传递对象或者数组 -->
        <alert-tip :dialog="dialog" v-show="isDialog" @dialogConfirm="dialogCancel" @close='dialogCancel'></alert-tip>
    </div>
</template>
<script lang="ts">
    import Vue from 'vue'
    import Component from 'vue-class-component'
    import axios from 'axios'
    import {mapMutations} from 'vuex'
    import alertTip from '@/components/dialog/alertTip.vue'

    export default class EditComponents extends Vue {
        /**
         * @name ts在vue框架中语法注意
         * 1. 属性声明要添加类型，属性后不能有标点符号
         * 2.
         *
         *
         *
         *
         *
         * */
        name: string =  'edit'
        data () {
            return {
                editList :[
                    {
                        "SortName":"我的应用",//必须有
                        "SortId":"1", //必须有
                        "SortList":[
                            {
                                "text": "火车票", //必须有
                                "background":"#e9b4c5", //暂时标记页面颜色
                                "SubSortId":"2",//必须有
                            },
                            {
                                "text": "机票",
                                "background":"#b4e2e9",
                                "SubSortId":"2"
                            },
                            {
                                "text": "日本",
                                "background":"#badd9b",
                                "SubSortId":"2"
                            },
                            {
                                "text": "德国",
                                "background":"#9bddd6",
                                "SubSortId":"2"
                            },
                             {
                                "text": "健身",
                                "background":"#e9b4c5",
                                "SubSortId":"3"
                            },
                            {
                                "text": "在线教育",
                                "background":"#b4e2e9",
                                "SubSortId":"3"
                            },
                            {
                                "text": "拍照",
                                "background":"#badd9b",
                                "SubSortId":"3"
                            }
                        ]
                    },
                    {
                        "SortName":"旅游",
                        "SortId":"2",
                        "SortList":[
                            {
                                "text": "火车票",
                                "background":"#e9b4c5",
                                "SubSortId":"2"
                            },
                            {
                                "text": "机票",
                                "background":"#b4e2e9",
                                "SubSortId":"2"
                            },
                            {
                                "text": "日本",
                                "background":"#badd9b",
                                "SubSortId":"2"
                            },
                            {
                                "text": "德国",
                                "background":"#9bddd6",
                                "SubSortId":"2"
                            },

                            {
                                "text": "境内意外险",
                                "background":"#e9b4c5",
                                "SubSortId":"2"
                            },
                            {
                                "text": "境外意外险",
                                "background":"#b4e2e9",
                                "SubSortId":"2"
                            },
                            {
                                "text": "全年意外险",
                                "background":"#badd9b",
                                "SubSortId":"2"
                            },
                            {
                                "text": "高铁贵宾厅",
                                "background":"#9bddd6",
                                "SubSortId":"2"
                            }

                        ]
                    },
                    {
                        "SortName":"生活",
                        "SortId":"3",
                        "SortList":[
                            {
                                "text": "健身",
                                "background":"#e9b4c5",
                                "SubSortId":"3"
                            },
                            {
                                "text": "在线教育",
                                "background":"#b4e2e9",
                                "SubSortId":"3"
                            },
                            {
                                "text": "拍照",
                                "background":"#badd9b",
                                "SubSortId":"3"
                            }
                        ]
                    },
                    {
                        "SortName":"医疗",
                        "SortId":"4",
                        "SortList":[
                            {
                                "text": "问诊",
                                "background":"#e9b4c5",
                                "SubSortId":"4"
                            },
                            {
                                "text": "挂号",
                                "background":"#b4e2e9",
                                "SubSortId":"4"
                            },
                            {
                                "text": "电话医生",
                                "background":"#badd9b",
                                "SubSortId":"4"
                            }
                        ]
                    }
                ],
                allStatus: 0, //编辑0 ，完成1

                // 当前移动的信息
                curStartID : -1,

                //测试弹框提示
                isDialog: false,
                dialog:{     //传递参数
                    hasCancel: false, //是否有取消，默认没有
                    tips:"", //提示文案
                },

            }
        }

        created(){
        }
        mounted(){
        }
        methods:{
            //Mutation 必须是同步函数
            ...mapMutations([
                'USER_EDITLIST'
            ])

            //点击编辑设置状态
            setStatus(){
                this.editList.forEach((list,i) => {
                    list.SortList.forEach((item:object,index:number) => {
                        if(list.SortId=="1"){//我的应用
                            if(list.EditStatus == "undefined"){
                                Vue.$set(item,"EditStatus","minus");
                            }else{
                                item.EditStatus = "minus";
                            }
                        }else{
                            if(list.EditStatus == "undefined"){
                                Vue.$set(item,"EditStatus","plus");
                            }else{
                                item.EditStatus = "plus";
                            }
                        }
                    })
                });
            },
            //编辑
            setEdit(){
                this.allStatus = 1;
                this.setStatus();
            },
            //完成 或 async setFinish
            setFinish(){
                this.allStatus = 0;
                let status = {
                    userEditList: this.editList[0].SortList || []
                }
                //2018-02-28测试state管理
                this.USER_EDITLIST(status);
                //脚本路由跳转方式
                this.$router.go(-1); // this.$router.push({path:'/index'})
            },
            //加减应用
            minusOrPlus(list:object, ind:number,item:object,index:number){
                //点击从我的应用减去
                if(this.allStatus==1){
                    let curElem = this.editList[ind];

                    if(list.SortId == "1"){ //已在“我的应用”中
                        curElem.SortList.splice(index,1);
                        item.EditStatus = "minus";
                        this.editList.forEach( el => {
                            if(el.SortId == "1") return false;
                            for(let kk = 0; kk < el.SortList.length; kk++){
                                let element = el.SortList[kk];
                                if(element.text == item.text){
                                    element.EditStatus = "plus";
                                    break;
                                }
                            }
                        })
                    }else{
                        //加状态
                        if(item.EditStatus == "plus"){
                            if(this.editList[0].SortList.length > 6){
                                //测试单一弹框按钮
                                this.isDialog = true;
                                this.dialog.tips = "添加到首页的应用不能超过7个";
                                return false;
                            }else{
                                item.EditStatus = "minus";
                                this.editList[0].SortList.push(item);
                            }
                        }else{
                            item.EditStatus = "plus";
                            //从我的应用中减去
                            this.editList[0].SortList.forEach( (el, ii) => {
                                if(el.text == item.text){
                                    this.editList[0].SortList.splice(ii,1);
                                }
                            })
                        }
                    }
                }else{
                    //测试有"取消"（或者其他）按钮
                    this.dialog.tips = "是否跳转详情页，测试弹框问题";
                    this.dialog.hasCancel = true;
                    this.isDialog = true;
                    //this.$router.push({path:'/lifelist'});
                }
            },
            //拖曳事件
            drag(event){
                if(event.path && event.path[0] && event.path[0].dataset.id){
                    this.curStartID = parseInt(event.path[0].dataset.id);  //当前移动的项
                }
                console.log(this.curStartID,"移动时项目id");
            },
            preventOver(event){
                event.preventDefault();
            },
            drop(event){
                let curEndID = -1;//当前移动的项
                if(event.path && event.path[1] && event.path[1].dataset.id){
                    curEndID = parseInt(event.path[1].dataset.id); // 获取当前拖到的位置在所有项中占据的位置
                }
                console.log(curEndID,"移后的位置id");
                //将当前拖动的项判断是否拖动离开运来位置
                //curEndID!=-1 已经移动
                if(curEndID != -1 && this.curStartID != -1 && curEndID != this.curStartID){
                    console.log("已经移动");
                    //改变渲染数据，重新渲染
                    let useList = this.editList[0].SortList;
                    let dragItem = useList[this.curStartID]; //移动的项
                    useList.splice(this.curStartID,1);
                    useList.splice(curEndID,0, dragItem);
                }
            },

            //弹框事件
            dialogConfirm(){
                this.isDialog = false;
            },
            dialogCancel(){
                this.isDialog = false;
            }
        }
    }
</script>
<style scoped lang="less">
@import "../../style/mixin";
.edit_item{
    width: 100%;
    h6{
        padding: 0 30/@rem;
        height: 80/@rem;
        line-height: 80/@rem;
        font-size: 32/@rem;
        display: flex;
        justify-content: space-between;
        span{
            font-size: 28/@rem;
        }
        .btu_edit{
            width: 120/@rem;
            text-align: right;
            color: @text-minor;
        }
    }
    .edits{
        padding: 0 30/@rem;
    }
    ul{
        width: 100%;
        display: flex;
        flex-flow: row wrap;
        li{
            box-sizing: border-box;
            position: relative;
            // margin: 10/@rem 1.5%;
            flex: 0 0 25%;
            text-align: center;
            .e_icon{
                margin: 20/@rem auto 0;
                width: 80%;
                display: block;
                height: 80/@rem;
                border-radius: 50%;
            }
            p{
                padding: 20/@rem 0;
                color: @text-primary;
            }
            .e_status{
                position: absolute;
                right: 5%;
                top: 20/@rem;
                z-index: 1;
                span{
                    display: block;
                    border-radius: 50%;
                    width: 30/@rem;
                    height: 30/@rem;
                    line-height: 30/@rem;
                    color: #fff;
                }
                .e_minus{
                    background: #ff4444;
                }
                .e_plus{
                    background: #1336ee;
                }
            }

        }
    }
}

</style>

