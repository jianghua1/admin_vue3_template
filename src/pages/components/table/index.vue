<template>
  <div>
    <el-tabs v-model="activeName" class="demo-tabs">
      <el-tab-pane label="基础示例" name="1">
        <VTable :columns="columns" :data="tableData" :pagination="pagination" @page-current-change="handlePageChange"
          @page-next-click="handlePageChange2" @page-prev-click="handlePageChange3"
          @page-size-change="handlePageChange4"></VTable>
      </el-tab-pane>
      <el-tab-pane label="带斑马纹表格" name="2">
        <VTable :columns="columns" :data="tableData" stripe></VTable>
      </el-tab-pane>
      <el-tab-pane label="带边框表格" name="3">
        <VTable :columns="columns" :data="tableData" border></VTable>
      </el-tab-pane>
      <!-- <p>带状态表格</p> -->
      <!-- <VTable :columns="columns" :data="tableData" :row-class-name="tableRowClassName"></VTable> -->
      <el-tab-pane label="固定表头" name="4">
        <VTable :columns="columns" :data="tableData" :height="500"></VTable>
      </el-tab-pane>
      <el-tab-pane label="固定列" name="5">
        <VTable :columns="fixedTableColumns" :data="fixedTableData">
          <!-- <el-table-column fixed="right" label="Operations" width="120">
        <template #default="{ row, column, $index }">
        <template #default="scope">
          <el-button link type="primary" size="small" @click="handleClick(scope)">Detail</el-button>
          <el-button link type="primary" size="small">Edit</el-button>
        </template>
</el-table-column> -->
        </VTable>
      </el-tab-pane>

      <el-tab-pane label="固定列和表头" name="6">
        <VTable :columns="fixedTableColumns" :data="fixedTableData" :height="250"></VTable>
      </el-tab-pane>

      <el-tab-pane label="流体高度" name="7">
        <VTable :columns="flowTableColumns" :data="flowTableData" :max-height="400">
          <el-table-column fixed="right" label="Operations" min-width="120">
            <template #default="scope">
              <el-button link type="primary" size="small" @click.prevent="deleteRow(scope.$index)">
                Remove
              </el-button>
            </template>
          </el-table-column>
        </VTable>
        <el-button class="mt-4" style="width: 100%" @click="onAddItem">
          Add Item
        </el-button>
      </el-tab-pane>

      <el-tab-pane label="多级表头" name="8">
        <VTable :columns="multiLevelTableColumns" :data="fixedTableData"></VTable>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script setup lang='tsx'>
import type { PaginationType, TableColumnType } from '@/components/Table/types';
import dayjs from 'dayjs'

definePage({
  meta: {
    title: '基础表格',
    icon: 'mdi:message',
    order: 100
  }
})

const activeName = ref('1')

// @ts-ignore const
const pagination = ref({
  align: 'center',
  small: false,
  background: false,
  layout: 'total, sizes,prev, pager, next, jumper',
  pagerCount: 7,
  pageSizes: [10, 20, 30, 40, 50],
  total: 300
} as PaginationType)

const handleClick = (scope, opt: string) => {
  console.log("opt", opt)
  console.log("scope", scope)
}

const columns = [
  {
    prop: 'date',
    label: '日期'
  },
  {
    prop: 'name',
    label: '姓名'
  },
  {
    prop: 'address',
    label: '地址'
  }
] as TableColumnType[]

const tableData = [
  {
    date: '2016-05-03',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    date: '2016-05-02',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    date: '2016-05-04',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
  }
]

const fixedTableColumns = [
  { label: '日期', prop: 'date' },
  { label: '姓名', prop: 'name' },
  { label: '州', prop: 'state' },
  { label: '城市', prop: 'city' },
  { label: '地址', prop: 'address' },
  { label: '邮政编码', prop: 'zip' },
  { label: '标签', prop: 'tag' },
  {
    label: '操作', prop: 'operation', width: '120', fixed: 'right',
    defaultSlot: (_props) => (
      <>
        <el-button link type="primary" size="small" onClick={(e) => {
          e.stopPropagation();
          handleClick(_props, 'detail')
        }}>
          Detail
        </el-button>
        <el-button link type="primary" size="small" onClick={(e) => {
          e.stopPropagation();
          handleClick(_props, 'edit')
        }}>
          Edit
        </el-button>
      </>
    )
  }
  // ,
  // { label: '操作1', prop: 'operation1', width: '120', fixed: 'right',
  //   headerSlot: (_props) => (
  //     <>
  //       <el-button link type="primary" size="small" onClick={()=>handleClick(_props)}>Detail</el-button>
  //       <el-button link type="primary" size="small" >Edit</el-button>
  //     </>
  //   )
  // }
] as TableColumnType[]

const fixedTableData = [
  {
    date: '2016-05-03',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Home',
  },
  {
    date: '2016-05-02',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Office',
  },
  {
    date: '2016-05-04',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Home',
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Office',
  },
]

//流体高度
const flowTableColumns = [
  { label: '日期', prop: 'date' },
  { label: '姓名', prop: 'name' },
  { label: '州', prop: 'state' },
  { label: '城市', prop: 'city' },
  { label: '地址', prop: 'address' },
  { label: '邮政编码', prop: 'zip' },
  { label: '标签', prop: 'tag' }
] as TableColumnType[]

const flowTableData = ref([
  {
    date: '2016-05-03',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Home',
  },
  {
    date: '2016-05-02',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Office',
  },
  {
    date: '2016-05-04',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Home',
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Office',
  },
])

const now = new Date()
const deleteRow = (index: number) => {
  flowTableData.value.splice(index, 1)
}

const onAddItem = () => {
  now.setDate(now.getDate() + 1)
  flowTableData.value.push({
    date: dayjs(now).format('YYYY-MM-DD'),
    name: 'Tom',
    state: 'California',
    city: 'Los Angeles',
    address: 'No. 189, Grove St, Los Angeles',
    zip: 'CA 90036',
    tag: 'Office'
  })
}

//多级表头
const multiLevelTableColumns =
  [
    {
      prop: "date",
      label: "Date",
      width: 150
    },
    {
      label: "Delivery Info",
      children: [
        {
          prop: "name",
          label: "Name",
          width: 120
        },
        {
          label: "Address Info",
          children: [
            {
              prop: "state",
              label: "State",
              width: 120
            },
            {
              prop: "city",
              label: "City",
              width: 120
            },
            {
              prop: "address",
              label: "Address"
            },
            {
              prop: "zip",
              label: "Zip",
              width: 120
            }
          ]
        }
      ]
    }
  ]

const handlePageChange = () => {
  console.log(1)
}
const handlePageChange2 = () => {
  console.log(2)
}
const handlePageChange3 = () => {
  console.log(3)
}
const handlePageChange4 = () => {
  console.log(4)
}
</script>
<style scoped></style>