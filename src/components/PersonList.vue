<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';
//exportしたPersonをimportする
import { Person } from './Persons.vue';

type Props = {
  //親から持ってきて、配列形式で紐付ける
  persons: Person[];
};

defineProps<Props>();
const emit = defineEmits(['delete']);

//消す時の処理
const onClickDelete = (id: number, name: string) => {
  //削除に条件をつける confirm関数（JSの機能）確認アラートを出す
  if (confirm(name + ' ' + 'を消しますか?')) {
    emit('delete', id);
  }
};
</script>

<template>
  <li v-for="person in persons" :key="person.id" class="person-list">
    <span>{{ person.name }}</span>
    <span>{{ person.age }}&nbsp歳</span>
    <span>種類&nbsp:&nbsp{{ person.species }}</span>
    <!-- クリックされたら、配列で回している1つのpersonのidを渡す -->
    <button @click="onClickDelete(person.id, person.name)" class="delete-button">
      <span>削除</span>
    </button>
  </li>
</template>

<style scoped>
.person-list {
  display: flex;
  justify-content: space-between;
  width: 320px;
  margin-bottom: 12px;
  padding: 8px 10px;
  background: #01a58d;
  color: #fff;
  border-radius: 5px;
  font-size: 0.875rem;
  font-weight: bold;
  list-style-type: none;
}

span {
  min-width: 40px;
  text-align: left;
}
.delete-button {
  width: 60px;
  height: 24px;
  border: none;
  border-radius: 3px;
  background: #f3f2f0;
  color: #594a46;
  font-size: 0.75rem;
  font-weight: bold;
  transition: 0.2s;
}
.delete-button:hover {
  background: #d2d1cf;
}
</style>
