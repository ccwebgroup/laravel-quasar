<template>
  <q-page padding>
    <q-card style="width: 400px" class="q-mx-auto q-mt-md">
      <q-card-section>
        <form @submit="addEmployee" class="q-gutter-md">
          <q-input v-model="form.name" label="Name" outlined />
          <q-input v-model="form.email" label="Email" outlined />
          <q-input v-model="form.password" label="Password" outlined />
          <q-input v-model="form.age" label="Age" outlined />

          <q-btn class="mybtn" type="submit" label="Submit" color="indigo" />
        </form>
      </q-card-section>
    </q-card>

    <q-list v-for="item in employees" :key="item">
      <q-item>
        <q-item-section avatar>
          <q-avatar size="50px" color="grey-3">
            <q-icon name="person"></q-icon>
          </q-avatar>
        </q-item-section>

        <q-item-section> {{ item.email }} </q-item-section>

        <q-item-section side>
          <q-btn
            @click="deleteEmployee(item)"
            round
            flat
            color="negative"
            icon="delete"
          ></q-btn>
        </q-item-section>
      </q-item>
    </q-list>

    <q-dialog v-model="userDialog">
      <q-card>
        <q-card-section>
          <p>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Error
            quisquam magnam eaque earum, autem molestiae voluptatum vero at iure
            soluta similique repellendus saepe quasi? Omnis non aut cumque saepe
            optio!
          </p>
        </q-card-section>
      </q-card>
    </q-dialog>

    <q-btn @click="userDialog = true" label="Click" />
  </q-page>
</template>

<style lang="scss">
.mybtn {
  transition: transform 250ms;
}

.mybtn:hover {
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.15);
  transform: translateX(5px);
}
</style>

<script setup>
import { reactive, ref, onBeforeMount } from "vue";
import axios from "axios";

const userDialog = ref(false);

const form = reactive({
  name: "",
  email: "",
  password: "",
  age: "",
});

const employees = ref([]);

async function addEmployee() {
  const response = await axios.post(
    "http://localhost:8000/api/employees/",
    form
  );
  employees.value.unshift(response.data);
}

async function getAllEmployees() {
  const response = await axios.get("http://localhost:8000/api/employees/");
  employees.value = response.data;
}

async function deleteEmployee(employee) {
  await axios.delete("http://localhost:8000/api/employees/" + employee.id);
  employees.value = employees.value.filter((item) => {
    return item.id != employee.id;
  });
}

onBeforeMount(() => {
  getAllEmployees();
});
</script>
