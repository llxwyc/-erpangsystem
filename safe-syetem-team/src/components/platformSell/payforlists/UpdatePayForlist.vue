<template>
  <div class="addList">
    <h3>修改理赔</h3>
    <el-container>
      <el-container>
        <div class="add">
          <el-form
            label-position="right"
            label-width="90px"
            :model="newPayforlist"
            ref="ruleForm"
            class="demo-ruleForm"
          >
          <el-form-item label="理赔人:" prop="users_id">
              <el-select
                v-model="newPayforlist.users_id"
                placeholder="请选择理赔人"
                  @change="change_user"
              >
                <el-option
                  v-for="item in allUsers"
                  :value="item._id"
                  :key="item._id"
                  :label="item.name"
                  
                >
                </el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="保险订单:" prop="safeorder_id">
              <el-select
                v-model="newPayforlist.safeorder_id._id"
                placeholder="请选择保险订单"
              >
                <el-option
                  v-for="item in order"
                  :value="item._id"
                  :key="item._id"
                  :label="item.name"
                   @change="change_user"
                >
                </el-option>
              </el-select>
            </el-form-item>
            
            <el-form-item label="申请日期:"  prop="data">
              <el-date-picker
                type="date"
                placeholder="选择日期"
                v-model="newData"
                style="width: 100%"
                disabled
              ></el-date-picker>
            </el-form-item>
            <el-form-item label="理赔原因" prop="resaon">
              <el-input type="textarea" v-model="newPayforlist.resaon"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button
                type="primary"
                style="width: 200px"
                @click.prevent="updatePayforlist"
                >确认修改</el-button
              >
            </el-form-item>
          </el-form>

          <div>
            <el-upload
              class="avatar-uploader"
              action="http://localhost:3000/images/uploadImages"
              :show-file-list="false"
              :on-success="handleAvatarSuccess"
            >
              <img v-if="newPayforlist.pay_img" :src="newPath + newPayforlist.pay_img" class="avatar" />
              <i v-else class="el-icon-plus avatar-uploader-icon"></i>
            </el-upload>

            <span style="font-size: 14px; color: #606266">上传理赔凭证</span>
          </div>
        </div>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import { createNamespacedHelpers } from "vuex";
const { mapState, mapMutations, mapActions } = createNamespacedHelpers(
  "payforlists"
);
export default {
    mounted() {
        this.changeImagePath("/images/");
        this.getPayforlistById(this.$route.params);
        this.changeImage();
        this.getAllUsers();
    },
  data() {
    return {
      imageUrl: "",
      imageName: "",
    };
  },
  watch: {
    imageName(val) {
        console.log(val);
      this.changeImage(val);
    },
  },
  computed: {
      ...mapState(['payforlist','path','allUsers','order']),
    newData: {
      get() {
        this.changeData();
        // console.log(this.payforlists.data);
        return this.newPayforlist.sq_date;
      },
      set(val) {
        this.changeData(val);
        this.changeSqData(val);
      },
    },
     newPayforlist: {
      get() {
        return this.payforlist;
      },
      set(val) {
        // console.log(val);
        this.getPayforlist(val);
      },
    },
      newPath() {
      return "http://localhost:3000" + this.path;
    },
  
  },
  methods: {
    ...mapMutations(["changeImage",'changeSqData','changeImagePath','setImage']),
    ...mapActions(['updatePayforlist','getPayforlistById','getUserById','getAllUsers']),
    open(msg) {
      this.$alert(msg, "添加理赔订单", {
        confirmButtonText: "确定",
      });
    },
    handleAvatarSuccess(res, file) {
      console.log(res);
      this.imageUrl = "http://localhost:3000/temp/" + res[0].filename;
      //console.log(this.imageUrl);
       this.setImage(res.data);
      this.imageName = res[0].filename;
      this.newPayforlist.pay_img=res[0].filename;
       this.changeImagePath("/temp/");
    },
    //图片大小，格式验证
    beforeAvatarUpload(file) {

    },
    //处理时间
    changeData(nowDate) {
      if (!nowDate) {
        nowDate = new Date();
      }
      var date = {
        year: nowDate.getFullYear(),
        month: nowDate.getMonth() + 1,
        day: nowDate.getDate(),
      };
      this.newPayforlist.sq_date =
        date.year +
        "-" +
        (date.month >= 10 ? date.month : "0" + date.month) +
        "-" +
        (date.day >= 10 ? date.day : "0" + date.day);
      //console.log(this.payforlists.data);
    },
      change_user(){
        this.newPayforlist.safeorder_id='';
      this.getUserById({_id:this.newPayforlist.users_id});
    
    },
  },
};
</script>

<style>
.addList > h3 {
  text-align: left;
  margin: 0px 0px 20px 0px;
  height: 40px;
  line-height: 40px;
  background-color: #ccd7e6;
  padding: 0px 20px;
}
.add {
  width: 900px;
  justify-content: space-evenly;
  margin: 0 auto;
  display: flex;
  text-align: left;
}
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>