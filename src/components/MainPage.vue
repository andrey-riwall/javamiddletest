<template>
  <section class="main">
    <button class="login" @click="logoutF()">
      Log out
    </button>
    <button class="create" @click="createStatus=true">
      Create org
    </button>
    <button v-if="createStatus" class="form-close" @click.prevent="createStatus=false">close</button>
    <form action="#" class="create__form" :class="{ is_active: createStatus }" @submit.prevent="createOrg">
      <div class="form-container">
        <input type="text" class="create__input" placeholder="Название" v-model="formName">
        <input type="text" class="create__input" placeholder="ИНН" v-model="formINN">
        <input type="text" class="create__input" placeholder="Подразделение" v-model="formSubdiv">
        <input type="text" class="create__input" placeholder="КПП" v-model="formKPP">
        <button type="submit" class="create__btn">
          Создать
        </button>
      </div>
    </form>
    <div class="container">
      <h2 class="heading">Home</h2>
      <ul class="list">
        <li class="item" :class="{ is_active: item.status }" v-for="item in cards" :key="cards.indexOf(item)">
          <h3 class="item__heading">
            {{ item.heading }}
          </h3>
          <ul class="tabs">
            <li class="tab" :class="{ is_active: tab.status }" v-for="tab in item.tabs" :key="tab.tab">
              <button class="tab__btn" @click.prevent="openCard(cards.indexOf(item), tab.content, item.tabs.indexOf(tab))">
                {{ tab.tab }}
              </button>
            </li>
          </ul>
          <h4 class="item__heading">
            {{ item.content }}
          </h4>
        </li>
      </ul>
    </div>
  </section>
</template>

<style scoped lang='scss'>
  .main {
    position: relative;
    min-height: 100vh;
    background: #557085;
  }

  .login {
    position: absolute;
    top: 50px;
    right: 30px;
    color: white;
    background: none;
    border: none;
    cursor: pointer;
    font-size: 20px;
  }

  .create {
    position: absolute;
    top: 80px;
    right: 30px;
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

    background: rgba(black, .4);

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
    background: rgba(black, .1);
    font-size: 20px;
  }

  .create__btn {
    background: none;
    border: none;
    cursor: pointer;

    padding: 10px 20px;
    border-radius: 10px;
    background: rgba(black, .1);
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
    height: 92px;
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
      background: rgba(black, .05);
    }
  }

  .tab__btn {
    width: 100%;
    height: 100%;
    padding: 10px;
    background: rgba(black, .1);
    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;

    font-size: 20px;
  }
</style>

<script>
  import axios from 'axios'
  import { mapActions } from 'vuex';

  export default {
    components: {  },
    data: function() {
      return {
        createStatus: false,

        cards: [
          {
            heading: 'МУ',
            tabs: [
              {
                tab: 'МУ1',
                content: 'МУ1',
                status: false,
              },
              {
                tab: 'МУ2',
                content: 'МУ2',
                status: false,
              },
            ],
            content: '',
            status: false,
          }
        ],

        formName: '',
        formINN: '',
        formSubdiv: '',
        formKPP: '',
      }
    },
    methods: {
      getCards: function() {
        axios.get(`http://localhost:8081/api/idm/`, {})
        .then(response => {
          let cards = [];
          let content = response.data.content;
          content.forEach(el => {
            let obj = {
              heading: el.name,
              tabs: [
                {
                  tab: 'МУ1',
                  content: 'МУ1',
                  status: false,
                },
                {
                  tab: 'МУ2',
                  content: 'МУ2',
                  status: false,
                },
              ],
              content: '',
              status: false,
            }
          })
        })
        .catch(error => {
          alert(error);
        })
      },
      createOrg: function() {
        axios.post(`http://localhost:8081/api/org/create`, {
          name: this.formName,
          inn: this.formINN,
          subdivision: this.formSubdiv,
          kpp: this.formKPP,
        })
        .then(response => {
          console.log('succes');
          this.createStatus = false;
        })
        .catch(error => {
          alert(error)
        })
      },
      logoutF: function() {
        this.logout()
        this.$router.push({ name: 'auth' });
      },
      ...mapActions({ logout: 'logout' }),

      openCard: function(cardIndex, content, tabIndex) {
        this.cards[cardIndex].content = content;
        this.cards[cardIndex].tabs.forEach(el => {
          el.status = false;
        })
        this.cards[cardIndex].tabs[tabIndex].status = true;
        if(!this.cards[cardIndex].status) {
          this.cards[cardIndex].status = true;
        }
      }
    },
    created: function() {
      this.getCards();
    },
  }
</script>
