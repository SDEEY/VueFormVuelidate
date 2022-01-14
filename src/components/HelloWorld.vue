<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <form @submit.prevent="submitHandler">
      <div class="container">
        <div>
          <input v-model="lastname" placeholder="Фамилия" type="text">
          <p v-if="v$.lastname.$error">{{ v$.lastname.$errors[0].$message }}</p>
          <input v-model="name" placeholder="Имя" type="text">
          <p v-if="v$.name.$error">{{ v$.name.$errors[0].$message }}</p>
          <input placeholder="Отчество" type="text">
          <input v-model="dateOfBirth" placeholder="Дата рождения" type="text">
          <p v-if="v$.dateOfBirth.$error">{{ v$.dateOfBirth.$errors[0].$message }}</p>
          <input v-model="phoneNumber" placeholder="Номер телефона" type="text" @input="phoneNumberValidateHandler">
          <p v-if="v$.phoneNumber.$error && !this.phoneNumberCheck">{{ v$.phoneNumber.$errors[0].$message + '11 цифр. Начинается с 7' }}</p>
          <input placeholder="Пол" type="text">
          <div class="selectOfClients">
            <select v-model="selectedClients" multiple>
              <option value="" disabled>Группа клиентов</option>
              <option v-for="(client, index) in groupOfClients" :value="client" :key="index">{{ client.name }}</option>
            </select>
            <p>Выбрано:</p>
            <h3 v-for="(client, index) in selectedClients" :key="index">{{ client.name }}</h3>
            <p v-if="v$.selectedClients.$error">{{ v$.selectedClients.$errors[0].$message }}</p>
          </div>
          <select v-model="selectedTherapist">
            <option value="" disabled>Лечащий врач</option>
            <option v-for="(therapistt, index) in therapist" :value="therapistt" :key="index">{{
                therapistt.name
              }}
            </option>
          </select>
          <div class="divCheckbox">
            <input type="checkbox" v-model="doNotSendSMS">
            <span>Не отправлять СМС. </span>
          </div>
        </div>

        <div>
          <input placeholder="Индекс" type="text">
          <input placeholder="Страна" type="text">
          <input placeholder="Область" type="text">
          <input v-model="city" placeholder="Город" type="text">
          <p v-if="v$.city.$error">{{ v$.city.$errors[0].$message }}</p>
          <input placeholder="Улица" type="text">
          <input placeholder="Дом" type="text">


          <select v-model="selectedTypeOfDoc">
            <option value="" disabled>Тип документа</option>
            <option v-for="(typeOfDoc, index) in typeOfDocs" :value="typeOfDoc" :key="index">{{
                typeOfDoc.name
              }}
            </option>
          </select>
          <p v-if="v$.selectedTypeOfDoc.$error">{{ v$.selectedTypeOfDoc.$errors[0].$message }}</p>
          <input placeholder="Серия" type="text">
          <input placeholder="Номер" type="text">
          <input placeholder="Кем выдан" type="text">
          <input v-model="dateOfIssue" placeholder="Дата выдачи" type="text">
          <p v-if="v$.dateOfIssue.$error">{{ v$.dateOfIssue.$errors[0].$message }}</p>
        </div>
      </div>
      <button type="submit">send</button>
    </form>
  </div>
</template>

<script>
import useVuelidate from '@vuelidate/core'
import {required} from '@vuelidate/validators'

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data() {
    return {
      v$: useVuelidate(),
      lastname: '',
      name: '',
      dateOfBirth: '',
      phoneNumber: '',
      phoneNumberCheck: false,
      groupOfClients: [
        {name: 'VIP'},
        {name: 'Проблемные'},
        {name: 'ОМС'}
      ],
      selectedClients: [],
      therapist: [
        {name: 'Иванов'},
        {name: 'Захаров'},
        {name: 'Чернышева'}
      ],
      selectedTherapist: '',
      doNotSendSMS: false,
      city: '',
      typeOfDocs: [
        {name: 'Паспорт'},
        {name: 'Свидетельство о рождении'},
        {name: 'Вод. удостоверение'}
      ],
      selectedTypeOfDoc: '',
      dateOfIssue: '',
    }
  },
  validations() {
    return {
      lastname: {required},
      name: {required},
      dateOfBirth: {required},
      phoneNumber: {required},
      selectedClients: {required},
      city: {required},
      selectedTypeOfDoc: {required},
      dateOfIssue: {required},
    }
  },

  methods: {
    phoneNumberValidateHandler(e) {
      console.log(e.target.value.length)
      const value = e.target.value
      if (value && value[0] === '7' && value.length === 11) this.phoneNumberCheck = true
    },

    submitHandler() {
      this.v$.$validate()
      console.log(this.v$)

      if (!this.v$.$error && this.phoneNumberCheck) {
        alert('submited')
        return
      }

      alert('dont submited')
    },
  },
}
</script>

<style scoped>

input, select {
  display: block;
  width: 250px;
  min-height: 40px;
  margin-bottom: 10px;
}

h3, p {
  margin-bottom: 10px;
}

.container {
  display: flex;
}

.container div:first-child {
  margin-right: 20px;
}

input[type="checkbox"] {
  margin-right: 10px;
  margin-bottom: 0;
  width: 20px;
}

.divCheckbox {
  display: flex;
  align-items: center;
}

</style>
