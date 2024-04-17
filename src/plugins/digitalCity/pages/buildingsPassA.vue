<!--
 * @Description: 
 * @Version: 1.668
 * @Autor: 地虎降天龙
 * @Date: 2023-12-15 10:58:31
 * @LastEditors: 地虎降天龙
 * @LastEditTime: 2023-12-18 11:05:32
-->

<template>
	<pagesShow :disableRender="true" :showAxesHelper="false">
		<template v-slot:ability>
			<buildingsPassA v-bind="passState" />
      <Suspense >
				<fireB v-if="fireShowBase==true"/>
			</Suspense>
      <Suspense >
				<fireC v-if="fireShowBase==true"/>
			</Suspense>
      <Suspense>
				<precipitation v-if="fireShowBase==true" v-bind="toRefsState" />
		</Suspense>
      <!-- 设备正常状态 -->
      //各类场景添加
<!-- const fireShowBase = ref(false);//火灾场景 


const noguzhang1ShowBase = ref(false);//大区一正常场景 
const guzhang1ShowBase = ref(false);//大区一故障场景 
const noGuzhangShowBase = ref(false);//大区二非故障场景 

const snowShowBase = ref(false);//雪灾场景  -->
      <Suspense>
				<markA v-if="noguzhang1ShowBase" :position="[470, 30, -50]" :scale="0.13" img="./plugins/digitalCity/image/znsba.png" :foremost="false" />
			</Suspense>
			<radraA v-if="noguzhang1ShowBase" :size="200" :radius="200" :position="[470, 30, -50]" color="#66ffff" />
      <!-- //设备故障状态 -->
      <Suspense>
				<markA v-if="guzhang1ShowBase" :position="[470, 30, -50]" :scale="200" img="./plugins/digitalCity/image/znsb-err.png" :foremost="true" :sizeAttenuation="true" />
			</Suspense>
			<radraA v-if="guzhang1ShowBase" :size="200" :radius="200" :position="[470, 30, -50]" color="#ff0000" />
      <!-- 大区二设备 -->
			<Suspense>
				<markA v-if="noGuzhangShowBase" :position="[-410, 19, -260]" :scale="200" img="./plugins/digitalCity/image/znsb-err.png"
					:sizeAttenuation="true" :foremost="true" />
			</Suspense>
			<radraB v-if="noGuzhangShowBase" :position="[-410, 19, -260]" :height="60" color="#ff0000" />
		</template>
    
    <!-- <template v-slot:ability>
			<Suspense>
				<fireB />
			</Suspense>
		</template> -->
	</pagesShow>
  <div class="bottomLight">
    <div class="bottomLightTipF">
        <div class="bottomLightTipFCenter">
          <el-icon :class="CircleCheckFilledxuanz" :style="{
                    color: CircleCheckFilledc
                  }"><Loading /></el-icon>
        </div>
        
    </div>
    <div class="bottomLightTipF">
        <div class="bottomLightTipFCenter">
          <el-icon :style="{
                    color: CircleCheckFilledc
                  }"><CircleCheckFilled /></el-icon>
        </div>
        <button @click="handleExceedChange"> sss</button>
        
    </div>
    <div class="bottomLightTipF">
      
       <div class="bottomLightTipFCenter">
          <el-icon :style="{
                    color: CircleCheckFilledw
                  }"><WarningFilled /></el-icon>
        </div>
    </div>
    <div class="bottomLightTipF">
      <div class="bottomLightTipFCenter">
          <el-icon :style="{
                    color: CircleCheckFilledi
                  }"><InfoFilled /></el-icon>
      </div>
    </div>
  
  </div>

