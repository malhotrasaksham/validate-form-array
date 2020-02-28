<template>
  <div id="app">
    <form @submit.prevent="Submit">
      <div v-for="(item, index) in items" :key="item.id">
        <div class="controlRow">
          <span class="control" :class="{error: $v.items.$each[index].name.$error}">
            <input
              type="text"
              v-model="items[index].name"
              placeholder="Name"
              @blur="$v.items.$each[index].name.$touch()"
            >
          </span>
          <span class="control" :class="{error: $v.items.$each[index].age.$error}">
            <input
              type="text"
              v-model="items[index].age"
              placeholder="Age"
              @blur="$v.items.$each[index].age.$touch()"
            >
          </span>
          <button type="button" @click="RemoveItem(index)">Remove</button>
        </div>
      </div>
      <div class="actions">
        <button type="button" @click="AddItem">Add</button>
        <button type="submit" :disabled="!items.length || $v.items.$invalid">Submit</button>
      </div>
    </form>
  </div>
</template>

<script>
import { required, numeric, minValue } from "vuelidate/lib/validators";
export default {
  name: "App",
  data() {
    return {
      items: []
    };
  },
  validations: {
    items: {
      $each: {
        name: {
          required
        },
        age: {
          required,
          numeric,
          minValue: minValue(18)
        }
      }
    }
  },
  methods: {
    AddItem() {
      this.items.push({
        id: Date.now(),
        name: undefined,
        age: undefined
      });
    },
    Submit() {
      alert("Form Valid");
    },
    RemoveItem(index) {
      delete this.$v.items.$each[index];
      this.items.splice(index, 1);
      //this.$v.items.$touch();
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
<style scoped>
input {
  outline: none;
}
.error input {
  border: 1px solid red;
}
</style>
