<template>
  <v-card>
    <v-card-title flat color="white">
      <v-btn color="primary" @click="addBrand">新增</v-btn>
      <!--空间隔离组件-->
      <v-spacer />
      <!--搜索框，与search属性关联-->
      <v-text-field label="输入关键字搜索" append-icon="search" v-model="search" hide-details/>
    </v-card-title>

    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center"><img v-if="props.item.image" :src="props.item.image" width="130" height="40"/></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <td class="text-xs-center">
          <v-icon small class="mr-2" @click="editItem(props.item.id)">
            edit
          </v-icon>
          <v-icon small @click="deleteItem(props.item.id)">
            delete
          </v-icon>
        </td>
      </template>
    </v-data-table>

    <!--弹出的对话框-->
    <v-dialog max-width="500" v-model="show" persistent>
      <v-card>
        <!--对话框的标题-->
        <v-toolbar dense dark color="primary">
          <v-toolbar-title>新增品牌</v-toolbar-title>
          <v-spacer/> <!--分隔符-->
          <!--关闭窗口的按钮-->
          <v-btn icon @click="closeWindow"><v-icon>close</v-icon></v-btn>
        </v-toolbar>
        <!--对话框的内容，表单-->
        <v-card-text class="px-5" style="height:400px">
          <my-brand-form></my-brand-form>
        </v-card-text>
      </v-card>
    </v-dialog>

  </v-card>
</template>




<script>

  // 导入自定义的表单组件
  import MyBrandForm from './MyBrandForm'

  export default {
    name: "MyBrand",
    data() {
      return {
        totalBrands: 0, // 总条数
        brands: [], // 当前页品牌数据
        search: "", // 查询关键字
        loading: true, // 是否在加载中
        show: false,//控制对话框的显示
        pagination: {}, // 分页信息
        headers: [ // 头信息
          {text: 'id', align: 'center', value: 'id'},
          {text: '名称', align: 'center', value: 'name', sortable: false},
          {text: 'LOGO', align: 'center', value: 'image', sortable: false},
          {text: '首字母', align: 'center', value: 'letter'},
          {text: '操作', align: 'center', value: 'id', sortable: false}
        ]
      }
    },
    watch: {
      pagination:{
        deep: true, // 深度监视
        handler(){
          this.getDataFromServer();
        }
      },
      search(){
        this.pagination.page = 1;
        this.getDataFromServer();
      }
    },
    methods: {
      getDataFromServer() { // 从服务端加载数据的函数
        this.loading = true; // 加载数据
        // 通过axios获取数据
        this.$http.get("/item/brand/page", {
          params: {
            page: this.pagination.page, // 当前页
            rows: this.pagination.rowsPerPage, // 每页条数
            sortBy: this.pagination.sortBy, // 排序字段
            desc: this.pagination.descending, // 是否降序
            key: this.search // 查询字段
          }
        }).then(resp => { // 获取响应结果对象
          this.totalBrands = resp.data.total; // 总条数
          this.brands = resp.data.items; // 品牌数据
          this.loading = false; // 加载完成
        });
      },
      deleteItem(id) {
        console.log(key)
      },
      editItem(id) {
        console.log(id)
      },
      addBrand() {
        // 控制弹窗可见：
        this.show = true;
      },
      closeWindow(){
        // 关闭窗口
        this.show = false;
      }
    },
    // 渲染后执行
    mounted() {
      this.getDataFromServer() // 调用数据初始化函数
    },
    components: {
      MyBrandForm
    }
  }
</script>

<!-- scoped:当前样式只作用于当前组件的节点 -->
<style scoped>

</style>
