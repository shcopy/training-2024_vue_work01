<!--
餐點管理工具

Level 1：將菜單轉為資料格式
Level 2：可以重新設定菜單的庫存數量
Level 3（挑戰）：可以重新設定品項名稱
作業需求：

將以下的表格轉為使用資料，並使用 Vue 進行渲染。
數量的部分可以點擊，並且調整庫存數量。
庫存數量不會低於 0。
新增欄位 “編輯”，按下後可以修改該欄位的品項名稱（按下確認後執行更換）

<table>
  <thead>
    <tr>
      <th scope="col">品項</th>
      <th scope="col">描述</th>
      <th scope="col">價格</th>
      <th scope="col">庫存</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>珍珠奶茶</td>
      <td><small>香濃奶茶搭配QQ珍珠</small></td>
      <td>50</td>
      <td><button>-</button>20<button>+</button></td>
    </tr>
    <tr>
      <td>冬瓜檸檬</td>
      <td><small>清新冬瓜配上新鮮檸檬</small></td>
      <td>45</td>
      <td><button>-</button>18<button>+</button></td>
    </tr>
    <tr>
      <td>翡翠檸檬</td>
      <td><small>綠茶與檸檬的完美結合</small></td>
      <td>55</td>
      <td><button>-</button>34<button>+</button></td>
    </tr>
    <tr>
      <td>四季春茶</td>
      <td><small>香醇四季春茶，回甘無比</small></td>
      <td>45</td>
      <td><button>-</button>10<button>+</button></td>
    </tr>
    <tr>
      <td>阿薩姆奶茶</td>
      <td><small>阿薩姆紅茶搭配香醇鮮奶</small></td>
      <td>50</td>
      <td><button>-</button>25<button>+</button></td>
    </tr>
    <tr>
      <td>檸檬冰茶</td>
      <td><small>檸檬與冰茶的清新組合</small></td>
      <td>45</td>
      <td><button>-</button>20<button>+</button></td>
    </tr>
    <tr>
      <td>芒果綠茶</td>
      <td><small>芒果與綠茶的獨特風味</small></td>
      <td>55</td>
      <td><button>-</button>18<button>+</button></td>
    </tr>
    <tr>
      <td>抹茶拿鐵</td>
      <td><small>抹茶與鮮奶的絕配</small></td>
      <td>60</td>
      <td><button>-</button>20<button>+</button></td>
    </tr>
  </tbody>
</table>
-->

<script setup>
import { ref } from "vue";

const data = [
  {
    id: 1,
    name: "珍珠奶茶",
    description: "香濃奶茶搭配QQ珍珠",
    price: 50,
    stock: 20,
  },
  {
    id: 2,
    name: "冬瓜檸檬",
    description: "清新冬瓜配上新鮮檸檬",
    price: 45,
    stock: 15,
  },
  {
    id: 3,
    name: "翡翠檸檬",
    description: "綠茶與檸檬的完美結合",
    price: 55,
    stock: 30,
  },
  {
    id: 4,
    name: "四季春茶",
    description: "香醇四季春茶，回甘無比",
    price: 45,
    stock: 10,
  },
  {
    id: 5,
    name: "阿薩姆奶茶",
    description: "阿薩姆紅茶搭配香醇鮮奶",
    price: 50,
    stock: 25,
  },
  {
    id: 6,
    name: "檸檬冰茶",
    description: "檸檬與冰茶的清新組合",
    price: 45,
    stock: 20,
  },
  {
    id: 7,
    name: "芒果綠茶",
    description: "芒果與綠茶的獨特風味",
    price: 55,
    stock: 18,
  },
  {
    id: 8,
    name: "抹茶拿鐵",
    description: "抹茶與鮮奶的絕配",
    price: 60,
    stock: 20,
  },
];

const drinks = ref(data);
const tempEdit = ref({
  id: '',
  name: '',
});

// 新增
const addDrink = () => {
  if (text.value) {
    drinks.value.push({
      text: text.value,
      id: new Date().getTime()
    });
    // console.log(drinks.value);
    text.value = '';
  }
}

// 刪除
const deleteDrink = (drink) => {
  console.log(drink);
  const index = drinks.value.findIndex(item => item.id === drink.id);
  // console.log(index);

  drinks.value.splice(index, 1);
  // console.log(drink);
}

// 編輯
const prepareEdit = (drink) => {
  tempEdit.value = { ...drink }; // 拷貝技巧 血輪眼
  console.log(tempEdit.value);
  // 傳參考，純 JS 的鍋：物件傳參考
}

const confirmEdit = () => {
  const index = drinks.value.findIndex(item => item.id === tempEdit.value.id);
  console.log(index, drinks.value);

  // 將 tempEdit 的值帶回去 drinks
  drinks.value[index] = tempEdit.value;

  // 清空
  tempEdit.value = {}
}

function handleDrinkStock(id, stock) {
  drinks.value = drinks.value.map((item) => {
    if (item.id === id) {
      item.stock = stock;
    }
    return item;
  });
}
</script>

<template>
  <table>
    <thead>
      <tr>
        <th scope="col">品項</th>
        <th scope="col">描述</th>
        <th scope="col">價格</th>
        <th scope="col">庫存</th>
        <th scope="col"></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="drink in drinks" :key="drink.id">
        <td>{{ drink.name }}<button type="button" @click="prepareEdit(drink)">編輯</button></td>
        <td>
          <small>{{ drink.description }}</small>
        </td>
        <td>{{ drink.price }}</td>
        <td>
          <button @click="handleDrinkStock(drink.id, drink.stock - 1)" :disabled="drink.stock < 1">
            -
          </button>
          {{ drink.stock }}
          <button @click="handleDrinkStock(drink.id, drink.stock + 1)">+</button>
        </td>
        <td>
          <button type="button" @click="deleteDrink(drink)">刪除</button>
        </td>
      </tr>
    </tbody>
  </table>

  <hr />
  <div v-if="tempEdit.id">
    <h1>編輯區域</h1>
    當前修改的值: {{ tempEdit.name }}<br />
    <input type="text" v-model="tempEdit.name">
    <button type="button" @click="confirmEdit()">確認編輯</button>
  </div>
</template>

<style></style>