<div class="bs-sysMsg">
        <span style="font-weight: bold;line-height:50px;font-size:23px"
          ><span v-show="sysDidMesStates == false">事件列表</span>
        </span>
        <el-collapse v-model="activeName" accordion>
          <el-collapse-item  name="1">
            <template #title>
              <el-carousel
                style="width: 100%"
                height="49px"
                direction="vertical"
                indicator-position="none"
                :autoplay="false"
                :interval="1000"
              >
                <!-- <el-carousel-item v-for="item in systemMsgNode" :key="item.id">
                <span>{{ item.title }}</span>
              </el-carousel-item> -->
                <el-carousel-item>
                  <span>点击查看基本消息</span>
                </el-carousel-item>
              </el-carousel>
              <!-- <span>点击查看节点消息</span> -->
            </template>
            <!-- <c-scrollbar maxHeight="100%" trigger="hover"> -->
            <c-scrollbar maxHeight="280px" height="280px" trigger="hover">
              <div v-for="item in nodeMesVisList" class="event-content">
                <div class="event-mes-block-node">
                  {{ item.mes }}
                </div>
                <!-- <el-icon :size="18" class="iconfont">
                    <Edit />
                  </el-icon> -->
                <div class="event-detail" @click="showNodeDetial(item)">
                  <el-icon class="event-detail-buttom"><MoreFilled /></el-icon>
                </div>
              </div>
            </c-scrollbar>
          </el-collapse-item>
          <el-collapse-item name="2">
            <template #title>
              <el-carousel
                style="width: 100%"
                height="49px"
                direction="vertical"
                indicator-position="none"
                :autoplay="true"
                :interval="1000"
              >
                <!-- <el-carousel-item v-for="item in systemMsgBlock" :key="item.id">
                <span>{{ item.title }}</span>
              </el-carousel-item> -->
                <el-carousel-item>
                  <span>点击查看故障消息</span>
                </el-carousel-item>
              </el-carousel>
              <!-- <span>点击查看区块消息</span> -->
            </template>
            <c-scrollbar maxHeight="280px" height="280px" trigger="hover">
              <div v-for="item in blockMesVisList" class="event-content">
                <div class="event-mes-block-node">
                  {{ item.mes }}
                </div>
                <!-- <el-icon :size="18" class="iconfont">
                    <Edit />
                  </el-icon> -->
                <div class="event-detail" @click="showBlockDetial(item)">
                  <el-icon class="event-detail-buttom"><MoreFilled /></el-icon>
                </div>
              </div>
            </c-scrollbar>
          </el-collapse-item>
          <el-collapse-item name="3">
            <template #title>
              <el-carousel
                style="width: 100%"
                height="49px"
                direction="vertical"
                indicator-position="none"
                :autoplay="true"
                :interval="1000"
              >
                <!-- <el-carousel-item v-for="item in systemMsgBlock" :key="item.id">
                <span>{{ item.title }}</span>
              </el-carousel-item> -->
                <el-carousel-item>
                  <span>点击查看修复消息</span>
                </el-carousel-item>
              </el-carousel>
              <!-- <span>点击查看区块消息</span> -->
            </template>
            <c-scrollbar maxHeight="280px" height="280px" trigger="hover">
              <div v-for="item in blockMesVisList" class="event-content">
                <div class="event-mes-block-node">
                  {{ item.mes }}
                </div>
                <!-- <el-icon :size="18" class="iconfont">
                    <Edit />
                  </el-icon> -->
                <div class="event-detail" @click="showBlockDetial(item)">
                  <el-icon class="event-detail-buttom"><MoreFilled /></el-icon>
                </div>
              </div>
            </c-scrollbar>
          </el-collapse-item>
        </el-collapse>
      </div>

  




  <el-dialog
    v-model="dialogWholeLatencyVisible"
    :show-close="false"
    width="50%"
	center
  >
  <template #header="{ titleId, titleClass }">
      <div class="my-header">
        <span style="font-size:28px" :id="titleId" :class="titleClass">流程控制配置选择</span>
        
      </div>
   </template>
		<el-row :gutter="20" justify="center">
			<el-col :span="8" style="display: flex; justify-content: center; height:300px"><div class="grid-content ep-bg-purple" />
				
				  <el-upload
					ref="upload"
					class="upload-demo"
					action="https://run.mocky.io/v3/9d059bf9-4660-45f2-925d-ce80ad6c4d15"
					:limit="1"
					:on-exceed="handleExceed"
					:auto-upload="false"
					:on-change="handleChange"
					:before-remove="beforeRemove"
				>
					<template #trigger>
					<el-button class="buttonSize" type="primary" @click="findUserSimList" size="large">导入数据</el-button>
					</template>
					<template #tip>
					<div style="font-size:18px" class="el-upload__tip text-red">
						限制XXX类型文件，新文件会覆盖旧文件
					</div>
					</template>
					
				</el-upload>
				
			</el-col>
			<el-col :span="8" style="display: flex; justify-content: center; height:300px"><div class="grid-content ep-bg-purple" />
				<el-button class="buttonSize" type="primary" @click="findaddUserSimList" size="large">自适应配置数据</el-button>
			</el-col>
		</el-row>
		 
         <template #footer>
			<div class="dialog-footer">
				<el-button  type="info" @click="dialogWholeLatencyVisible = false" size="large">关闭</el-button>
				<el-button v-show="sysMesStates" type="success" @click="handleEndFile" size="large">开始</el-button>
			</div>
		 </template>
  </el-dialog>
	<el-dialog
          v-model="dialogWholeSimVisible"
          :title="流程控制选择"
          width="76%"
          :close-on-click-modal="false"
          center
        >
          <el-card>
            <el-steps
              style="display: flex; justify-content: center"
              :space="200"
              :active="activeIndex - 0"
              align-center
              finish-status="success"
            >
              <el-step title="基本信息"></el-step>
              <el-step title="节点配置"></el-step>
              <el-step title="区块配置"></el-step>
              <el-step title="网络配置"></el-step>
              <el-step title="完成配置"></el-step>
            </el-steps>
            <div class="cardContent">
              <div class="mesBox" v-show="activeIndex == 0">
                <div v-show="activeIndex == 0">
                  <h2>  
                    数字孪生是充分利用物理模型、传感器更新、运行历史等数据，集成多学科、
                    多物理量、多尺度、多概率的仿真过程，在虚拟空间中完成映射，从而反映相对应的实体装备的全生
                    命周期过程。数字孪生是一种超越现实的概念，可以被视为一个或多个重要的、彼此依赖的装备系统的数字映射系统。
                  </h2>

                </div>
              </div>
              <div class="mesBox" v-show="activeIndex == 4">
                <div v-show="activeIndex == 4">
                  <h2>
                    数字孪生是充分利用物理模型、传感器更新、运行历史等数据，集成多学科、
                    多物理量、多尺度、多概率的仿真过程，在虚拟空间中完成映射，从而反映相对应的实体装备的全生
                    命周期过程。数字孪生是一种超越现实的概念，可以被视为一个或多个重要的、彼此依赖的装备系统的数字映射系统。
                  </h2>
                  <div class="dataImport">
                    <!-- <el-button type="primary" size="large">导入数据</el-button> -->
                    <!-- <el-button type="primary" @click="nextEnd"
                      >直接获取仿真结果</el-button
                    > -->
                  </div>
                </div>
              </div>
              <div
                class="cardSeting"
                v-show="activeIndex != 0 && activeIndex != 4"
              >
                <div v-show="activeIndex == 1">
                
                </div>
                <div v-show="activeIndex == 2">
                 
                </div>
                <div v-show="activeIndex == 3">
                  
                </div>
              </div>
              <div
                class="cardSeting2"
                v-show="activeIndex != 0 && activeIndex != 4"
              >
                <div v-show="activeIndex == 1">
                
                </div>
                <div v-show="activeIndex == 2">
                 
                </div>
                <div v-show="activeIndex == 3">
                 
                </div>
              </div>
            </div>
          </el-card>
          <template #footer>
            <span class="dialog-footer">
              <el-button v-if="activeIndex != 0" @click="activeIndex--">上一步</el-button>
              <el-button
                type="primary"
                @click="next"
                v-text="activeIndex == 4 ? `开始` : `下一步`"
              ></el-button>
            </span>
          </template>
    </el-dialog>
