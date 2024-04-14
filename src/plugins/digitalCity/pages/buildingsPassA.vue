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
		</template>
	</pagesShow>


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
				<el-button v-show="sysMesStates" type="success" @click="startTis" size="large">开始</el-button>
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
                    协作式仿真由仿真发起者进行全局仿真参数配置，仿真开始后该配置项内的相关参数将无法被修改。
                  </h2>

                </div>
              </div>
              <div class="mesBox" v-show="activeIndex == 4">
                <div v-show="activeIndex == 4">
                  <h2>
                    配置完相关参数后点击创建，协作式仿真创建者会创建第初始仿真行为，后续有用户想要加入仿真需要对当前仿真进行申请，管理员通过申请即可加入该轮仿真。
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
import { ElMessage, ElMessageBox } from 'element-plus'
import { reactive,ref } from 'vue'
import { Pane } from 'tweakpane'

import { genFileId } from 'element-plus'
import type { UploadInstance, UploadProps, UploadRawFile } from 'element-plus'
//文件上传
const upload = ref<UploadInstance>()

const handleExceed: UploadProps['onExceed'] = (files) => {
  upload.value!.clearFiles()
  const file = files[0] as UploadRawFile
  file.uid = genFileId()
  upload.value!.handleStart(file)
}

const submitUpload = () => {
  upload.value!.submit()
}

//导入后的按钮可视化
let sysMesStates =  ref(false);
//发生改变的情况
const handleChange: UploadProps['onChange'] = (uploadFile, uploadFiles) => {
  sysMesStates.value = true;
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
	speed: 10.0,
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
</style>>