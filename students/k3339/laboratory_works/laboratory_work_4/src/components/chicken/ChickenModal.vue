<script setup>
import {ref, watch} from "vue";


const props = defineProps({
  modelValue: {
    type: Boolean,
    required: true,
  },
  chickenData: {
    type: Object,
  },
  breeds: {
    type: Array
  },
  mode: {
    type: String,
    default: "add",
  },

});


const emits = defineEmits(["update:modelValue", "submit-chicken"]);

const formData = ref({...props.chickenData});

watch(
    () => props.chickenData,
    (newVal) => {
      formData.value = {...newVal};
    }
);


function closeModal() {
  emits("update:modelValue", false);
}

function handleSubmit() {
  emits("submit-chicken", {...formData.value, cell: formData.value.cell.cell_code});
  closeModal();
}
</script>

<template>
  <v-dialog :model-value="modelValue" @update:model-value="closeModal" persistent max-width="500">
    <v-card>
      <v-card-title>{{ mode === "add" ? "Добавить курицу" : "Редактировать курицу" }}</v-card-title>
      <v-card-text>
        <v-form @submit.prevent="handleSubmit">
          <v-text-field v-model="formData.weight" label="Вес" required type="number"></v-text-field>
          <v-text-field v-model="formData.age" label="Возраст" required type="number"></v-text-field>
          <v-select
              v-model="formData.breed"
              :items="breeds"
              item-title="name"
              item-value="id"
              label="Порода"/>
          <v-text-field v-model="formData.egg_performance_month" label="Яйценоскость за месяц" required type="number"></v-text-field>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-btn color="secondary" @click="closeModal">Отмена</v-btn>
        <v-btn color="primary" @click="handleSubmit">Сохранить</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>