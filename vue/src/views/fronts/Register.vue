<template>
    <div class="container">
        <div class="login-box">
            <h2 class="title" @click="$router.push('/front')">{{ $sysTitle }}</h2>
            <el-form :model="form" label-position="right" label-width="80px">
                <el-form-item label="账号">
                    <el-input placeholder="请输入账号" v-model="form.username" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input type="password" placeholder="请输入密码" v-model="form.password" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="姓名">
                    <el-input placeholder="请输入姓名" v-model="form.name" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="性别">
                    <el-select v-model="form.gender" placeholder="请选择">
                        <el-option v-for="item in genderOptions" :key="item.value" :label="item.label" :value="item.value">
                        </el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="手机">
                    <el-input placeholder="请输入手机" v-model="form.phone" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="身份证">
                    <el-input placeholder="请输入身份证" v-model="form.idcard" autocomplete="off"></el-input>
                </el-form-item>
<!--                <el-form-item label="头像">-->
<!--                    <el-upload name="avatar" class="upload-demo" action="#" :http-request="uploadHeadFile"-->
<!--                        :file-list="fileList" list-type="picture">-->
<!--                        <el-button size="small" type="primary">点击上传</el-button>-->
<!--                    </el-upload>-->
<!--                </el-form-item>-->
                <el-form-item>
                    <el-button type="primary" style="width: 280px;" @click="submitForm">注册</el-button>
                    <div class="text-right">
                        已有账号?去<router-link to="/login">登录</router-link>
                    </div>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
import { registerAPI } from '@/api/system'
import request from '@/utils/request'
export default {
    name: 'Register',
    data() {
        return {
            form: {
                username: '', // 如果需要，可以添加更多字段
                password: '',
                name: '',
                gender: 1,
                phone: '',
                idcard: '',
                // 根据需要添加更多字段
            },
            genderOptions: [
                {
                    label: '男',
                    value: 1
                },
                {
                    label: '女',
                    value: 2
                }
            ],
            fileList: []
        }
    },
    methods: {
        async submitForm() {
            // 检查是否有任何必填字段为空
            for (const key of Object.keys(this.form)) {
                if (!this.form[key]) {
                    this.$message.error('注册信息不完整');
                    return; // 如果有任何字段为空，则阻止注册
                }
            }

            // 所有字段都填写了，继续注册
            const res = await registerAPI(this.form)
            this.$message({
                message: res.message,
                type: res.flag ? "success" : "error",
            });
            if (res.flag) {
                setTimeout(() => {
                    this.$router.push('/login')
                }, 1000);
            }
        },
        uploadHeadFile(item) {
            // 你的上传逻辑保持不变
        },
    }
}
</script>

<style lang="less" scoped>
.container {
    width: 100vw;
    height: 100vh;
    //background-color: #282c35;
    background: url(@/assets/酒店.PNG);
    background-size: 100%;
    display: flex;

    .login-box {
        padding: 10px;
        border-radius: 10px;
        min-width: 400px;
        min-height: 200px;
        background-color: aliceblue;
        margin: auto;
        padding-right: 40px;

        .title {
            text-align: center;
            cursor: pointer;
            margin: 20px 0;
            width: 110%;
            &:hover{
                color: #409eff;
            }
        }
    }
}
</style>
