<template>
  <!-- 遍历传来的路由数组 -->
  <template v-for="(item, index) in menuList" :key="item.path">
    <!-- 如果没有子路由，就用这种写法： -->
    <template v-if="!item.children">
        <el-menu-item :index="item.path" v-if="!item.meta.hidden" @click="goRoute">
            <!-- 具名插槽 -->
            <el-icon>
                <component :is="item.meta.icon"></component>
              </el-icon>
            <template #title>
              <span>{{ item.meta.title }}</span>
            </template>
        </el-menu-item>
    </template>

    <!-- 只有一个子路由，也不需要折叠 -->
    <template v-if="item.children && item.children.length == 1" >
        <el-menu-item :index="item.children[0].path" v-if="!item.children[0].meta.hidden" @click="goRoute">
            <el-icon>
                  <component :is="item.children[0].meta.icon"></component>
                </el-icon>  
            <template #title>
              <span>{{ item.children[0].meta.title }}</span>
            </template>
        </el-menu-item>
    </template>

    <!-- 有子路由且个数大于1 -->
    <template v-if="item.children && item.children.length > 1">
        <el-sub-menu :index="item.path" v-if="!item.meta.hidden"> 
            <template #title>
              <el-icon>
                  <component :is="item.meta.icon"></component>
              </el-icon>  
                <span>{{ item.meta.title }}</span>
            </template>
            <!-- 递归组件 -->
            <Menu :menuList="item.children"></Menu>
        </el-sub-menu>
    </template>
  </template>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'; 
//获取父组件传递过来的全部路由数组
defineProps(['menuList']);
//获取路由器对象
let $router=useRouter()
//点击菜单的回调
const goRoute=(vc:any)=>{
  //路由跳转
  $router.push(vc.index)
}
</script>

<script lang="ts">
export default {
  name:'Menu'
}
</script>

<style scoped>

</style>