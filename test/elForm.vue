<template>

        <el-form :model="formData"
                 ref="FORM"
                 label-width="140px"
                 class="demo-ruleForm"
                 style="margin-top:16px;margin-right: 10px"
                 @submit.native.prevent
                 :rules="Validate">
            <template v-for="(item,index) in formData">
                <el-form-item
                        :label="item.label"
                        :prop="item.prop"
                        :width="item.width"
                        :key="index"
                        >
                    <div v-if="item.type==='input'">
                    <el-input v-model="formData.name" :placeholder="item.placeholder"  :style={width:item.width}></el-input>
                    </div>
                    <div v-if="item.type==='select'">
                        <el-select v-model="formData.svcId"
                                   :placeholder="item.placeholder"
                                   >
                            <el-option
                                    v-for="item in options"
                                    :key="item.key"
                                    :label="item.label"
                                    :value="item.value">
                            </el-option>
                        </el-select>
                    </div>
                </el-form-item>
            </template>
        </el-form>
</template>

<script>
    export default {
        name: "addContacts",
        props: ["newdialog",'formData','editContent','Validate'],
        data(){
            return {
                title:'',
                loading:false,
                create:{
                    name:'',
                    phone:'',
                    email:'',
                    id:''
                },
                Validate : {
                    name: [
                        { required: true, message: '不能为空', trigger: 'blur'}
                    ],
                    phone: [
                        { required: true, message: '不能为空', trigger: 'blur'}
                    ]
                }
            }
        },

        watch:{
            newdialog(n) {
                this.clearFormFields();
                //开关为关闭 不向下执行
                if(!n){return}
                if(typeof this.editContent == 'object'){
                    this.title = '编辑';
                    this.edit()
                    return
                }
                if (n) {
                    this.loading = false;
                    this.title = '新增';
                }
            }
        },
        methods:{
            edit(){
                setTimeout(()=>{
                    Object.keys(this.create).forEach(x=>{
                        this.create[x] =  this.editContent[x]
                        console.log(this.create[x],this.editContent[x]);
                    })
                })
            },
            ok() {
                this.loading = true;
                let params = {...this.create};
                let api = null;
                if  (this.title === '新增'){
                    api = 
                    delete params.id
                    params = [params]
                }else{
                    api = 
                }
                this.$refs['create'].validate((valid)=> {
                    console.log(valid);
                    this.$http[api](params).then(res => {
                        if (res.data.rtnCode == 1) {
                            this.$message.success(res.data.msg)
                            this.loading = false;
                            this.cancel()
                            this.$emit('search')
                        } else {
                            this.loading = false;
                        }
                    });

                });
            },
            cancel() {
                this.$emit("cancel_Contanacts");
            },
            clearFormFields() {
                this.$nextTick(() => {
                    this.$refs['FORM'].resetFields();
                });
            },
        }
    }
</script>

<style scoped>

</style>