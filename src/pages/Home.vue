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
      Test: {{ test }}
      <div v-if="test === 123">
        test === 123
      </div>
      <br><br>
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
      this.getUsersCount();
    },
    serverPrefetch () {
      return this.getUsersCount()
    },
    data: function() {
      return {
        test: 123,
        items: [
          { message: 'Message 1' },
          { message: 'Message 2' }
        ],
      };
    },
    methods: {
      add() {
        this.test = this.test === 123 ? 12345 : 123;
        this.$store.commit(COUNTER_MUTATIONS.INCREMENT, 1);
        this.items.push({ message: 'Message 3' });
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
    },
  }
</script>
