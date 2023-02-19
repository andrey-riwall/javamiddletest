<template>
  <section class="ir">
    <router-link :to="{ name: 'main' }" class="back">
      Back
    </router-link>
    <div class="container">
      <h2 class="heading">
        Ваши межведомственные запросы
      </h2>
      <ul class="list">
        <li class="item" v-for="item in cards">
          <h3 class="item__heading">
            Организация
          </h3>
          <table class="fl-table">
            <thead>
              <tr>
                <th>Название организации</th>
                <th>ИНН</th>
                <th>Подразделение</th>
                <th>КПП</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>
                  {{ item.organizationDTO.name }}
                </td>
                <td>
                  {{ item.organizationDTO.inn }}
                </td>
                <td>
                  {{ item.organizationDTO.subdivision }}
                </td>
                <td>
                  {{ item.organizationDTO.kpp }}
                </td>
              </tr>
            </tbody>
          </table>
          <h3 class="item__heading">
            Межведомственные запросы
          </h3>
          <table class="fl-table">
            <thead>
              <tr>
                <th>Вид запроса</th>
                <th>Наименование запроса</th>
                <th>Дата создания запроса</th>
                <th>Ответственный за запрос</th>
                <th>Статус</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="el in item.interdepartmentalRequestDTO">
                <td>
                  {{ el.type.type }}
                </td>
                <td>
                  {{ el.name }}
                </td>
                <td>
                  {{ el.date }}
                </td>
                <td>
                  {{ el.user.name }}
                </td>
                <td>
                  {{ el.status ? 'Подтвержден' : 'Не подтвержден' }}
                </td>
              </tr>
            </tbody>
          </table>
        </li>
      </ul>
    </div>
  </section>
</template>

<style lang='scss'>
  .ir {
    position: relative;
    min-height: 100vh;
    background: #557085;
    padding-bottom: 50px;
  }

  .back {
    position: absolute;
    top: 30px;
    left: 30px;
    color: white;
    font-size: 25px;
  }

  .container {
    max-width: 1700px;
    padding: 0 15px;
    margin: 0 auto;
    padding-top: 100px;
  }

  .heading {
    color: white;
    margin-bottom: 30px;
  }

  .list {
    display: flex;
    flex-direction: column;
    gap: 40px;
  }

  .item {
    background: #fff;
    border-radius: 10px;
  }

  .item__heading {
    margin-top: 10px;
    margin-left: 20px;
    margin-bottom: 10px;
    font-size: 20px;
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
    background: #4fc3a1;
  }

  .fl-table thead th:nth-child(odd) {
    color: #ffffff;
    background: #324960;
  }

  .fl-table tr:nth-child(even) {
    background: #f8f8f8;
  }
</style>

<script>
  import axios from 'axios';

  export default {
    data: function() {
      return {
        cards: [],
      }
    },
    methods: {
      getCards: function() {
        axios
          .get('http://localhost:8081/api/idm/get-user-responsible',{})
          .then(response => {
            this.cards = response.data;
          })
          .catch(error => {
            console.log(error, 'getCards');
          })
      },
    },
    created: function() {
      this.getCards();
    }
  }
</script>