</template>

<script setup lang="ts">
import pagesShow from '../components/pagesShow.vue'
import buildingsPassA from '../components/buildings/buildingsPassA.vue'
import fireB from '../components/fire/fireB.vue'//火灾
import fireC from '../components/fire/fireC.vue'//火灾
import precipitation from '../components/weather/precipitation.vue';//极寒天气
import { ElMessage, ElMessageBox,ElLoading  } from 'element-plus'
import { reactive,ref, toRefs } from 'vue'
import { Pane } from 'tweakpane'
import markA from '../components/buildings/buildingsMarkA.vue'
import radraA from '../components/radras/radraA.vue'
import radraB from '../components/radras/radraB.vue'
import { genFileId } from 'element-plus'
import type { UploadInstance, UploadProps, UploadRawFile } from 'element-plus'

import {
  CircleCheckFilled,
  WarningFilled,
  InfoFilled,
  Loading
} from "@element-plus/icons-vue";
import { el, fa } from 'element-plus/es/locale';


const precipitationState = reactive({
	speed: 12,
	size: 15,
	count: 6000,
	color: '#fff',
	type: 'snow',	// snow rain point
})
const areaXYZ = reactive({
	areaX: 1500,
	areaY: 1000,
	areaZ: 1500,
})
const toRefsState = reactive({
	...toRefs(precipitationState),
	...toRefs(areaXYZ),
})
//文件上传
const upload = ref<UploadInstance>()
//底部灯光icon控制
let CircleCheckFilledc = ref("black");
let CircleCheckFilledw = ref("black");
let CircleCheckFilledi = ref("black");

