<template>
  <div>
    <el-table
      :data="bannerlist"
      style="width: 100%"
      border
    >
      <el-table-column prop="id" label="编号" width="160">
      </el-table-column>
      <el-table-column prop="title" label="轮播图标题" width="160">
      </el-table-column>
      <!-- <el-table-column prop="pid" label="上级菜单"> </el-table-column> -->
      <el-table-column label="图片" width="160">
        <template slot-scope="item">
          <div>
            <img :src="$imgUrl + item.row.img" alt="" />
          </div>
        </template>
      </el-table-column>
      <!-- <el-table-column prop="url" label="菜单地址" width="160">
      </el-table-column> -->
      <el-table-column label="状态">
        <template slot-scope="scope">
          <div>
            <el-tag type="success" v-if="scope.row.status === 1">启用</el-tag>
            <el-tag type="danger" v-else>禁用</el-tag>
          </div>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <div>
            <el-button type="primary" size="small" @click="edit(scope.row.id)">编辑</el-button>
            <el-button type="danger" size="small" @click="del(scope.row.id)"
              >删除</el-button
            >
          </div>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import { getBannerDelete } from "../../util/axios";
export default {
  data() {
    return {};
  },
  mounted() {
    this.getBannerList();
  },
  computed: {
    ...mapGetters({
      bannerlist: "banner/getBannerList",
    }),
  },
  methods: {
    ...mapActions({
      getBannerList: "banner/getBannerListAction",
    }),
    edit(id){
      this.$emit('edit',id);
    },
    del(id) {
      this.$confirm("确定要删除这条数据吗?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          getBannerDelete({ id }).then((res) => {
            // console.log(res);
            if (res.data.code === 200) {
              this.$message.success(res.data.msg);
              this.getBannerList();
            }else{
              this.$message.error(res.data.msg);
            }

          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    },
  },
};
</script>

<style lang="stylus" scoped>
img {
  width: 100px;
}
</style>
