<script setup>
import { ref } from 'vue'
import useValidate from 'vue-tiny-validate'
import Card from './components/Card.vue'
import Toast from './components/Toast.vue'

const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
const formData = ref({
  firstName: '',
  lastName: '',
  email: '',
  queryType: '',
  message: '',
  isConsent: false
})
const rules = ref({
  firstName: {
    name: 'required',
    test: value => Boolean(value),
    message: 'This field is required'
  },
  lastName:
  {
    name: 'required',
    test: value => Boolean(value),
    message: 'This field is required'
  },

  email: [
    {
      name: 'required',
      test: value => Boolean(value),
      message: 'This field is required'
    },
    {
      name: 'pattern',
      test: value => Boolean(!value) || RegExp(emailRegex).test(value),
      message: 'Please enter a valid email address'
    },
  ],
  queryType:
  {
    name: 'required',
    test: value => Boolean(value),
    message: 'Please select a query type'
  },

  message:
  {
    name: 'required',
    test: value => Boolean(value),
    message: 'This field is required'
  },

  isConsent:
  {
    name: 'required',
    test: value => Boolean(value),
    message: 'To submit this form, please consent to being contacted'
  },

})
const options = ref({
  lazy: true,
  firstError: true
})
const { result } = useValidate(formData, rules)
function submitForm() {
  result.value.$test()
  if (result.value.$invalid) return

  showToast.value = true
  resetForm()
  setInterval(() => showToast.value = false, 3500)
}
const resetForm = () => {
  result.value.$reset()
  formData.value = {
    firstName: '',
    lastName: '',
    email: '',
    queryType: '',
    message: '',
    isConsent: false
  }
}
const showToast = ref(false)
</script>

<template>
      <Teleport to="body">
  <Toast v-if="showToast">
    <p class="flex items-center gap-2 mb-2 font-bold">
      <img src="/images/icon-success-check.svg" alt="icon success">
      <span>Message Sent!</span>
    </p>
    <p class="text-sm">Thanks for completing the form. We'll be in touch soon!</p>
  </Toast>
</Teleport>

  <div class="flex justify-center w-full min-h-screen px-4 py-8 bg-green-100 md:items-center">
    <Card>
      <h1 class="mb-8 text-3xl font-bold">Contact Us</h1>
      <form @submit.prevent="submitForm" class="flex flex-col w-full gap-6">
        <div class="flex flex-col w-full gap-6 md:flex-row md:gap-4">
          <div class="flex flex-col justify-start w-full gap-2">
            <label for="first-name" class="w-full">First Name *</label>
            <input v-model="formData.firstName" type="text" id="first-name" name="first-name" autocomplete="name"
              class="w-full px-4 py-3 border rounded-lg cursor-pointer focus:border-green-600 focus:outline-0"
              :class="{ 'border-red-600': result.firstName.$invalid }" />
            <p class="w-full text-sm text-red-600">
              <span v-for="message, index in result.firstName.$messages" :key="index">{{ message }}</span>
            </p>
          </div>
          <div class="flex flex-col justify-start w-full gap-2">
            <label for="last-name" class="w-full">Last Name *</label>
            <input v-model="formData.lastName" type="text" id="last-name" name="last-name"
              class="w-full px-4 py-3 border rounded-lg cursor-pointer focus:border-green-600 focus:outline-0"
              :class="{ 'border-red-600': result.lastName.$invalid }" />
            <p class="w-full text-sm text-red-600">
              <span v-for="message, index in result.lastName.$messages" :key="index">{{ message }}</span>
            </p>
          </div>
        </div>
        <div class="flex flex-col justify-start w-full gap-2">
          <label for="email" class="w-full">Email Address *</label>
          <input v-model="formData.email" type="text" id="email" name="email" autocomplete="email"
            class="w-full px-4 py-3 border rounded-lg cursor-pointer focus:border-green-600 focus:outline-0"
            :class="{ 'border-red-600': result.email.$invalid }" />
          <p class="w-full text-sm text-red-600">
            <span v-for="message, index in result.email.$messages" :key="index">{{ message }}</span>
          </p>
        </div>
        <div class="flex flex-col justify-start w-full gap-3">
          <p for="query-type" class="w-full">Query Type *</p>
          <div class="flex flex-col w-full gap-3 md:flex-row md:gap-4">
            <div
              class="group flex items-center w-full gap-3 px-6 py-3 border rounded-lg has-[:checked]:bg-green-100 has-[:checked]:border-green-600">
              <input v-model="formData.queryType" type="radio" name="query" id="query-general" value="general"
                class="scale-125 accent-green-600">
              <label class="group-hover:cursor-pointer" for="query-general">General Inquiry</label>
            </div>
            <div
              class="group flex items-center w-full gap-3 px-6 py-3 border rounded-lg has-[:checked]:bg-green-100 has-[:checked]:border-green-600">
              <input v-model="formData.queryType" type="radio" name="query" id="query-support" value="support"
                class="scale-125 accent-green-600">
              <label class="group-hover:cursor-pointer" for="query-support">Support Request</label>
            </div>
          </div>
          <p class="w-full text-sm text-red-600">
            <span v-for="message, index in result.queryType.$messages" :key="index">{{ message }}</span>
          </p>
        </div>
        <div class="flex flex-col justify-start w-full gap-2">
          <label for="message" class="w-full">Message *</label>
          <textarea v-model="formData.message" id="message" name="message" rows="6"
            class="w-full px-4 py-3 border rounded-lg cursor-pointer focus:border-green-600 focus:outline-0"
            :class="{ 'border-red-600': result.message.$invalid }"></textarea>
          <p class="w-full text-sm text-red-600">
            <span v-for="message, index in result.message.$messages" :key="index">{{ message }}</span>
          </p>
        </div>
        <div class="flex flex-col justify-start w-full gap-2">
          <div class="flex items-center w-full gap-4">
            <input @change="formData.isConsent = !formData.isConsent" :checked="formData.isConsent" type="checkbox"
              name="contact-consent" id="consent" class="scale-125 accent-green-600">
            <label class="cursor-pointer" for="consent">I consent to to being contacted by the team *</label>
          </div>
          <p class="w-full text-sm text-red-600">
            <span v-for="message, index in result.isConsent.$messages" :key="index">{{ message }}</span>
          </p>
        </div>
        <button type="submit"
          class="px-4 py-3 font-bold text-white bg-green-600 rounded-lg hover:bg-green-900 active:bg-green-950">Submit</button>
      </form>
    </Card>
  </div>
</template>
