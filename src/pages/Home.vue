<template>
  <div v-bind:style="{ padding: '40px' }">
    <div>Home</div>
    <br />
    <div>
      Simple counter: {{ count }}
      <button @click="add">Increment vuex count</button>
      <br><br>
      Users count: {{ usersCount }}
      <br><br>
      Message: {{ message }}
      <br>
      Reversed message: {{ reversedMessage }}
      <br><br>
      <button @click="addMessage">Add message</button>
      <ul id="example-1">
        <li v-for="item in items" v-bind:key="item.message">
          {{ item.message }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import { mapState } from 'vuex';
  import { COUNTER_MUTATIONS, COUNTER_ACTIONS } from '../store/modules/counter';

  export default {
    name: 'Home',
    metaInfo: {
      title: 'Vue ssr - homepage',
      meta: [{ name: 'description', content: 'Vue ssr template' }],
    },
    mounted() {
      this.messageCounter = 3;
      this.getUsersCount();
    },
    serverPrefetch () {
      return this.getUsersCount()
    },
    data: function() {
      return {
        message: 'Hello',
        items: [
          { message: 'Message 1' },
          { message: 'Message 2' }
        ],
      };
    },
    methods: {
      add() {
        this.$store.commit(COUNTER_MUTATIONS.INCREMENT, 1);
      },
      addMessage() {
        this.items.push({ message: `Message ${this.messageCounter++}` });
      },
      getUsersCount() {
        return this.$store.dispatch(COUNTER_ACTIONS.GET_USERS_COUNT);
      },
    },
    computed: {
      ...mapState({
        count: state => state.counter.count,
        usersCount: state => state.counter.usersCount,
      }),
      reversedMessage: function () {
        return this.message.split('').reverse().join('')
      }
    },
  }
</script>
