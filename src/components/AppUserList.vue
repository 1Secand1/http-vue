<template>
  <ul v-if="users.length !== 0">
    <li class="card row" v-for="{ id, userName } in users" :key="id">
      <h3>{{ userName }}</h3>
      <button class="btn danger" @click="$emit('userRemove', id)">
        Удалить
      </button>
    </li>
  </ul>

  <div v-else class="card center">
    <h4>Список пользователей пуст</h4>
  </div>
</template>

<script>
export default {
  emits: ["userRemove"],
  props: ["users"],
  data() {
    return {};
  },

  mounted() {},
  methods: {},
};
</script>

<style scoped>
.row {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
