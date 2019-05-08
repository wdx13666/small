<template>
  <v-form v-model="valid" ref="myBrandForm">
    <v-text-field v-model="brand.name" label="请输入品牌名称" required />
    <v-text-field v-model="brand.letter" label="请输入品牌首字母" required />
    <v-cascader
      url="/item/category/list"
      multiple
      required
      v-model="brand.categories"
      label="请选择商品分类"/>
    <v-layout row>
      <v-flex xs3>
        <span style="font-size: 16px; color: #444">品牌LOGO：</span>
      </v-flex>
      <v-flex>
        <v-upload
          v-model="brand.image"
          url="/upload"
          :multiple="false"
          :pic-width="250"
          :pic-height="90"
        />
      </v-flex>
    </v-layout>
    <v-layout class="my-4" row>
      <v-spacer/>
      <v-btn @click="submit" color="primary">提交</v-btn>
      <v-btn @click="clear" >重置</v-btn>
    </v-layout>
  </v-form>
</template>

<script>
    export default {
      name: "my-brand-form111",
      data() {
        return {
          valid:false, // 表单校验结果标记
          brand:{
            name:'', // 品牌名称
            letter:'', // 品牌首字母
            image:'',// 品牌logo
            categories:[], // 品牌所属的商品分类数组
          }
        }
      },
      methods:{
        submit(){
          // 提交表单
        },
        clear(){
          // 重置表单
          // 重置表单
          this.$refs.myBrandForm.reset();
          // 需要手动清空商品分类
          this.categories = [];
        }
      }
    }
</script>

<style scoped>

</style>
