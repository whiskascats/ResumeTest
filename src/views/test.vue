<template>
  <div class="container pt-5">
    <div class="col-12 d-flex justify-content-between pb-5">
      <button type="button" class="btn btn-primary" @click="addData">Add</button>
      <button type="button" class="btn btn-success" @click="updateData">Save</button>
      <button type="button" class="btn btn-danger" @click="getData">Update</button>
    </div>

    <table width="100%">
      <tbody>
        <tr class="table-rows">
          <th>Name</th>
          <th>Birthday</th>
          <th>Salary</th>
          <th>Address</th>
        </tr>
        <template v-if="filerList.length>0">
          <tr class="table-rows" v-for="item in filerList" :key="item">
            <td>
              <input type="text" v-model="item.Name" placeholder="請填入姓名">
            </td>
            <td>
              <input type="date" v-model="item.DateOfBirth">
            </td>
            <td>
              <input type="range" name="" v-model="item.Salary" min="0" max="99999">
            </td>
            <td>
              <input type="text" v-model="item.Address" placeholder="請填入地址">
            </td>
          </tr>
        </template>
      </tbody>
    </table>
  </div>
</template>
<script setup>
import { computed, onMounted, ref } from 'vue';
import axios from 'axios';
import _ from 'lodash';
const list = ref([])
onMounted(async() => {
  await getData()
})

const getData = () => {
  axios.get('http://nexifytw.mynetgear.com:45000/api/Record/GetRecords')
  .then(res => {
    if(res.data.Success) {
      list.value = res.data.Data;
    } else {
      console.log('API讀取錯誤');
    }
  })
}
const addData = () => {
  if(checkData()) {
    list.value.splice(0,0, {
      Name: '',
      DateOfBirth: '',
      Salary: 0,
      Address: ''
    })
  }
}
const updateData = () => {
  if(checkData()) {
    axios({
      url: 'http://nexifytw.mynetgear.com:45000/api/Record/SaveRecords',
      method: 'POST',
      data: list.value
    })
    .then(res => {
      if(res.data.Success) {
        alert('更新成功');
        getData();
      } else {
        alert('更新失敗');
      }
    })
  }

}
const checkData = () => {
  let passed = true;
  document.querySelectorAll('input').forEach(item => {
    if(item.value) item.classList.remove('is-valid')
    else {
      passed = false
      item.classList.add('is-valid')
    }
  })

  return passed
}
const filerList = computed(() => {
  let newList = list.value
  newList.forEach(item => {
    if(item.DateOfBirth.includes('T')) item.DateOfBirth = item.DateOfBirth.split('T')[0];
    
  })
  return newList
})
</script>