	<!--
用户详情信息组件界面
author：risatoar
date：2017/12/26
-->
<template>
	<!-- 用户详情信息组件主体 -->
	<div class="home-userdetail">
		<!-- 用户详情信息组件背景层 -->
		<div class="userdetail-wrap" style="position: relative">
			<h3>用户信息</h3>
			 <Button type="warning" v-if="userd.isBan == true" style="position: absolute; top: 45px; right:20px;" @click="showUnBan()">申请解封</Button>
			 <Modal
		         v-model="modal1">
		         <p slot="header" style="color:#f60;text-align:center">
                    <Icon type="information-circled"></Icon>
                    <span>申请解封</span>
                </p>
                <div style="text-align:center;font-size:16px">
                    解封理由: {{ userd.reason }}
                     <p>
					    <Button @click="handleRender">填写解封理由</Button>
					</p>
                </div>
                <div slot="footer">
                    <Button type="error" size="large" long :loading="modal_loading" @click="onSend">发送申请</Button>
                </div>
		     </Modal>
			<!-- 用户详情信息组件内容层,利用table形式展示,每条数据配备修改按钮 -->
			<table class="table table-striped">
				<!-- 昵称展示及修改 -->
				<tr>
					<td class="td1"><Icon type="pizza"></Icon>  昵称</td>
					<td class="td2">
						<input type="text" maxlength="10" placeholder="填写你的昵称" v-model="userd.nickname" @click="inputShow('edit1','save1')">
					</td>
					<td>
						<span  @click="showSave('edit1','save1')" v-if="edit1">
							<Icon type="edit" style="padding:0 5px;color:#2d8cf0;"></Icon>修改
						</span>
						<span class="save" v-if="save1">
							<span @click="chickSave('edit1','save1','nickname')">保存</span>
							<span @click="hideSave('edit1','save1')">取消</span>
						</span>
					</td>
				</tr>
				<!-- 年龄展示及修改 -->
				<tr>
					<td class="td1"><Icon type="happy-outline"></Icon>  年龄</td>
					<td class="td2">
						<input type="text" maxlength="3" placeholder="填写你的年龄" v-model="userd.age" @click="inputShow('edit2','save2')">
					</td>
					<td>
						<span @click="showSave('edit2','save2')" v-if="edit2">
							<Icon type="edit" style="padding:0 5px;color:#2d8cf0;"></Icon>修改
						</span>
						<span class="save" v-if="save2">
							<span @click="chickSave('edit2','save2','age')">保存</span>
							<span @click="hideSave('edit2','save2')">取消</span>
						</span>
					</td>
				</tr>
				<!-- 真实姓名展示及修改 -->
				<tr>
					<td class="td1"><Icon type="icecream"></Icon>  真实姓名</td>
					<td class="td2">
						<input type="text" maxlength="15" placeholder="填写你的真实姓名" v-model="userd.truename" @click="inputShow('edit3','save3')">
					</td>
					<td>
						<span @click="showSave('edit3','save3')" v-if="edit3">
							<Icon type="edit" style="padding:0 5px;color:#2d8cf0;"></Icon>修改
						</span>
						<span class="save" v-if="save3">
							<span @click="chickSave('edit3','save3','truename')">保存</span>
							<span @click="hideSave('edit3','save3')">取消</span>
						</span>
					</td>
				</tr>
				<!-- 公司名称展示及修改 -->
				<tr>
					<td class="td1"><Icon type="location"></Icon>  公司名称</td>
					<td class="td2">
						<input type="text" maxlength="20" placeholder="填写你的公司名称" v-model="userd.company" @click="inputShow('edit4','save4')">
					</td>
					<td>
						<span @click="showSave('edit4','save4')" v-if="edit4">
							<Icon type="edit" style="padding:0 5px;color:#2d8cf0;"></Icon>修改
						</span>
						<span class="save" v-if="save4">
							<span @click="chickSave('edit4','save4','company')">保存</span>
							<span @click="hideSave('edit4','save4')">取消</span>
						</span>
					</td>
				</tr>
				<!-- 手机号展示及修改 -->
				<tr>
					<td class="td1"><Icon type="ios-telephone"></Icon>  电话/手机号</td>
					<td class="td2">
						<input type="text" maxlength="11" placeholder="填写你的电话/手机号" v-model="userd.telephone" @click="inputShow('edit5','save5')">
					</td>
					<td>
						<span @click="showSave('edit5','save5')" v-if="edit5">
							<Icon type="edit" style="padding:0 5px;color:#2d8cf0;"></Icon>修改
						</span>
						<span class="save" v-if="save5">
							<span @click="chickSave('edit5','save5','telephone')">保存</span>
							<span @click="hideSave('edit5','save5')">取消</span>
						</span>
					</td>
				</tr>
				<!-- 头像上传修改 -->
				<tr>
					<td class="td1"><Icon type="person"></Icon>  头像</td>
					<td class="td2">
						<!-- <input type="text" placeholder="填写你的头像" v-model="userd.icon" @click="inputShow('edit6','save6')"> -->
						<div class="demo-upload-list" v-for="item in uploadList">
						    <template v-if="item.status === 'finished'">
						        <img :src="item.url">
						        <div class="demo-upload-list-cover">
						            <Icon type="ios-eye-outline" @click.native="handleView(item.name)"></Icon>
						            <Icon type="ios-trash-outline" @click.native="handleRemove(item)"></Icon>
						        </div>
						    </template>
						    <template v-else>
						        <Progress v-if="item.showProgress" :percent="item.percentage" hide-info></Progress>
						    </template>
						</div>
						<Poptip trigger="hover" content="点击图标选择头像并上传">
						<Upload
						    ref="upload"
						    :show-upload-list="false"
						    :default-file-list="defaultList"
						    :on-success="handleSuccess"
						    :format="['jpg','jpeg','png']"
						    :max-size="2048"
						    :on-format-error="handleFormatError"
						    :on-exceeded-size="handleMaxSize"
						    :before-upload="handleBeforeUpload"
						    type="drag"
						    action="/uploads"
						    style="display: inline-block;width:58px;">
						    <div style="width: 58px;height:58px;line-height: 58px;">
						        <Icon type="camera" size="20"></Icon>
						    </div>
						</Upload>
						<Modal title="查看大图" v-model="visible">
						    <img :src="'/static/img/uploads/' + imgName " v-if="visible" style="width: 100%">
						</Modal>
					</Poptip>
					</td>
				</tr>
				<!-- 电子邮箱展示及修改 -->
				<tr>
					<td class="td1"><Icon type="email"></Icon>  电子邮箱</td>
					<td class="td2">
						<input type="text" maxlength="30" placeholder="填写你的邮箱" v-model="userd.mail" @click="inputShow('edit7','save7')">
					</td>
					<td>
						<span @click="showSave('edit7','save7')" v-if="edit7">
							<Icon type="edit" style="padding:0 5px;color:#2d8cf0;"></Icon>修改
						</span>
						<span class="save" v-if="save7">
							<span @click="chickSave('edit7','save7','mail')">保存</span>
							<span @click="hideSave('edit7','save7')">取消</span>
						</span>
					</td>
				</tr>
			</table>
		</div>
	</div>
