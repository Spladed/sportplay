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

                <!--用户列表 border边框 stripe隔行变色-->
                <el-table :data="userList" border stripe>
                    <!--索引列-->
                    <el-table-column type="index"></el-table-column>
                    <el-table-column label="用户名" prop="username"></el-table-column>
                    <el-table-column label="邮箱" prop="email"></el-table-column>
                    <el-table-column label="密码" prop="password"></el-table-column>
                    <el-table-column label="角色" prop="role"></el-table-column>
                    <el-table-column label="状态" prop="state">
                        <!--作用域插槽-->
                        <template slot-scope="scope">
                            <!-- scope.row 每一行封存的数据-->
                            <el-switch v-model="scope.row.state"></el-switch>
                        </template>
                    </el-table-column>
                    <el-table-column label="操作">
                        <template slot-scope="scope">
                            <!-- 修改 -->
                            <el-button type="primary" icon="el-icon-edit" size="mini" @click="showEditDialog(scope.row.id)"></el-button>
                            <!-- 删除 -->
                            <el-button type="danger" icon="el-icon-delete" size="mini" @click="deleteUser(scope.row.id)"></el-button>
                            <!-- 权限 -->
                            <el-tooltip effect="dark" content="分配权限" placement="top-start" :enterable="false">
                                <el-button type="warning" icon="el-icon-setting" size="mini"></el-button>
                            </el-tooltip>
                        </template>
                    </el-table-column>
                </el-table>
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
            //  查询信息实体
            queryInfo: {
                query: "",  //查询信息
                pageNum: 1,     //当前页
                pageSize: 5,    //每页最大数
            },
            userList: [],    //用户列表
            total: 0,    //总记录数
        }
    },
    methods: {
        // 获取所有用户
        async getUserList() {
            const {data: res} = await this.$http.get("alluser", {params: this.queryInfo});
            this.userList = res.data;
            this.total = res.numbers;
        },
    },
}
</script>

<style lang="less" scoped>
.el-breadcrumb {
    margin-bottom: 15px;
    font-size: 17px;
}

</style>