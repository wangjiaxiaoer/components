<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <button @click="showAddress">显示地址</button>
    <Address :showChose="isShow" @closeAddress="closeAddress" />
    <TreeTable :data="list" />
  </div>
</template>

<script>
// @ is an alias to /src
import Address from '@/components/Address';
import TreeTable from '@/components/TreeTable';

import list from '@/data/TreeTable.json';
export default {
  name: 'Home',
  components: {
    TreeTable,
    Address,
  },
  data() {
    return {
      isShow: false,
      list: list,
    };
  },
  methods: {
    showAddress() {
      this.isShow = true;
    },
    closeAddress() {
      this.isShow = false;
    },
  },
};
</script>
