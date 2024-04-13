<template>
    
  <el-container  class="bodypage" style="width: 100vw; height: 100vh;background-color: #ffffff">
    <el-header>
      <h1 style="color:#ffffff">基于数字孪生的可视化智能流控管理平台</h1>
    </el-header>
    
    <el-main style="background-color: #fbfdff;opacity: 0.7;">
      <slot name="main">
        <el-carousel  indicator-position="outside" height="70vh" arrow="never">
          <!-- 临时列表 -->
          <el-carousel-item v-for="item in 1" :key="item">
            <div class="boxContentPc">
              
              <div class="boxTextPc">
                <div class="textShowPc">Visual Intelligent Flow Control Management Platform Based On Digital Twin</div>
                <div class="textShowPc">基于数字孪生的可视化智能流控管理平台</div>
                <div class="textCoPc">数字孪生是充分利用物理模型、传感器更新、运行历史等数据，集成多学科、
                    多物理量、多尺度、多概率的仿真过程，在虚拟空间中完成映射，从而反映相对应的实体装备的全生
                    命周期过程。数字孪生是一种超越现实的概念，可以被视为一个或多个重要的、彼此依赖的装备系统的数字映射系统。</div>
                <div style="padding-top:50px" class="textButtonPc">
                  <el-button
                    class="buttons"
                    color="#626aef"
                    size="large"
                    @click="toPage(1, 1, false)"
                    >开  始</el-button
                  >
                  <!-- <div class=" cursor-pointer text-right" style="margin-top: 6px;margin-bottom: -8px;"
						@click="toPage(1, 1, true)">点击查看演示</div> -->
                  <!-- :dark="isDark" -->
                </div>
              </div>
            </div>
          </el-carousel-item>
        </el-carousel>
      </slot>
    </el-main>
    <el-footer height="40px" style="color: #909399; text-align: center"
      >MayCopyright © 2024 || PC(Google Chrome/Microsoft
      Edge)</el-footer
    >
  </el-container>
</template>

<script  setup lang="ts">
// import { useRouter } from "vue-router";
// const router = useRouter();
import { useRouter, useModel } from '@fesjs/fes' //fesJS的路由被他自己封装了
import { UserOutlined } from '@fesjs/fes-design/icon'

const { menuSetup } = useModel('forPreview')
let publicPath = process.env.BASE_URL

const router = useRouter()
// const toPage = () => {
// 	let path = `/plugins/digitalCity/pages/fog`
		
// 	let routeUrl = router.resolve({
// 		path: path
// 	})
// 	window.open(routeUrl.href, '_blank')
// }

const toPage = (plugin: any, value: any, isOnline: boolean) => {
    plugin = {
    "name": "digitalCity",
    "title": "",
    "intro": "",
    "version": "0.0.1",
    "author": "",
    "website": "https://gitee.com/hawk86104",
    "state": "active",
    "require": [
        "cannon-es"
    ],
    "preview": []
        
};
    value = "buildingsPassA";
	let path = `/plugins/${plugin.name}/${value}`
	if (plugin.pNode) {
		path = `/plugins/${plugin.pNode}/${plugin.name}/${value}`
	}
	if (isOnline) {
		path = 'https://opensource.icegl.cn/#' + path
		window.open(path, '_blank')
	} else {
		let routeUrl = router.resolve({
			path: path
		})
		window.open(routeUrl.href, '_blank')
	}
}

</script>
<style scoped>
.el-carousel__item h3 {
  display: flex;
  color: #526f94;
  opacity: 0.75;
  line-height: 300px;
  margin: 0;
}

.el-carousel__item:nth-child(n) {
  /* background-color: #fbfdff;
  opacity: 0.6; */
}
.boxContent {
  display: flex;
  justify-content: space-between;
  align-items: center;
  overflow: hidden;
  width: 85%;
  height: 100%;
  margin: 0px auto;
}
.boxContentPc {
    
  overflow: hidden;
  width: 90%;
  height: 100%;
  margin: 0px auto;
}
.boxText {
  width: 45%;
  height: 70%;
  margin: 0px auto;
  text-align: left;
}
.boxTextPc{
  width: 100%;
  height: 70%;
  margin: 0px auto;
  text-align: left;
}
.boxPage {
  width: 460px;
  height: 216px;
  margin: 0px auto;
}
.boxPagePc {
  width: 100%;
  height: 50%;
  margin: 0px auto;
}
.textShow {
  padding-top: 1%;
  font-size: 42px;
}
.textShowPc{
  padding-top: 1%;
  font-size: 35px;
}
.textCo {
  padding-top: 2%;
  font-size: 16px;
}
.textCoPc {
  padding-top: 2%;
  font-size: 30px;
}
.textButton {
  padding-top: 5%;
}
.buttons {
  color: rgb(255, 255, 255);
  width: 300px;
  height: 70px;
  font-size: 35px;
}
.buttons:hover {
  color: rgb(227, 251, 255);
}
.bodypage{
    background-image: url("../images/back.jpg");
    opacity: 0.9;
}
.mianback{
    background-color: aqua;
}
</style>
