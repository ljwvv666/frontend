<template>

    <el-button type="primary" style="margin-bottom: 20px;" @click="downloadFile">下载模板</el-button>
    <!-- 注意事项 -->
    <el-card style="max-width: 680px; margin-bottom: 20px;">
        <template #header>
        <div class="card-header">
            <span>Excel模板上传的注意事项</span>
        </div>
        </template>
            <span>
                1. 模板中所有标题含“必填”的列都是必须填的，否则会出现解析失败的问题。
            </span>
            <el-divider></el-divider>
            <span>
                2. 模板中的“功能描述”列的各行必填项，否则无法解析
            </span>
    </el-card>

    <el-upload
      class="upload-demo"
      drag
      action="http://your-backend-endpoint/upload"
      :on-success="handleUploadSuccess"
      :on-error="handleUploadError"
      :before-upload="beforeUpload"
      :limit="1"
      :on-exceed="handleExceed"
      accept=".xls,.xlsx"
    >
      <el-icon class="el-icon--upload"><upload-filled /></el-icon>
      <div class="el-upload__text">
        Drop file here or <em>click to upload</em>
      </div>
      <template #tip>
        <div class="el-upload__tip">
          Only Excel files (.xls, .xlsx) with a size less than 500kb
        </div>
      </template>
    </el-upload>
    <el-row :gutter="20">
      <el-col :span="18"><div class="grid-content ep-bg-purple" /></el-col>
      <el-col :span="6"><div class="grid-content ep-bg-purple" />
        <el-button type="primary" @click="reload">重新上传文件</el-button>
        <el-button type="primary" @click="viewFP">开始分析功能点</el-button>
      </el-col>
    </el-row>

</template>
  


<script setup lang="ts">
import { UploadFilled } from '@element-plus/icons-vue'
import { ElMessage } from "element-plus";
import { RouterLink,useRouter } from "vue-router";
import axios from 'axios';

const router = useRouter();
const reload = () => {

}

//同时还要add一个gsc表 传一个sysId
const viewFP = () => {
  router.push("/index/viewFP");
  
}

// 上传成功回调
const handleUploadSuccess = (response: any, file: any) => {
  console.log("Upload Success:", response, file);
  ElMessage.success("文件上传成功");
};

// 上传失败回调
const handleUploadError = (error: any, file: any) => {
  console.error("Upload Error:", error, file);
  ElMessage.error("文件上传失败");
};

// 上传前验证文件
const beforeUpload = (file: File) => {
  const isExcel = file.type === "application/vnd.ms-excel" || file.type === "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet";
  const isLt500kb = file.size / 1024 < 500;

  if (!isExcel) {
    ElMessage.error("只能上传 Excel 文件 (.xls, .xlsx)");
    return false;
  }
  if (!isLt500kb) {
    ElMessage.error("文件大小不能超过 500kb");
    return false;
  }
  return true;
};

// 文件超出限制时的回调
const handleExceed = () => {
  ElMessage.warning("只能上传一个文件，请删除原有文件后再试！");
};

// 下载模板文件
const downloadFile = () => {
  const fileUrl = 'evaluation.xlsx';
  const fileName = 'template-file.xlsx';

  const link = document.createElement('a');
  link.href = fileUrl;
  link.download = fileName;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
}

</script>
  