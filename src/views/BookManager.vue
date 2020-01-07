<template>
  <div class="book-manager">
    <el-form :inline="true" :model="book" class="demo-form-inline">
      <el-form-item label="图书名称">
        <el-input v-model="book.name" placeholder="请输入图书名称"></el-input>
      </el-form-item>
      <el-form-item label="图书价格">
        <el-input v-model.number="book.price" placeholder="请输入图书价格"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">添加</el-button>
      </el-form-item>
    </el-form>
    <el-table :data="books" border style="width: 60%;margin: 0 auto;">
      <el-table-column prop="id" label="id" width="180"></el-table-column>
      <el-table-column prop="name" label="名字" width="300"></el-table-column>
      <el-table-column prop="price" label="价格"></el-table-column>
      <el-table-column fixed="right" label="操作" width="100">
        <template slot-scope="book">
          <el-button @click="handleDelete(book)" type="text" size="small">删除</el-button>
          <el-button @click="handleEdit(book)" type="text" size="small">修改</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog title="修改信息" :visible.sync="dialogVisible" width="40%">
      <el-form :model="curbook">
        <el-form-item label="图书名称" label-width="80px">
          <el-input v-model="curbook.name"></el-input>
        </el-form-item>
        <el-form-item label="图书价格" label-width="80px">
          <el-input v-model="curbook.price"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="confirmEdit">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialogVisible: false,
      book: { name: "", price: "" },
      curbook: {},
      books: [
        { id: 1, name: "java", price: 60 },
        { id: 2, name: "javascript", price: 66 },
        { id: 3, name: "python", price: 55 }
      ]
    };
  },
  methods: {
    onSubmit() {
      if (!this.book.name||!this.book.price) return;
      let len = this.books.length;
      let maxId = len > 0? this.books[len-1].id : 0;
      this.book.id = ++maxId;
      let book = this._.cloneDeep(this.book);
      this.books.push(book);
    },
    handleDelete(book) {
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
          this.deleteBook(book)
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
    },
    deleteBook(book) {
      let index = this.books.findIndex(item => item.id===book.row.id)
      this.books.splice(index, 1)
    },
    handleEdit(book) {
      this.curbook = this._.cloneDeep(book.row);
      this.dialogVisible = true;
    },
    confirmEdit() {
      let index = this.books.findIndex(item => item.id === this.curbook.id)
      this.books.splice(index, 1, this.curbook)
      this.dialogVisible = false
    }
  }
};
</script>

<style>
</style>