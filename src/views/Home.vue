<template>
  <div>
    <el-container class="container">
      <el-header>
        <div>
          <span>面试题前端展示</span>
        </div>
      </el-header>
    </el-container>
    <el-card class="box-card">
      <el-row :gutter="20">
        <el-col :span="12">
          <el-input placeholder="请输入学号" v-model="stid">
            <el-button @click="searchStudent" slot="append" icon="el-icon-search"></el-button>
          </el-input>
        </el-col>
        <el-col :span="6">
          <el-button type="primary" @click="addDialog = true">添加学生信息</el-button>
        </el-col>
      </el-row>

      <!-- 表格区 -->
      <el-table border :data="studnetList" stripe>
        <el-table-column label="#" type="index"></el-table-column>
        <el-table-column label="学号" prop="stid"></el-table-column>
        <el-table-column label="姓名" prop="name"></el-table-column>
        <el-table-column label="年级" prop="grad"></el-table-column>
        <el-table-column label="班级" prop="class"></el-table-column>
        <!-- 自定义作用域插槽 -->
        <el-table-column label="操作" width="175px">
          <template slot-scope="scope">
            <el-tooltip effect="dark" content="添加成绩" placement="top" :enterable="false">
              <el-button
                @click="addScore(scope.row.stid)"
                type="primary"
                icon="el-icon-circle-plus"
                size="mini"
              ></el-button>
            </el-tooltip>
            <el-tooltip effect="dark" content="修改学生信息" placement="top" :enterable="false">
              <el-button
                @click="EditStudent(scope.row.stid)"
                type="primary"
                icon="el-icon-edit"
                size="mini"
              ></el-button>
            </el-tooltip>
            <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>

    <!-- ——————————————添加学生信息弹框———————— -->
    <el-dialog title="添加学生信息" :visible.sync="addDialog" width="50%" @close="addDialogclose">
      <!-- 内容信息 -->
      <el-form :model="addForm" :rules="addFormRules" ref="addFormRef" label-width="70px">
        <el-form-item label="学号" prop="stid">
          <el-input v-model="addForm.stid"></el-input>
        </el-form-item>
        <el-form-item label="姓名" prop="name">
          <el-input v-model="addForm.name" type="name"></el-input>
        </el-form-item>
        <el-form-item label="年级" prop="grad">
          <el-input v-model="addForm.grad"></el-input>
        </el-form-item>
        <el-form-item label="班级" prop="class">
          <el-input v-model="addForm.class"></el-input>
        </el-form-item>
      </el-form>
      <!-- 底部按钮 -->
      <span slot="footer" class="dialog-footer">
        <el-button @click="addDialog = false">取 消</el-button>
        <el-button type="primary" @click="addStudent">确 定</el-button>
      </span>
    </el-dialog>

    <!-- ————————————添加成绩弹框——————————— -->
    <el-dialog
      title="添加学生成绩"
      :visible.sync="addScoreDialog"
      width="50%"
      @close="addScoreDialogclose"
    >
      <!-- 内容信息 -->
      <el-form :model="scoreForm" ref="scoreFormRef" label-width="70px">
        <el-form-item label="课程编号" prop="scid">
          <el-input v-model="scoreForm.scid"></el-input>
        </el-form-item>
        <el-form-item label="课程名" prop="coursename">
          <el-input v-model="scoreForm.coursename"></el-input>
        </el-form-item>
        <el-form-item label="分数" prop="score">
          <el-input v-model="scoreForm.score"></el-input>
        </el-form-item>
      </el-form>
      <!-- 底部按钮 -->
      <span slot="footer" class="dialog-footer">
        <el-button @click="addDialog = false">取 消</el-button>
        <el-button type="primary" @click="addScoreData">确 定</el-button>
      </span>
    </el-dialog>

    <!-- ————————————查询弹框——————————— -->
    <el-dialog title="查询结果" :visible.sync="searchDialog" width="50%">
      <!-- 内容信息 -->
      <el-form :inline="true" :model="searchData" class="demo-form-inline">
        <el-form-item label="姓名:">
          <span>{{searchData.name}}</span>
        </el-form-item>
        <el-form-item label="学号:">
          <span>{{searchData.stid}}</span>
        </el-form-item>
      </el-form>
      <el-form :inline="true" :model="searchData" class="demo-form-inline">
        <el-form-item label="班级:">
          <span>{{searchData.grad}}</span>
        </el-form-item>
        <el-form-item label="年级:">
          <span>{{searchData.class}}</span>
        </el-form-item>
      </el-form>
      <el-form :inline="true" :model="searchData" class="demo-form-inline">
        <ul>
          <li v-for="(s,i) in searchData.score" :key="i">
             <span>{{s.coursename}}: {{s.score}}</span>
          </li>
        </ul>
      </el-form>

    </el-dialog>
  </div>
</template>




