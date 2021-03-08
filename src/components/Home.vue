<template>
    <!--使用el-container就不能使用div了-->
    <el-container class="home-container">
        <!--头部-->
        <el-header>
            <div>
                <img src="../assets/logo.png" class="logo-img" alt/>
                <span>个人运动平台</span>
            </div>
            <el-button type="info" @click="logout">安全退出</el-button>
        </el-header>
        <!--主体-->
        <el-container>
            <!--侧边栏-->
            <el-aside :width="isCollapse?'64px':'200px'">
                <div class="toggle-button" @click="toggleCollapse">|||</div>
                <el-menu background-color="#545c64" text-color="#fff" active-text-color="#409eff"
                         unique-opened :collapse="isCollapse" :collapse-transition="false"
                         :router="true" :default-active="activePath">
                    <!-- 一级菜单 -->
                    <el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id">
                        <template slot="title">
                            <!--每一行前的图标。如果想让每行的图标不一样，请看P23-->
                            <!--<i class="el-icon-location"></i>-->
                            <span>{{ item.title }}</span>
                        </template>
                        <!-- 二级菜单 -->
                        <el-menu-item :index="it.path" v-for="it in item.slist" :key="it.id" @click="saveNavState(it.path)">
                            <template slot="title">
                                <!--<i class="el-icon-location"></i>-->
                                <span>{{ it.title }}</span>
                            </template>
                        </el-menu-item>
                    </el-submenu>
                </el-menu>
            </el-aside>
            <!--主体内容-->
            <el-main>
                <router-view></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>

<script>
export default {
    data() {
        return {
            // 菜单列表
            menuList: [],
            // 伸缩
            isCollapse: false,
            // 默认路径
            activePath: '/welcome',
        }
    },
    // 页面一加载就做的事
    created() {
        // 查询menuList
        this.getMenuList();
        // 取出session中的path，动态修改activePath
        this.activePath=window.sessionStorage.getItem('activePath');
    },
    methods: {
        // 安全退出
        logout() {
            window.sessionStorage.clear();  //清除session
            this.$router.push("/login");    //回到首页
        },
        // 获取导航菜单方法
        async getMenuList() {
            const {data: res} = await this.$http.get("menus");
            console.log(res)
            if (res.flag != 200) return this.$message.error("获取列表失败");   //访问失败的错误信息
            this.menuList = res.menus;    //访问成功的数据回填
        },
        // 控制侧边栏伸缩
        toggleCollapse() {
            this.isCollapse = !this.isCollapse;
        },
        // 保存路径
        saveNavState(activePath){
            // 把activePath放在session中
            window.sessionStorage.setItem('activePath',activePath);
            this.activePath=activePath;
        }
    }
}
</script>

<style lang='less' scoped>
//布局器样式
.home-container {
    height: 100%;
}

//头部样式
.el-header {
    background-color: #373d41;
    display: flex;
    justify-content: space-between; //左右贴边
    padding-left: 0%; //左边界
    align-items: center;
    color: #fff;
    font-size: 20px;

    > div {
        display: flex;
        align-items: center;

        span {
            margin-left: 15px;
        }
    }
}

//侧边栏样式
.el-aside {
    background-color: #333744;

    .el-menu {
        border-right: none;
    }
}

//主题内容样式
.el-main {
    background-color: #eaedf1;
}

.logo-img {
    width: 55px;
    height: 55px;
}

//按钮样式
.toggle-button {
    background-color: #4a5064;
    font-size: 10px;
    line-height: 24px;
    color: #fff;
    text-align: center;
    letter-spacing: 0.2em;
    cursor: pointer;
}
</style>