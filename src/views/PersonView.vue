<script setup lang="ts">
import {ref, reactive, watch, nextTick} from 'vue';
class Person{
  id: number;
  name: string;
  surname: string;
  gender: string;
  static lastId: 1;
  constructor(name: string, surname: string, gender: string){
    this.id = Person.lastId++;
    this.name = name;
    this.surname = surname;
    this.gender = gender;
  }
}
const person = reactive<Person>({id: -1, name: '', surname: '', gender: ''});
const msg = reactive({name: '', surname: '', gender: ''});
const checkName = function(name: string){
    if(name.trim().length === 0) {
      msg.name = "First name is empty!!!"
      return false;
    }
    msg.name = ""
    return true;
}
const checkSurName = (surname: string) =>{
    if(surname.trim().length === 0) {
      msg.surname = "Second name is empty!!!"
      return false;
    }
    msg.surname = ""
    return true;
}

function checkgender(gender: string) {
  if(gender.trim().length === 0) {
      msg.gender = "Gender is empty!!!"
      return false;
    }
    msg.gender = ""
    return true;
}

function doSummit() {
  if(checkName(person.name) && checkSurName(person.surname) && checkgender(person.gender) ){
    const p = new Person(person.name, person.surname, person.gender)
    console.log(p);
    clearForm();
  }
}

const clearForm = function() {
  person.name = ""
  person.surname = ""
  person.gender = ""
  nextTick(() => {
    msg.name = ""
    msg.surname = ""
    msg.gender = ""
  })
}

watch(
  () => person.name,

    (name) => {
      checkName(name);
  }
)

watch(
  () => person.surname,

    (surname) => {
      checkSurName(surname);
  }
)

watch(
  () => person.gender,

    (gender) => {
      checkgender(gender);
  }
)
</script>

<template>
    <div>
        <form>
            <label for="name">FIrst Name</label>
            <input type="text" id="name" v-model="person.name" autocomplete="off"/>
            <span class="error">{{ msg.name }}</span>

            <label for="surname">Second Name</label>
            <input type="text" id="surname" v-model="person.surname" autocomplete="off"/>
            <span class="error">{{ msg.surname }}</span>

            <label for="gender">Gender</label>
            <select type="text" v-model="person.gender">
                <option hidden selected>Select Gender</option>
                <option value="M">Male</option>
                <option value="F">Female</option>
            </select>
            <span class="error">{{ msg.gender }}</span>
            <input type="submit" value="Summit" @click="doSummit"/>
        </form>
        <pre>{{ person }} {{ msg }}</pre>
    </div>
</template>

<style scoped>
input[type=text], select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

input[type=submit] {
  width: 100%;
  background-color: #4CAF50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

input[type=submit]:hover {
  background-color: #45a049;
}

div {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}
.error {
  color: red;
  font-size: smaller;
  display: block;
}
</style>