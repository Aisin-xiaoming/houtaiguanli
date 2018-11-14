<template>
    <div class="table">
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-menu"></i>我的基础表格</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
        	<!-- 添加按钮 -->
        	<el-button type="primary" icon="el-icon-plus" class="red" @click="handleAdd">添加</el-button>
		    <el-table
		      :data="tableData"
		      style="width: 100%">
		      <el-table-column
		        prop="date"
		        label="日期"
		        width="180">
		      </el-table-column>
		      <el-table-column
		        prop="name"
		        label="姓名"
		        width="180">
		      </el-table-column>
		      <el-table-column
		        prop="address"
		        label="地址">
		      </el-table-column>
		      <el-table-column label="操作" width="180" align="center">
                    <template slot-scope="scope">
                        <el-button type="text" icon="el-icon-edit"@click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                        <el-button type="text" icon="el-icon-delete" class="red" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                    </template>
                </el-table-column>
		    </el-table>

        </div>
    	<!-- 编辑弹出 -->
		<el-dialog
			title="编辑"
			:visible.sync="editVisible"
			width="30%"
			>
		     <el-form ref="form" :model="form" label-width="50px">
	            <el-form-item label="日期">
	                <el-date-picker type="date" placeholder="选择日期" v-model="form.date" value-format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
	            </el-form-item>
	            <el-form-item label="姓名">
	                <el-input v-model="form.name"></el-input>
	            </el-form-item>
	            <el-form-item label="地址">
	                <el-input v-model="form.address"></el-input>
	            </el-form-item>

	        </el-form>
			<span slot="footer" class="dialog-footer">
				<el-button @click="editVisible = false">取 消</el-button>
				<el-button  v-if="isEdit" type="primary" @click="saveEdit">bianji确 定</el-button>
				<el-button v-else type="primary" @click="	addHang">确 定</el-button>
			</span>
		</el-dialog>
		<!-- 删除按钮 -->
		<el-dialog title="提示" :visible.sync="delVisible" width="300px" center>
            <div class="del-dialog-cnt">删除不可恢复，是否确定删除？</div>
            <span slot="footer" class="dialog-footer">
                <el-button @click="delVisible = false">取 消</el-button>
                <el-button type="primary" @click="deleteHang">确 定</el-button>
            </span>
        </el-dialog>
        <!-- 添加 -->
    </div>
</template>

<script>
    export default {
    	 methods: {
		      handleClose(done) {
		        this.$confirm('确认关闭？')
		          .then(_ => {
		            done();
		          })
		          .catch(_ => {});
		      },
		      //编辑按钮
		      handleEdit(index, row) {
		      	this.isEdit = true
                this.idx = index;
                let item = this.tableData[index];
                this.form = {
                    name: item.name,
                    date: item.date,
                    address: item.address
                }
                //当编辑弹出为true时弹出
                this.editVisible = true;

            },
            //删除按钮
            handleDelete(index, row) {
                this.idx = index;
                this.delVisible = true;
            },
            //添加按钮
            handleAdd(){
            	this.isEdit = false
            	this.editVisible = true;
            	this.form = {};
            	
            },
            // 保存编辑后的内容确认按牛
            saveEdit(){
            	this.editVisible=false;
            	this.$set(this.tableData,this.idx,this.form);
            	this.$message.success(`修改${this.idx+1}行成功`)

            },
            deleteHang(){
            	this.delVisible = false;
            	this.tableData.splice(this.idx,1);
            	this.$message.success(`删除${this.idx+1}行成功`)
            },
            //确认按钮
            addHang(){
            	this.tableData.push(this.form);
            	this.editVisible= false;
            },
		    },
        data(){
        	return{
        		isEdit:true,
        		form: {
                    name: '',
                    date: '',
                    address: ''
                },
                editVisible: false,
        		delVisible: false,
        		addVisible:false,
        		 tableData: [{
            date: '2016-05-02',
            name: '小萍萍',
            address: '上海市普陀区金沙江路 1518 弄'
          }, {
            date: '2016-05-04',
            name: '小明明',
            address: '上海市普陀区金沙江路 1517 弄'
          }, {
            date: '2016-05-01',
            name: '小萍萍',
            address: '上海市普陀区金沙江路 1519 弄'
          }, {
            date: '2016-05-03',
            name: '小明明',
            address: '上海市普陀区金沙江路 1516 弄'
          }]
        	}
        },
       
        
      
    }

</script>

<style scoped>
    
</style>
