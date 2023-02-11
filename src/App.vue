<template>
  <div
    class="department"
    v-for="(item, index) in staff.department"
    :key="index"
  >
    <el-checkbox
      :model-value="item.value"
      :label="item.name"
      size="large"
      @change="departmentChange(item)"
    />
    <i @click="handleShow(item)" class="button">{{
      item.isShow === true ? "-" : "+"
    }}</i>
    <div class="users" v-show="item.isShow">
      <template class="users" v-for="(i, index) in item.users" :key="index">
        <el-checkbox
          :model-value="i.value"
          :label="i.name"
          size="large"
          @change="usersChange(i, item)"
        />
      </template>
    </div>
  </div>

  <div>选中结果：{{ JSON.parse(JSON.stringify(result)) }}</div>
</template>

<script setup>
import { reactive } from "vue";

const staff = reactive({
  department: [
    {
      id: 1,
      value: false,
      name: "部门 1",
      isShow: true,
      checkIndex: 0,
      users: [
        { id: 1, value: false, name: "小一" },
        { id: 2, value: false, name: "小二" },
        { id: 3, value: false, name: "小三" },
      ],
    },
    {
      id: 2,
      value: false,
      name: "部门 2",
      isShow: true,
      checkIndex: 0,
      users: [
        { id: 4, value: false, name: "小四" },
        { id: 5, value: false, name: "小五" },
        { id: 6, value: false, name: "小六" },
      ],
    },
    {
      id: 3,
      value: false,
      name: "部门 3",
      isShow: true,
      checkIndex: 0,
      users: [
        { id: 7, value: false, name: "小七" },
        { id: 8, value: false, name: "小八" },
        { id: 9, value: false, name: "小九" },
      ],
    },
  ],
});

const handleShow = (item) => {
  item.isShow = !item.isShow;
};

//选中结果
const result = reactive({
  department: [],
  users: [],
});

//全选
const departmentChange = (item) => {
  item.value = !item.value;
  result.department.push(item.id);

  if (item.value === true) {
    item.users.forEach((element) => {
      element.value = true;
    });
  } else {
    item.users.forEach((element) => {
      element.value = false;
    });
  }
};

const usersChange = (i, item) => {
  i.value = !i.value;
  result.users.push(i.id);

  //判断如果user全选 部门默认选中
  if (i.value) {
    item.checkIndex++;
  } else {
    item.checkIndex--;
  }
  if (item.checkIndex === 3) {
    item.value = true;
  } else {
    item.value = false;
  }
};
</script>

<style scoped>
.users {
  padding-left: 20px;
}
.button {
  padding: 10px;
  font-size: large;
  cursor: pointer;
}
</style>
