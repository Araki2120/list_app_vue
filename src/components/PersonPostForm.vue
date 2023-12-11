<script setup lang="ts">
import { computed, ref, defineEmits } from 'vue';

const inputtingName = ref<string>('');
const inputtingAge = ref<number>(0);
const selectSpecies = ref<string>('');

//input内容を「register」という名前でemit
const emit = defineEmits(['register']);
const register = () => {
  const person = {
    id: Math.random(),
    name: inputtingName.value,
    age: inputtingAge.value,
    species: selectSpecies.value,
  };

  emit('register', person);
  //空欄に戻す
  inputtingName.value = '';
  inputtingAge.value = 0;
  selectSpecies.value = '';
};

//スタイル関係の設定
const nameLengthLimit = 10;

const checkName = computed(() => {
  if (inputtingName.value.length > nameLengthLimit) {
    return false;
  } else {
    return true;
  }
});

const color = computed(() => {
  return checkName.value ? 'white' : '#f7b2a7';
});
</script>

<template>
  <div class="form-container">
    <div class="input-container">
      <div class="input-box">
        <span>名前:</span>
        <input class="input-name" v-model="inputtingName" />
      </div>
      <!-- v-if 文字数が多すぎる（false）の場合表示する -->
      <p v-if="!checkName" class="errorMessage">
        ※名前は{{ nameLengthLimit }}文字以内で入力してください
      </p>
      <div class="input-box">
        <span>年齢:</span>
        <input type="number" class="input" v-model="inputtingAge" />
      </div>
      <div class="input-box">
        <span>種類:</span>
        <select name="pets" class="select" id="pet-select" v-model="selectSpecies">
          <option value="">- 種類を選んでください -</option>
          <option value="犬">犬</option>
          <option value="猫">猫</option>
          <option value="うさき">うさぎ</option>
          <option value="ハムスター">ハムスター</option>
          <option value="鳥">鳥</option>
          <option value="その他">その他</option>
        </select>
      </div>
    </div>
    <!-- v-bindで10文字を超えると押せなくなるように組み込む -->
    <button @click="register" :disabled="!checkName" class="register-button">登録</button>
  </div>
</template>

<style scoped>
.form-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 340px;
  margin: 12px;
  padding: 24px 0;
  background: #f7715d;
  border-radius: 5px;
}

.input-container {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 24px;
}
.input-box {
  width: 240px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

span {
  font-size: 1rem;
  font-weight: bold;
}

.input-name,
.input {
  width: 180px;
  height: 32px;
  padding: 0 4px;
  border: none;
  border-radius: 5px;
}

.input-name {
  /* v-bindで変数を割り当てられる */
  background: v-bind(color);
}

.errorMessage {
  font-size: 0.75rem;
  font-weight: bold;
  color: #fff;
}

.select {
  width: 188px;
  height: 32px;
  padding: 0 4px;
  border: none;
  border-radius: 5px;
}

.register-button {
  width: 80px;
  height: 32px;
  border: none;
  border-radius: 5px;
  background: #f3f2f0;
  color: #594a46;
  font-size: 1rem;
  font-weight: bold;
  transition: 0.2s;
}
.register-button:hover {
  background: #d2d1cf;
}
</style>
