<template>
  <fieldset
    class="border border-slate-700 rounded-md absolute sm:w-[60vw] ml-0 lg:ml-10 bg-dark-primary-color overflow-hidden z-40 m-auto"
  >
    <legend class="ml-10">Edit Profile</legend>
    <v-icon
      name="fa-times"
      fill="#302f31"
      scale="1"
      @click="closeAdd"
      class="absolute right-3 cursor-pointer"
    />
    <div class="form-container w-full p-10 h-full flex flex-wrap justify-center gap-5 align-middle">
      <div
        v-for="item in userInputField"
        :key="item.id"
        class="w-full sm:w-5/12 text-secondary-color"
      > 
        <label :for="item.name" class="text-sm font-helvetica text-primary-text-color pl-3">
          {{ item.label }}
        </label>
        <input
          :type="item.type"
          :name="item.name"
          @blur="validateField(item.name)"
          v-model="user[item.name]"
          class="p-2 focus:outline-none w-full h-10 mb rounded-3xl border border-black focus:border-hover-yellow focus:ring focus:ring-btn-yellow focus:ring-opacity-50 text-black"
        />
        <span v-if="formErrors[item.name]" class="text-orange-300 pl-3 text-sm">{{
          formErrors[item.name]
        }}</span>
      </div>
      <div class="w-full sm:w-[20%]  self-center text-secondary-color flex flex-col mt-2">
        <label
          for="profile"
          class="cursor-pointer items-center p-2 text-sm text-gray-900 bg-gray-50 rounded-full focus-within:outline-none focus-within:border-hover-yellow focus-within:ring focus-within:ring-btn-yellow focus-within:ring-opacity-50"
          >Profile Pic</label
        >
        <input
          type="file"
          id="profile"
          name="profile"
          @change="handleProfileChange"
          class="hidden"
        />
        <span v-if="formErrors.profile" class="text-orange-300 mt-1 pl-3 block text-sm">{{
          formErrors.profile
        }}</span>
      </div>
      <div class="w-full sm:w-[20%] self-center text-secondary-color flex flex-col mt-2">
        <label
          for="cover"
          class="cursor-pointer items-center p-2 text-sm text-gray-900 bg-gray-50 rounded-full focus-within:outline-none focus-within:border-hover-yellow focus-within:ring focus-within:ring-btn-yellow focus-within:ring-opacity-50"
          >Cover Pic</label
        >
        <input type="file" id="cover" name="cover" @change="handleCoverChange" class="hidden" />

        <span v-if="formErrors.file" class="text-orange-300 mt-1 pl-3 block text-sm">{{
          formErrors.file
        }}</span>
      </div>
      
        <div class="w-full flex flex-col sm:w-5/12">
        <label for="gender" class="text-sm font-helvetica text-primary-text-color pl-3">Gender</label>
        <select
          v-model="user.gender"
          id="gender"
          name="gender"
          class="rounded-3xl px-3 py-2 mt-2 border border-black text-black focus:outline-none focus:border-hover-yellow focus:ring focus:ring-btn-yellow focus:ring-opacity-50"
        >
          <option value="" disabled>Select Gender</option>
          <option value="0">Male</option>
          <option value="1">Female</option>
          <option value="2">Other</option>
        </select>
        <span v-if="formErrors.gender" class="text-orange-300">{{ formErrors.gender }}</span>
      </div>

      <div class="w-full flex flex-col sm:w-5/12">
        <label for="country" class="text-sm font-helvetica text-primary-text-color pl-3">
          Country
        </label>
        <select
          v-model="user.country"
          id="country"
          name="country"
          class="rounded-3xl px-3 py-2 mt-2 border border-black text-black focus:outline-none focus:border-hover-yellow focus:ring focus:ring-btn-yellow focus:ring-opacity-50"
        >
          <option value="" disabled>Select Country</option>
          <option v-for="country in countryOptions" :key="country" :value="country">
            {{ country }}
          </option>
        </select>
        <span v-if="formErrors.country" class="text-orange-300">{{ formErrors.country }}</span>
      </div>

      <div class="w-full flex justify-center gap-2 align-middle">
        <button
          class="bg-btn-yellow h-10 w-2/6 hover:text-secondary-color text-slate-200 text-md rounded-full hover:border hover:bg-transparent border-secondary-color bg-secondary-color"
          type="submit"
          @click="editUser()"
        >
          Add User
        </button>
      </div>
    </div>
  </fieldset>