let CircleCheckFilledcl = ref("black");//加载按钮
let CircleCheckFilledxuanz = ref("");//加载旋转按钮

const changeCircleCheckFilledcl = () => {
  if(CircleCheckFilledcl.value == "black"){
    CircleCheckFilledcl.value = "#15ff00" ;
  }
  else{
    CircleCheckFilledcl.value = "black";
  }
}
const changeCircleCheckFilledc = () => {
  if(CircleCheckFilledc.value == "black"){
    CircleCheckFilledc.value = "#15ff00" ;
  }
  else{
    CircleCheckFilledc.value = "black";
  }
}
//旋转开启关闭
const changeCircleCheckFilledxuanz = () => {
  if(CircleCheckFilledxuanz.value == "is-loading"){
    CircleCheckFilledxuanz.value = "" ;
  }
  else{
    CircleCheckFilledxuanz.value = "is-loading";
  }
}
const changeCircleCheckFilledw = () => {
  if(CircleCheckFilledw.value == "black"){
    CircleCheckFilledw.value = "red";
  }else{
    CircleCheckFilledw.value = "black";
  }
    
}
const changeCircleCheckFilled1 = () => {
  if(CircleCheckFilledi.value = "#black"){
    CircleCheckFilledi.value = "#fff200";
  }else{
    CircleCheckFilledi.value = "#black";
  }
    
}
//临时控制加载效果
const handleExceedChange = () => {
  console.log(1111);
  changeCircleCheckFilledcl();
  changeCircleCheckFilledxuanz();
  changeCircleCheckFilledc();
  changeCircleCheckFilledw();
  changeCircleCheckFilled1();
  fireShowBase.value = true;
  setTimeout(() => {
    CircleCheckFilledc.value = "black";
    CircleCheckFilledw.value = "black";
    CircleCheckFilledi.value = "black";
     fireShowBase.value = false;
  }, 3000);
  console.log(1112);
}
//文件上传修改
const handleExceed: UploadProps['onExceed'] = (files) => {
  upload.value!.clearFiles()
  const file = files[0] as UploadRawFile
  file.uid = genFileId()
  upload.value!.handleStart(file)

}

//侧边信息栏
let sysDidMesStates= ref(false);
let sysMesStatesContent= ref("");

const submitUpload = () => {
  upload.value!.submit()
}
//弹出框下一步控制
const activeName = ref("1");