</template>

<script>
import axios from 'axios'
export default {
	props: {
	  userd: {
	    type: Object
	  }
	},
	data() {
		return {
			edit1 : true,
			save1 : false,
			edit2 : true,
			save2 : false,
			edit3 : true,
			save3 : false,
			edit4 : true,
			save4 : false,
			edit5 : true,
			save5 : false,
			edit6 : true,
			save6 : false,
			edit7 : true,
			save7 : false,
			modal1: false,
			defaultList: [],
			modal_loading: false,
			imgName: '',
			visible: false,
			uploadList: [],
			nowUpLoad: ''
		}
	},
	created() {
		this.setDefault()
	},
	computed: {
		icon() {
			return this.$store.state.icon
		}
	},
	methods: {
		setDefault() {
			this.defaultList[0] = {
				name: this.userd.icon,
				url: '/static/img/uploads/' + this.userd.icon
			}
		},
		// 通过axios向后台传对应的个人信息字段修改,通过result传来的不同status值展示不同全局提示
		postChange(attr) {
			console.log(this.attr)
			axios.post("/edit/" + attr,this.userd).then((res)=> {
			    if(res.data.status == 1001){
			        this.success('修改成功')
			    }else if(res.data.status == 1002){
			        this.error('修改失败,密码错误')
			    }else if(res.data.status == 1003){
			    	this.warning('数据没有修改过')
			    }
			    this.$emit('on-change')
			}).catch((error)=> {
			  console.log(error);
			});
		},
		success (msg) {
		    this.$Message.success(msg);
		},
		warning (msg) {
		    this.$Message.warning(msg);
		},
		error (msg) {
		    this.$Message.error(msg);
		},
		// 通过对应参数判断是否展示保存按钮
		showSave(attr1,attr2) {
			this[attr1] = false
			this[attr2] = true
		},
		// 通过对应参数判断是否展示保存按钮
		hideSave(attr1,attr2) {
			this[attr1] = true
			this[attr2] = false
		},
		// hideAllSave() {
		// 	for(let i = 0;i<=8;i++){
		// 		this['save' + i] =false;
		// 		this['edit' + i] =false
		// 	}
		// },
		// 确认保存,调用postChange函数
		chickSave(attr1,attr2,attr3) {
			this[attr1] = true
			this[attr2] = false
			console.log(attr3)
			this.postChange(attr3)
		},
		// 点击对应数据的input框展示保存及取消按钮
		inputShow(attr1,attr2) {
			this[attr1] = false
			this[attr2] = true
		},
		success () {
            this.$Message.success('保存成功');
        },
        // 图片名称获取
        handleView (name) {
            this.imgName = name;
            this.visible = true;
        },
        // 图片列表展示
        handleRemove (file) {
            const fileList = this.$refs.upload.fileList;
            this.$refs.upload.fileList.splice(fileList.indexOf(file), 1);
        },
        // 上传图片路径成功回调
        handleSuccess (res, file) {
            file.url = 'http://localhost:81/static/img/uploads/' + res.filename;
            file.name = res.filename;
            this.imgName = res.filename;
            this.userd.icon = res.filename;
            this.defaultList[0].name = file.name;
            this.defaultList[0].url = file.url;
            this.$store.commit("updateIcon",file.name)
            const fileList = this.$refs.upload.fileList;
            this.$refs.upload.fileList.splice(fileList.indexOf(file), 1);

            axios.post("/edit/icon",this.userd).then((res)=> {
			    if(res.data.status == 1001){
			        this.success('头像设置成功')
			    }else if(res.data.status == 1002){
			        this.error('头像设置失败')
			    }else if(res.data.status == 1003){
			    	this.warning('请选择不同的头像进行上传')
			    }
			}).catch((error)=> {
			  console.log(error);
			  this.error('上传失败,请重新尝试上传或者换一个头像进行上传')
			});
        },
        // 判断图片格式
        handleFormatError (file) {
            this.$Notice.warning({
                title: 'The file format is incorrect',
                desc: 'File format of ' + file.name + ' is incorrect, please select jpg or png.'
            });
        },
        // 判断图片大小
        handleMaxSize (file) {
            this.$Notice.warning({
                title: 'Exceeding file size limit',
                desc: 'File  ' + file.name + ' is too large, no more than 2M.'
            });
        },
        // 判断同时上传的图片是否大于1张
        handleBeforeUpload () {
            const check = this.uploadList.length < 2;
            if (!check) {
                this.$Notice.warning({
                    title: '只能同时上传一张图片'
                });
            }
            return check;
        },
        showUnBan() {
        	this.modal1 = true;
        },
        onSend() {
           this.modal_loading = true;
           setTimeout(() => {
               this.modal_loading = false;
               this.modal2 = false;
               this.postChange("reason")
               this.$Message.success('发送成功');
           }, 2000);
        },
        handleRender () {
            this.$Modal.confirm({
                render: (h) => {
                    return h('Input', {
                        props: {
                            value: this.userd.reason,
                            autofocus: true,
                            placeholder: '请输入解封理由...'
                        },
                        on: {
                            input: (val) => {
                                this.userd.reason = val;
                            }
                        }
                    })
                }
            })
        }
    },
    mounted () {
        this.uploadList = this.$refs.upload.fileList;
    }
}
</script>

