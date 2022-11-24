<template>
  <div class="app-container">
    <!-- <el-form :inline="true">
      <el-form-item label="缺陷类型名称">
        <el-input v-model="queryParams.typeName" placeholder="请输入缺陷类型名称" clearable size="small"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" icon="el-icon-search" size="mini" @click="handleQuery">搜索</el-button>
        <el-button type="primary" icon="el-icon-plus" size="mini" @click="handleAdd" v-hasPermi="['basic:defectType:add']">新增</el-button>
      </el-form-item>
    </el-form> -->
    <el-form :inline="true">
      <el-form-item label="设备名称">
        <el-input v-model="queryParams.name" placeholder="请输入设备名称" clearable size="small"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" icon="el-icon-search" size="mini">搜索</el-button>
      </el-form-item>
      <el-row>
        <el-button type="primary" plain icon="el-icon-plus" size="mini">新增</el-button>
        <el-button type="success" plain icon="el-icon-edit" size="mini">修改</el-button>
        <el-button type="danger"  plain icon="el-icon-delete" size="mini">删除</el-button>
      </el-row>
    </el-form>

    <el-table :data="dataList" style="margin-top: 20px;" :cell-style="cellStyle">
      <el-table-column type="selection" width="55" align="center" />
      <el-table-column prop="deviceName" label="设备名称" ></el-table-column>
      <el-table-column prop="ip" label="设备IP"></el-table-column>
      <el-table-column prop="port" label="设备所在端口"></el-table-column>
      <el-table-column prop="status" label="设备状态" :formatter="statusFormat"></el-table-column>
      <el-table-column prop="deviceType" label="设备类型"></el-table-column>
      <el-table-column label="操作" align="center" class-name="small-padding fixed-width">
        <template>
          <el-button size="mini" type="text" @click="handleOperate" icon="el-icon-connection">操作</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 添加或修改菜单对话框 -->
    <!-- <el-dialog :title="title" :visible.sync="open" width="8x00px" :close-on-click-modal="false" append-to-body >
      <el-form ref="form" :model="form" label-width="120px" label-position="left">
        <el-row>
          <el-col :span="24">
            <el-form-item label="设备名称" prop="deviceName">
              <el-input v-model="form.deviceName" placeholder="请输入设备名称" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="24">
            <el-form-item label="设备IP" prop="ip">
              <el-input v-model="form.ip" placeholder="请输入设备IP" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="24">
            <el-form-item label="设备所在端口" prop="port">
              <el-input v-model="form.port" placeholder="请输入设备所在端口" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="12">
          <el-col :span="12">
            <el-form-item label="设备状态" prop="status">
              <el-input v-model="form.status" placeholder="请输入设备状态" />
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="设备类型" prop="deviceType">
              <el-input v-model="form.deviceType" placeholder="请输入设备类型" />
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary">确 定</el-button>
        <el-button>取 消</el-button>
      </div>
    </el-dialog> -->

    <!-- 添加或修改菜单对话框 -->
    <el-dialog :title="title" :visible.sync="operateOpen" width="8x00px" append-to-body >
      <el-row :gutter="20">
        <el-col :span="12">
          <el-card style="border-radius: 12px;" shadow="always">
            剩余电量：<span style="font-weight: bold; color: green">100%</span>
          </el-card>
        </el-col>
        <el-col :span="12">
          <el-card style="border-radius: 12px;" shadow="always">
            燃料余量：<span style="font-weight: bold; color: red">20%</span>
          </el-card>
        </el-col>
      </el-row>

      <el-row style="margin-top: 10px;" :gutter="20">
        <!-- <el-col :span="12">
          <el-card style="border-radius: 12px;" shadow="always">
            转向：
            <el-radio-group v-model="radio">
              <el-radio :label="3">正向</el-radio>
              <el-radio :label="6">反向</el-radio>
            </el-radio-group>
          </el-card>
        </el-col> -->
        <el-col :span="12">
          <el-card style="border-radius: 12px;" shadow="always">
            开关：
            <el-switch
              v-model="value1">
            </el-switch>
          </el-card>
        </el-col>
      </el-row>
      <!-- <el-row style="margin-top: 10px;" :gutter="20">
        <el-col :span="24">
          <el-card style="border-radius: 12px;" shadow="always">
            <span style="font-size: 14px; line-height: 44px;">变速:</span>
            <el-slider style="width: 90%; float: right; margin-right: 20px" v-model="value2"></el-slider>
          </el-card>
        </el-col>
      </el-row> -->
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dataList: [
        {
          deviceName: '锂电钻',
          ip: '114.14.56.3',
          port: 8001,
          status: 0,
          deviceType: '电钻'
        },
        {
          deviceName: '除胶枪',
          ip: '114.14.56.4',
          port: 8011,
          status: 1,
          deviceType: '除胶'
        },
        {
          deviceName: '喷漆枪',
          ip: '114.14.56.1',
          port: 8002,
          status: 1,
          deviceType: '喷漆'
        },
        {
          deviceName: '除锈喷枪',
          ip: '114.14.56.8',
          port: 8004,
          status: 1,
          deviceType: '除锈'
        },
        {
          deviceName: '喷火枪',
          ip: '114.14.56.9',
          port: 8006,
          status: 1,
          deviceType: '喷火'
        }
      ],
      queryParams: {
        name: undefined
      },
      radio: 3,
      value1: false,
      value2: 0,
      title: '喷火枪',
      open: false,
      operateOpen: false,
      form: {}
    };
  },
  created() {
  },
  methods: {
    statusFormat(row, column) {
      return row.status == 0 ? '离线' : '在线'
    },
    handleOperate () {
      this.operateOpen = true
    },
    cellStyle ({ row, column, rowIndex, columnIndex }) {
      if (row.status === 1 && columnIndex  === 4) {
        return 'color: green; font-weight: bold'
      } else if (row.status === 0 && columnIndex === 4) {
        return 'color: red; font-weight: bold'
      }
    },
  }
};
</script>
