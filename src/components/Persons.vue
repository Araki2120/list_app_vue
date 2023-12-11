<script setup lang="ts">
import { ref, Ref } from 'vue';

import PersonPostForm from './PersonPostForm.vue';
import PersonList from './PersonList.vue';

//parsonの型を指定 さらにListで使うためにexport
export type Person = {
  id: number;
  name: string;
  age: number;
  species: string;
};

// Personの配列の型をRefにしてオブジェクトを作る
const persons: Ref<Person[]> = ref([
  { id: 0, name: 'ふく', age: 11, species: '猫' },
  { id: 1, name: 'レオ', age: 3, species: '犬' },
]);

const registerPerson = (person: Person) => {
  // 上の配列personに子でinputされたリアティブな値を入れる
  persons.value.push(person);
};

const deletePerson = (id: number) => {
  //deleteしたもの以外を返すことで、実質的に押したものを消す
  persons.value = persons.value.filter((p) => p.id !== id);
};
</script>

<template>
  <div class="container">
    <h1 class="ttl">動物リスト</h1>
    <!-- @registerというイベントが飛んできたら、親のregisterPerson関数が呼ばれる -->
    <PersonPostForm @register="registerPerson" />
    <div class="list-container">
      <ul>
        <!-- 子に変数personsをbind -->
        <!-- @deleteが飛んできたら、親のdeletePerson関数を実行 -->
        <PersonList :persons="persons" @delete="deletePerson" />
      </ul>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.ttl {
  margin: 40px 0 12px;
  font-size: 1rem;
  line-height: 1.2;
}

.ttl:before {
  content: 'Animal List';
  display: block;
  font-size: 1.75rem;
}

ul {
  margin: 0;
  padding: 0;
}
</style>