let nodeMesVisList = reactive([
      {
        id: 0,
        content: "节点消息创建",
        mes: "节点消息创建",
        type: "normalMes",
        contentMessage: {
          blockDetail: { blockId: "", blockHeight: "", blockHash: "" },
          tradeTime: "",
          content: { id: null, mes: "区块消息创建" },
          id: null,
          type: "normalMes",
          isOrphan: "false",
          from: "1",
          to: "2",
          miner: "0",
          confirmId: null,
          transactionId: "0",
        },
      },
    ]);
    let blockMesVisList = reactive([
      {
        id: 0,
        content: { mes: "区块消息创建" },
        mes: "节点消息创建",
        type: "normalMes",
        contentMessage: {
          blockDetail: { blockId: "", blockHeight: "", blockHash: "" },
          tradeTime: "",
          content: { id: null, mes: "区块消息创建", blockId: "" },
          id: null,
          type: "normalMes",
          from: "1",
          to: "2",
          miner: "0",
          confirmId: null,
          isOrphan: "false",
          transactionId: "0",
        },
      },
    ]);










//导入后的按钮可视化
let sysMesStates =  ref(false);
//发生改变的情况
//文件读取
let fileList =  reactive([{}]);

let dataJson = reactive({});
const handleChange: UploadProps['onChange'] = (uploadFile, uploadFiles) => {
  fileList = uploadFiles;
  sysMesStates.value = true;
}

const handleEndFile = () => {
  let reader = new FileReader(); //新建一个FileReader
  reader.readAsText(fileList[0].raw, "UTF-8"); //读取文件
  reader.onload = (evt) => {
    //读取文件完毕执行此函数
    try {
      const locakDataJson = JSON.parse(evt.target.result);
      dataJson= locakDataJson;
      console.log(locakDataJson);
      dialogWholeLatencyVisible.value = false;
      const loading = ElLoading.service({
        lock: true,
        text: '加载中......',
        background: 'rgba(0, 0, 0, 0.7)',
      })
      setTimeout(() => {
        loading.close();
        // startDigController();
      }, 3000)
      // dataJson 就是读取的文件内容
    } catch (error) {
      ElMessageBox.alert('解析JSON时出错(JSON文件格式错误)：'+error, 'Title', {
        // if you want to disable its autofocus
        // autofocus: false,
        confirmButtonText: 'OK',
      })
      // 在这里处理错误
    }
  };
}


const open = () => {
  ElMessageBox.alert('This is a message', 'Title', {
    // if you want to disable its autofocus
    // autofocus: false,
    confirmButtonText: 'OK',
  })
}

//删除文件
const beforeRemove: UploadProps['beforeRemove'] = (uploadFile, uploadFiles) => {
  sysMesStates.value = false;
  return ElMessageBox.confirm(
    `Cancel the transfer of ${uploadFile.name} ?`
  ).then(
    () => true,
    () => false
  )
  
}


//配置栏
const dialogWholeSimVisible = ref(false);
//流程控制配置选择
 const dialogWholeLatencyVisible = ref(true);
let activeIndex = ref(0);

const nextEnd = () => {
      dialogWholeSimVisible.value = false;
     
      //基本数据导入
      
      
};
    //下一步
const next = () => {
	console.log()
    if (activeIndex.value == 4) {
        dialogWholeSimVisible.value = false;
        //基本数据导入
        // this.redrsad();
      } else {
        activeIndex.value++;
      }
	
};
const findaddUserSimList  = () =>{
	dialogWholeLatencyVisible.value = false;
	dialogWholeSimVisible.value = true;
}
const passState = reactive({
	color: '#00b4fb',
	uScalenum: 250,
	uScaleone: 82,
	uWidth: 0.2,
	speed: 2.0,
	uPosition: { x: 0, y: 0 },
})

