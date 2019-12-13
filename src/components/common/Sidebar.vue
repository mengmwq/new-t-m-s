<template>
    <div class="sidebar">
        <el-menu
                class="sidebar-el-menu"
                :default-active="onRoutes"
                :collapse="collapse"
                background-color="#324157"
                text-color="#bfcbd9"
                active-text-color="#20a0ff"
                unique-opened
                router
        >
            <template v-for="item in items">
                <template >
                    <el-menu-item :key="item.index"   v-if="item.title!=='权限管理'">
                        <i :class="item.icon"></i>
                        <!--        <span slot="title" >{{ item.title }}</span>-->
                            <a style="color: #ffff" :href='item.index' target='_blank'>{{ item.title }}</a>


                    </el-menu-item>
                    <el-menu-item :key="item.index"  :index="item.index" v-else>
                        <i :class="item.icon"></i>
                        <span slot="title" >{{ item.title }}</span>



                    </el-menu-item>
                </template>
            </template>
        </el-menu>
    </div>
</template>

<script>
import bus from '../common/bus';
export default {
    data() {
        return {
            collapse: false,
            items: [
                {
                    icon: 'el-icon-lx-home',
                    index: 'http://www.zjcoldcloud.com/dist/index.html#/login',
                    title: 'TMS运单管理系统'
                },
                {
                    icon: 'el-icon-lx-cascades',
                    index: 'http://out.ccsc58.cc/OMS/dist/#/login',
                    title: 'OMS订单管理系统'
                },
                {
                    icon: 'el-icon-lx-copy',
                    index:'table',

                    title: '权限管理'
                },

            ]
        };
    },
    computed: {
        onRoutes() {
            return this.$route.path.replace('/', '');
        }
    },
    created() {
        // 通过 Event Bus 进行组件间通信，来折叠侧边栏
        bus.$on('collapse', msg => {
            this.collapse = msg;
            bus.$emit('collapse-content', msg);
        });
    }
};
</script>

<style scoped>
.sidebar {
    display: block;
    position: absolute;
    left: 0;
    top: 70px;
    bottom: 0;
    overflow-y: scroll;
}
.sidebar::-webkit-scrollbar {
    width: 0;
}
.sidebar-el-menu:not(.el-menu--collapse) {
    width: 250px;
}
.sidebar > ul {
    height: 100%;
}
</style>