<style scoped>

h3{
	font-weight: bold;
	padding: 30px 0;
	float: left;
	padding-left: 20px;
}
td{
	font-family: Main Head;
	font-size: 16px;
	color: #464c5b;
	clear: both;
	height: 50px;
	width: 195px;
	vertical-align:middle;
}
span{
	padding: 0 5px;
	color: #2d8cf0;
}
input{
	text-align: left;
	flex: 1;
	-webkit-box-flex: 1;
    -ms-flex: 1;
    display: inline-block;
    border: none;
    outline: none;
    color: #909090;
    overflow: visible;
}
.td1{
	padding-left: 20px;
	text-align: left;
}
.td2{
	text-align: left;
}

/*上传图片demo*/
.demo-upload-list{
    display: inline-block;
    width: 60px;
    height: 60px;
    text-align: center;
    line-height: 60px;
    border: 1px solid transparent;
    border-radius: 4px;
    overflow: hidden;
    background: #fff;
    position: relative;
    -webkit-box-shadow: 0 1px 1px rgba(0,0,0,.2);
            box-shadow: 0 1px 1px rgba(0,0,0,.2);
    margin-right: 4px;
}
.demo-upload-list img{
    width: 100%;
    height: 100%;
}
.demo-upload-list-cover{
    display: none;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0,0,0,.6);
}
.demo-upload-list:hover .demo-upload-list-cover{
    display: block;
}
.demo-upload-list-cover i{
    color: #fff;
    font-size: 20px;
    cursor: pointer;
    margin: 0 2px;
}
</style>