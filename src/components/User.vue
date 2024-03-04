<template>
    <q-table
      :rows="users"
      :columns="columns"
      row-key="id"
      :pagination.sync="pagination"
      :sorting.sync="sorting"
      class="styled-table"
    >
      <!-- Slot for each column -->
      <template v-for="column in columns" :key="column.name" v-slot:[`body-cell-${column.name}`]="{ row }">
        <q-td>{{ row[column.field] }}</q-td>
      </template>
    </q-table>

</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  users: Object
});

const pagination = ref({ rowsPerPage: 5 });
const sorting = ref({ sortBy: 'id', descending: false },{ sortBy: 'name', descending: false });
const columns = [
  { name: 'id', align: 'left', label: 'ID', field: 'id',sortable:true},
  { name: 'name', align: 'left', label: 'Name', field: 'name' ,sortable:true},
  { name: 'email', align: 'left', label: 'Email', field: 'email' },
  { name: 'website', align: 'left', label: 'Website Name', field: 'website' },
  { name: 'phone', align: 'left', label: 'Phone No', field: 'phone' }
];
</script>
<!--Styling the headers of server table-->
<style>
.styled-table th {
  background-color: #84bedf; /* Set the background color */
  color: #bb1821; /* Set the text color */
  font-weight: bold; /* Make the text bold */
  border-bottom: 1px solid #0f0f0f; /* Add a bottom border */
}
</style>