</template>

<script setup>
import { ref, onMounted} from 'vue'
import axios from 'axios'
import {defineEmits, defineProps } from 'vue'
import { useToast } from 'vue-toast-notification'
const $toast = useToast()
const countryOptions = ref([])

const props = defineProps(['userData'])
const user = ref({
  firstname: props.userData.firstname,
  lastname: props.userData.lastname,
  username: props.userData.username,
  email: props.userData.email,
  dob: props.userData.dob,
  password: '',
  Repassword: '',
  country: props.userData.country,
  gender: props.userData.gender
})


const emit = defineEmits(['close'])

function closeAdd() {
  emit('close')
}

const userInputField = ref([
  { id: '1', name: 'firstname', type: 'text', label: 'First Name' },
  { id: '2', name: 'lastname', type: 'text', label: 'Last Name' },
  { id: '3', name: 'username', type: 'text', label: 'Username' },
  { id: '4', name: 'email', type: 'email', label: 'Email' },
  { id: '5', name: 'password', type: 'password', label: 'Password' },
  { id: '6', name: 'Repassword', type: 'password', label: 'Confirm Password' },
  { id: '7', name: 'dob', type: 'date', label: 'Date of Birth' }
])

const formErrors = ref({})
const access_token = localStorage.getItem('access_token')

const validateField = (fieldName) => {
  formErrors.value[fieldName] = ''

  if (fieldName === 'password' && user.value.password.length < 8) {
    formErrors.value.password = 'Password should be at least 8 characters long.'
  } else if (fieldName === 'Repassword' && user.value.password !== user.value.Repassword) {
    formErrors.value.Repassword = 'Passwords do not match.'
  }
}
const profileFile = ref(null)
const coverFile = ref(null)
const handleProfileChange = (event) => {
  profileFile.value = event.target.files[0]
}

const handleCoverChange = (event) => {
 coverFile.value = event.target.files[0]
}

const editUser = () => {
  formErrors.value = {}

  if (user.value.password.length < 8) {
    formErrors.value.password = 'Password should be at least 8 characters long.'
  } else if (!/(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])/.test(user.value.password)) {
    formErrors.value.password =
      'Password should contain at least one uppercase letter, one lowercase letter, one digit, and one special character.'
  }
  if (user.value.password !== user.value.Repassword) {
    formErrors.value.Repassword = 'Passwords do not match.'
  }
  if (user.value.username.length < 5) {
    formErrors.value.username = 'Username should be at least 5 characters long.'
  }
  if (!user.value.email) {
    formErrors.value.email = 'Please provide an email.'
  }
  if (!user.value.country) {
    formErrors.value.country = 'Please provide your country.'
  }

  if (Object.keys(formErrors.value).length === 0) {
    const formData = new FormData()
    formData.append('email', user.value.email)
    formData.append('password', user.value.password)
    formData.append('firstname', user.value.firstname)
    formData.append('lastname', user.value.lastname)
    formData.append('username', user.value.username)
    if (user.value.dob) {
      formData.append('dob', user.value.dob)
    }
    formData.append('gender', user.value
    .gender)
    formData.append('country', user.value.country)
    formData.append('is_user', 'True')
    if (profileFile.value) {
      formData.append('img_profile', profileFile.value)
    }
    if (coverFile.value) {
      formData.append('img_cover', coverFile.value)
    }
    formData.append('is_active', 'True')
    axios
      .patch('http://127.0.0.1:8000/api/user/edit/', formData,  {
          headers: {
              Authorization: `Bearer ${localStorage.getItem('access_token')}`,
            'Content-Type': 'application/json'
          }
        })
      .then((response) => {
        console.log('registered')
        $toast.success("User added successfully");
        closeAdd();
      })
      .catch((error) => {
        console.error(error)
        $toast.error("Error while adding user")
      })
  }
}
const fetchCountries = async () => {
  try {
    const response = await axios.get('http://127.0.0.1:8000/api/user/countrylistview/')
    countryOptions.value = response.data.countries
  } catch (error) {
    console.error('Error fetching countries:', error)
  }
}
onMounted(() => {
  fetchCountries()
})
</script>
