<template>
    <div> 用户列表
        <!--面包屑导航-->
        <el-breadcrumb separator-class="el-icon-arrow-right">
            <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>权限管理</el-breadcrumb-item>
            <el-breadcrumb-item>用户列表</el-breadcrumb-item>
        </el-breadcrumb>
        <!--用户列表主体部分-->
        <el-card>
            <!--间宽-->
            <el-row :gutter="25">
                <!--列宽-->
                <el-col :span="10">
                    <!--搜索区域-->
                    <el-input placeholder="请输入搜索内容">
                        <el-button slot="append" icon="el-icon-search" @click="getUserList"></el-button>
                    </el-input>
                </el-col>

                <el-col :span="4">
                    <el-button type="primary" @click="addDialogVisible = true">添加用户</el-button>
                </el-col>
            </el-row>
        </el-card>
    </div>
</template>

<script>
export default {
    created() {
        this.getUserList();
    },
    data() {
        return {

        }
    },
    methods: {
        // 获取所有用户
        getUserList() {
            console.log(123);
        },

    }

}
</script>

<style lang="less" scoped>
.el-breadcrumb {
    margin-bottom: 15px;
    font-size: 17px;
}

</style>