<script setup>
import { ref, computed } from 'vue'
import Button from './components/Button.vue'

const filterText = ref("")
const nameList = ref(["Emil, Hans", "Mustermann, Max", "Tisch, Roman"])
const selectedFirst = ref("")
const selectedLast = ref("")
const selected = ref("")

const filteredNames = computed(() => {
  return nameList.value.filter((name) => 
    name.split(', ')[0].startsWith(filterText.value)
  )
})

const resetSelectedFields = () => {
  selectedFirst.value = ""
  selectedLast.value = ""
}

const create = () => {
  nameList.value.push(selectedLast.value + ", " + selectedFirst.value)
  resetSelectedFields()
}

const update = () => {
  const updateIndex = nameList.value.indexOf(selected.value)
  nameList.value[updateIndex] = selectedLast.value + ", " + selectedFirst.value
  selected.value = selectedLast.value + ", " + selectedFirst.value
}

const remove = () => {
  const deleteIndex = nameList.value.indexOf(selected.value)
  nameList.value.splice(deleteIndex, 1)
  resetSelectedFields()
}

const updateInputOnSelect = (selectedName) => {
  [selectedLast.value, selectedFirst.value] = selectedName.split(', ')
  selected.value = selectedName
}

</script>

<template>
    <div>
      <input placeholder="Filter prefix" v-model="filterText">
    </div>

    <select size="5" @change="(event) => updateInputOnSelect(event.target.value)">
      <option v-for="name in filteredNames">
        {{ name }}
      </option>
    </select>

    <label for="FirstName">
      Name:
      <input id="FirstName" v-model="selectedFirst">
    </label>

    <label for="LastName">
      Surname:
      <input id="LastName" v-model="selectedLast">
    </label>

    <div class="buttons">
      <Button text="Create" @click="create"/>
      <Button text="Update" @click="update"/>
      <Button text="Delete" @click="remove"/>
    </div>
</template>

<style scoped>

input {
  display: block;
}

select {
  margin: 10px 15px 0px 0px;
  width: 200px;
  float: left;
}

.buttons {
  display: flex;
  gap: 5px;
  margin-top: 20px;
}

</style>
