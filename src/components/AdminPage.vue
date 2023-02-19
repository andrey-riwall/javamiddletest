<template>
  <section class="admin">
    <div class="container">
      <table class="fl-table">
        <thead>
          <tr>
            <th>Вид запроса</th>
            <th>Наименование запроса</th>
            <th>Дата создания запроса</th>
            <th>Ответственный за запрос</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in requests" :key="requests.indexOf(item)">
            <td>
              {{ item.name }}
            </td>
            <td>
              {{ item.type.type }}
            </td>
            <td>
              {{ item.date }}
            </td>
            <td>
              {{ item.userDTO }}
              <!-- {{ item.userDTO.name }} -->
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</template>

<style scoped lang='scss'>
.admin {
  position: relative;
  min-height: 100vh;
  background: #557085;
}

.container {
  max-width: 1700px;
  padding: 0 15px;
  padding-top: 100px;
  margin: 0 auto;
}

.fl-table {
  border-radius: 5px;
  font-size: 12px;
  font-weight: normal;
  border: none;
  border-collapse: collapse;
  width: 100%;
  max-width: 100%;
  white-space: nowrap;
  background-color: white;
}

.fl-table td,
.fl-table th {
  text-align: center;
  padding: 8px;
}

.fl-table td {
  border-right: 1px solid #f8f8f8;
  font-size: 12px;
}

.fl-table thead th {
  color: #ffffff;
  background: #4FC3A1;
}


.fl-table thead th:nth-child(odd) {
  color: #ffffff;
  background: #324960;
}

.fl-table tr:nth-child(even) {
  background: #F8F8F8;
}
</style>

<script>
import axios from 'axios';

export default {
  data: function () {
    return {
      requests: [],
    }
  },
  methods: {
    getAccess: function () {
      axios.get('http://localhost:8081/api/auth/access', {})
        .then(response => {
          console.log('access is allowed');
        })
        .catch(error => {
          console.log('access denied');
          this.$router.push({ name: 'main' });
        })
    },
    getRequests: function () {
      axios.get('http://localhost:8081/api/idm/get-all-wait-success', {})
        .then(response => {
          this.requests = response.data.InterdepartmentalRequestDTO;
        })
        .catch(error => {
          console.log(error, 'getRequests');
        })
    }
  },
  computed: {

  },
  created: function () {
    // this.getAccess();
    this.getRequests();
  }
}
</script>