<script>
export default {
  data() {
    return {
      searchData: {
        stid: "",
        name: "",
        grad: "",
        class: "",
        score: []
      },
      stid: "",
      studnetList: [
        { stid: "0211", name: "张三", grad: "五年级", class: "三班" },
        { stid: "0311", name: "李四", grad: "五年级", class: "一班" }
      ],
      scoreList: [
        {
          scid: "001",
          coursename: "语文",
          stid: "0211",
          datetime: "",
          score: 80
        },
        {
          scid: "002",
          coursename: "数学",
          stid: "0211",
          datetime: "",
          score: 80
        },
        {
          scid: "003",
          coursename: "外语",
          stid: "0211",
          datetime: "",
          score: 80
        }
      ],
      addForm: {
        stid: "",
        name: "",
        grad: "",
        class: ""
      },
      scoreForm: {
        scid: "",
        coursename: "",
        score: ""
      },
      searchDialog: false,
      addScoreDialog: false,
      addDialog: false,
      addFormRules: {
        stid: [
          { required: true, message: "请输入用学号", trigger: "blur" },
          { min: 4, max: 4, message: "学号长度固定4位", trigger: "blur" }
        ],
        name: [{ required: true, message: "请输入姓名", trigger: "blur" }],
        grad: [{ required: true, message: "请输入班级", trigger: "blur" }],
        class: [{ required: true, message: "请输入年级", trigger: "blur" }]
      }
    };
  },
  created() {
    //解决修改数据后，页面数据没有刷新
    this.studnetList = JSON.parse(sessionStorage.getItem("studnetList"));
    sessionStorage.setItem("studnetList", JSON.stringify(this.studnetList));

    this.scoreList = JSON.parse(sessionStorage.getItem("ScoreList"));
    sessionStorage.setItem("ScoreList", JSON.stringify(this.scoreList));
  },
  methods: {
    EditStudent(stid) {
      console.log(stid)
      var obj = JSON.parse(sessionStorage.getItem("studnetList"));
      for (var i = 0; i < obj.length; i += 1) {
        console.log(obj[i].name);
        console.log(obj[i]);
      this.$message.error('该功能还未实现！')
      }
    },
    searchStudent() {
      // console.log(this.stid);
      this.searchData = {
        stid: "",
        name: "",
        grad: "",
        class: "",
        score: []
      }
      this.searchDialog = true;
      var stobj = JSON.parse(sessionStorage.getItem("studnetList"));
      var scobj = JSON.parse(sessionStorage.getItem("ScoreList"));
      // console.log(stobj)
      // console.log(scobj)
      for (var j = 0; j < scobj.length; j++) {
        if ((scobj[j].stid == this.stid)) {
          var sdic = {};
          sdic["scid"] = scobj[j].scid;
          sdic["coursename"] = scobj[j].coursename;
          sdic["datetime"] = scobj[j].datetime;
          sdic["score"] = scobj[j].score;
          this.searchData.score.push(sdic);
          // console.log(sdic);
        }
      }
      for (var i = 0; i < stobj.length; i++) {
        if ((stobj[i].stid == this.stid)) {
          this.searchData.stid = stobj[i].stid;
          this.searchData.name = stobj[i].name;
          this.searchData.grad = stobj[i].grad;
          this.searchData.class = stobj[i].class;
        }
      }
    console.log(this.searchData)
    },

    addScore(stid) {
      this.addScoreDialog = true;
      this.stid = stid;
    },
    addScoreData() {
      console.log(this.stid);
      var scoreobj = JSON.parse(sessionStorage.getItem("ScoreList"));
      if (scoreobj == null) {
        scoreobj = [];
      }
      var sdic = {};
      console.log(scoreobj);
      console.log(this.scoreForm);
      sdic["scid"] = this.scoreForm.scid;
      sdic["coursename"] = this.scoreForm.coursename;
      sdic["stid"] = this.stid;
      sdic["datetime"] = Date();
      sdic["score"] = this.scoreForm.score;
      scoreobj.push(sdic);
      this.scoreList = scoreobj;
      sessionStorage.setItem("ScoreList", JSON.stringify(scoreobj));
      this.addScoreDialog = false;
      this.$message.success("添加学生成绩成功！");
    },
    addStudent() {
      var obj = JSON.parse(sessionStorage.getItem("studnetList"));
      if (obj == null) {
        obj = [];
      }
      for (var i=0;i<obj.length;i++){
        if (obj[i].stid == this.addForm.stid){
          return this.$message.error('该学号已经存在！')
        }
      }
      var dic = {};
      dic["stid"] = this.addForm.stid;
      dic["name"] = this.addForm.name;
      dic["grad"] = this.addForm.grad;
      dic["class"] = this.addForm.class;
      obj.push(dic);
      sessionStorage.setItem("studnetList", JSON.stringify(obj));
      this.studnetList = obj;
      this.addDialog = false;
      this.$message.success("添加学生信息成功！");
    },
    // 重置表单
    addDialogclose() {
      this.$refs.addFormRef.resetFields();
    },
    addScoreDialogclose() {}
  }
};
</script>

<style>
.el-card {
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15) !important;
  width: 70%;
  margin-left: 40px;
  margin-top: 15px;
}
.container {
  height: 100%;
}

.el-header {
  background: #423e3e;
  padding-left: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #fff;
  font-size: 20px;
}
.el-table {
  margin-top: 15px;
  font-size: 13px;
  line-height: 24px;
  text-align: center;
  cursor: pointer;
}
.el-table-column {
  border: 10px;
}
</style>