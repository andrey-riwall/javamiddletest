<template>
  <section class="main">
    <router-link :to="{ name: 'main' }" class="to-ir">back</router-link>
    <div class="container">
      <h2 class="heading">Home</h2>
      <ul class="list">
        <li class="item" :class="{ is_active: item.status }" v-for="item in cards" :key="cards.indexOf(item)">
          <h3 class="item__heading">
            {{ item.heading }} до {{ item.time }}
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
                  {{ item.organization.name }}
                </td>
                <td>
                  {{ item.organization.inn }}
                </td>
                <td>
                  {{ item.organization.subdivision }}
                </td>
                <td>
                  {{ item.organization.kpp }}
                </td>
              </tr>
            </tbody>
          </table>
          <ul class="tabs">
            <li class="tab" :class="{ is_active: item.requestEntitiesStatus }">
              <button class="tab__btn" @click.prevent="openRequestEntities(cards.indexOf(item))">
                Межведомственные запросы ({{ item.requestEntities.length }})
              </button>
            </li>
            <li class="tab" :class="{ is_active: item.documentsStatus }">
              <button class="tab__btn" @click.prevent="openDocuments(cards.indexOf(item))">
                Документы
              </button>
            </li>
          </ul>
          <div class="content__table">
            <table class="fl-table">
              <thead>
                <tr>
                  <th>Вид запроса</th>
                  <th>Наименование запроса</th>
                  <th>Дата создания запроса</th>
                  <th>Ответсвенный за запрос</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="el in item.content" :key="item.content.indexOf(el)">
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
                    {{ el.user.name}}
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </li>
      </ul>
    </div>
  </section>
</template>

<style scoped lang="scss">
.main {
  position: relative;
  min-height: 100vh;
  background: #557085;
}

.to-ir {
  position: absolute;
  top: 140px;
  right: 20px;
  color: white;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 20px;
}

.login {
  position: absolute;
  top: 50px;
  right: 20px;
  color: white;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 20px;
}

.createCard {
  position: absolute;
  top: 110px;
  right: 20px;
  color: white;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 20px;
}

.create {
  position: absolute;
  top: 80px;
  right: 20px;
  color: white;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 20px;
}

.create__form {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;

  display: flex;
  align-items: center;
  justify-content: center;

  background: rgba(black, 0.4);

  display: none;

  &.is_active {
    display: flex;
  }
}

.form-close {
  position: absolute;
  right: 700px;
  top: 120px;
  font-size: 30px;
  color: white;
  background: none;
  border: none;
  cursor: pointer;
  z-index: 100;
}

.form-container {
  display: flex;
  flex-direction: column;
  gap: 20px;

  padding: 50px 30px;
  background: #fff;
  border-radius: 10px;
}

.create__input {
  border: none;
  background: none;
  outline: none;

  padding: 10px 20px;
  border-radius: 10px;
  background: rgba(black, 0.1);
  font-size: 20px;
}

.create__btn {
  background: none;
  border: none;
  cursor: pointer;

  padding: 10px 20px;
  border-radius: 10px;
  background: rgba(black, 0.1);
  font-size: 20px;
}

.container {
  max-width: 1680px;
  padding: 0 15px;
  margin: 0 auto;
}

.heading {
  font-weight: 400;
  padding-top: 100px;
  color: white;
  font-size: 32px;
}

.list {
  padding-top: 50px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.item {
  background: #fff;
  border-radius: 10px;
  height: 160px;
  overflow: hidden;

  &.is_active {
    height: auto;
  }
}

.item__heading {
  margin-left: 30px;
  margin-top: 10px;
  margin-bottom: 10px;
}

.tabs {
  display: flex;
  align-items: center;
  gap: 1px;
  width: 100%;
}

.tab {
  display: flex;
  align-items: center;
  justify-content: center;
  flex: 1;

  &.is_active .tab__btn {
    background: rgba(black, 0.05);
  }
}

.tab__btn {
  width: 100%;
  height: 100%;
  padding: 10px;
  background: rgba(black, 0.1);
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;

  font-size: 20px;
}

.content__list {
  padding: 10px;
}

.content__btn {
  margin-top: 10px;
  padding: 10px 20px;
  font-size: 16px;
  background: rgba(black, 0.1);
  width: 100%;
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
import axios from "axios";
import { mapActions } from "vuex";

export default {
  components: {},
  data: function () {
    return {
      createStatus: false,
      createCardStatus: false,
      createIRStatus: false,
      createIRCard: null,

      cards: [],
      cardsData: [],

      formName: "",
      formINN: "",
      formSubdiv: "",
      formKPP: "",

      cardName: "",
      cardTime: "",
      cardOrgs: "",

      IRType: "",
      IRName: "",
      IRUser: "",
      IRMunicipalServ: "",

      orgs: [],
      users: [],
      types: [],
    };
  },
  methods: {
    getCards: function () {
      axios
        .get(`http://localhost:8081/api/idm/accounting`, {})
        .then((response) => {
          let cards = [];
          this.cardsData = response.data.content ? response.data.content : [];
          this.cardsData.forEach((el) => {
            let obj = {
              heading: el.name,
              content: "",
              requestEntities: el.interdepartmentalRequestEntities,
              documents: el.interdepartmentalDocuments,
              status: false,
              documentsStatus: false,
              requestEntitiesStatus: false,
              organization: el.organization,
              time: new Date(el.time),
            };
            cards.push(obj);
          });
          this.cards = cards;
        })
        .catch((error) => {
          console.log(error, "getCards");
        });
    },

    logoutF: function () {
      this.logout();
      this.$router.push({ name: "auth" });
    },
    ...mapActions({ logout: "logout" }),

    openRequestEntities: function (cardIndex) {
      this.cards[cardIndex].documentsStatus = false;
      this.cards[cardIndex].status = true;
      this.cards[cardIndex].content = this.cards[cardIndex].requestEntities;
      this.cards[cardIndex].requestEntitiesStatus = true;
    },
    openDocuments: function (cardIndex) {
      this.cards[cardIndex].requestEntitiesStatus = false;
      this.cards[cardIndex].status = true;
      this.cards[cardIndex].content = this.cards[cardIndex].documents;
      this.cards[cardIndex].documentsStatus = true;
    },
  },
  created: function () {
    this.getCards();
  },
};
</script>