const paneControl = new Pane({
	title: '后期效果',
	expanded: true,
});
paneControl.addBinding(passState, 'color', { label: '圈颜色' })
paneControl.addBinding(passState, 'uScalenum', {
	label: '最大范围',
	min: 1,
	max: 500,
	step: 10,
})
paneControl.addBinding(passState, 'uScaleone', {
	label: '单条圈间距',
	min: 1,
	max: 100,
	step: 1,
})
paneControl.addBinding(passState, 'uWidth', {
	label: '单条圈宽度',
	min: 0,
	max: 1,
	step: 0.01,
})
paneControl.addBinding(passState, 'speed', {
	label: '速度',
	min: 1,
	max: 20,
	step: 1,
})
paneControl.addBinding(passState, 'uPosition', {
	picker: 'inline',
	label: '位置',
	expanded: true,
	x: { min: -1000, max: 1000, step: 10 },
	y: { min: -1000, max: 1000, step: 10 },
});

// export default {
//   components: {},
//   props: {
//   },
//   data() {},
//   created() {},
//   mounted(){},
  
  
//   methods: {},

// }


//各类场景添加
const fireShowBase = ref(false);//火灾场景 


const noguzhang1ShowBase = ref(true);//大区一正常场景 
const guzhang1ShowBase = ref(true);//大区一故障场景 
const noGuzhangShowBase = ref(true);//大区二非故障场景 

const snowShowBase = ref(false);//雪灾场景 


const handleFireStart = () => {
  fireShowBase.value = true;
}
const handleFireEnd = () => {
  fireShowBase.value =false;
}

const handle1guzhangShowBaseEnd = () => {
  guzhang1ShowBase.value = false;
}
const handle1guzhangShowBaseStart = () => {
  guzhang1ShowBase.value = true;
}

const handlenoguzhangShowBaseEnd = () => {
  noguzhang1ShowBase.value = false;
}
const handlenoguzhangShowBaseStart = () => {
  noguzhang1ShowBase.value = true;
}
const handleNoGuStart = () => {
  noGuzhangShowBase.value   = true;
}
const handleNoGuEnd = () => {
  noGuzhangShowBase.value = false;
}

const handleSnowStart = () => {
  snowShowBase.value = true;
}
const handleSnowEnd = () => {
  snowShowBase.value  = false;
}

</script>

<style>
.el-row {
  margin-bottom: 20px;
}
.el-row:last-child {
  margin-bottom: 0;
}
.el-col {
  border-radius: 4px;
  
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.buttonSize{
	width: 250px;
	height: 50px;
	font-size: 22px;
	margin-top: 90px;
}
.bottomLight{
  position: absolute;
  width: 100%;
  height: 100px;
  bottom: 10px;
}

.bottomLightTipF{
  float: left;
  width: 24%;
  height: 100px;
  opacity: 0.8;
  display: flex; 
  justify-content: center;
  
  font-size: 40px;
}
.bottomLightTipFCenter{
  display: flex; 
  width: 13%;
  height: 70%;
  border-radius:50%;
  justify-content: center;
  background-color: aliceblue;
  align-items: center;
}
.bs-sysMsg {
  color: #ffffff;
  margin-top: 4%;
  border-radius: 4px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  border: 1px solid #565853;
  position: absolute;
  background-color: #565853;
  width: 19%;
  height: auto;
  z-index: 999;
  padding: 0 1%;
  opacity: 0.8;
  text-align: center;
}
.searchStartButton {
  padding-left: 20%;
  padding-top: 2px;
}
.closebold:hover {
  cursor: pointer;
}
.event-content {
  width: 100%;
  height: 35px;
  display: flex;
}
.event-mes-block-node {
  width: 80%;
  height: 35px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.event-detail {
  width: 20%;
  height: 35px;
}
.event-detail-buttom {
  font-size: 18px;
}
.event-detail-buttom:hover {
  cursor: pointer;
}

el-collapse {
      background-color: #565853 !important;
}
.el-collapse,.el-collapse-item__wrap {
      border: none;
      background-color:  #565853 !important;
    }
 .el-collapse-item__header {
        color: #fff;
        background-color: #565853 !important;
            border-bottom: 1px solid #393a37;
    }
 .el-collapse-item__content {
        color: #fff;
        background-color: #565853 !important;
        border-bottom: 1px solid #393a37;
    }
</style>>