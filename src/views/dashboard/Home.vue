<template>
  <div class="flex justify-between px-4 mt-4 sm:px-8">
    <h2 class="text-gray-900">
      <span class="text-xl"
        ><b>{{ loggedUser.name }}</b></span
      >
      Welcome to your dashboard
    </h2>

    <div class="flex items-center space-x-1 text-xs">
      <a href="#" class="font-bold text-indigo-700">Home</a>
      <svg xmlns="http://www.w3.org/2000/svg" class="h-2 w-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
      </svg>
      <span class="text-gray-600">Dashboard</span>
    </div>
  </div>

  <div class="mt-8 sm:grid-cols-5 sm:px-8 px-4">
    <div class="bg-white border rounded-lg shadow">
      <div class="flex justify-between items-center px-4 py-2 mb-2 border-b-2 text-gray-600">
        <h3 class="tracking-wider">My Companies</h3>
        <button class="border rounded-sm bg-gray-200 px-4 py-2 text-xs hover:bg-gray-300" @click="toggleModal()">
          Add New
        </button>
      </div>
      <div class="px-4">
        <table class="table-fixed min-w-full divide-y divide-gray-200">
          <thead>
            <tr>
              <th scope="col" class="py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Company Name
              </th>

              <th scope="col" class="py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Company Email
              </th>

              <th scope="col" class="py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Country
              </th>

              <th scope="col" class="py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Action
              </th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-200 text-sm text-gray-500">
            <tr v-for="company in companies" :key="company.id">
              <td class="py-4 whitespace-nowrap">{{ company.name }}</td>
              <td class="py-4 whitespace-nowrap text-sm text-gray-500">{{ company.email }}</td>
              <td class="py-4 whitespace-nowrap text-sm text-gray-500">{{ company.country }}</td>

              <td class="py-4 whitespace-nowrap text-sm text-blue-600">
                <a
                  @click="toggleModal2('modal' + company.id)"
                  class="border text-gray-100 rounded-sm bg-blue-700 px-4 py-2 text-xs hover:bg-gray-300"
                  >Edit</a
                >
              </td>
              <td class="py-4 whitespace-nowrap text-sm text-blue-600">
                <a
                  @click="showDeleteDailog(company.id)"
                  class="border rounded-sm text-gray-100 bg-red-600 pointer px-4 py-2 text-xs hover:bg-gray-300"
                  >Delete</a
                >
              </td>

              <div class="fixed z-10 overflow-y-auto top-0 w-full left-0 hidden" :id="'modal' + company.id">
                <div
                  class="flex items-center justify-center min-height-100vh pt-4 px-4 pb-20 text-center sm:block sm:p-0"
                >
                  <div class="fixed inset-0 transition-opacity">
                    <div class="absolute inset-0 bg-gray-900 opacity-75" />
                  </div>
                  <span class="hidden sm:inline-block sm:align-middle sm:h-screen">&#8203;</span>
                  <div
                    class="
                      inline-block
                      align-center
                      bg-white
                      rounded-lg
                      text-left
                      overflow-hidden
                      shadow-xl
                      transform
                      transition-all
                      sm:my-8 sm:align-middle sm:max-w-lg sm:w-full
                    "
                    role="dialog"
                    aria-modal="true"
                    aria-labelledby="modal-headline"
                  >
                    <form @submit.prevent="update('form' + company.id)" :id="'form' + company.id">
                      <input type="hidden" name="company_id" :value="company.id" />
                      <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                        <label>Company Name</label>
                        <input
                          type="text"
                          name="name"
                          class="w-full bg-gray-100 p-2 mt-2 mb-3"
                          v-bind:class="{ 'border-red-700': errors.name }"
                          :value="company.name"
                        />
                        <span v-if="errors.name" class="block text-sm text-red-700 -mt-2 mb-4">{{
                          errors.name[0]
                        }}</span>
                        <label>Company Email</label>
                        <input
                          type="email"
                          name="email"
                          class="w-full bg-gray-100 p-2 mt-2 mb-3"
                          v-bind:class="{ 'border-red-700': errors.email }"
                          :value="company.email"
                        />
                        <span v-if="errors.email" class="block text-sm text-red-700 -mt-2 mb-4">{{
                          errors.email[0]
                        }}</span>
                        <label>Company Location</label>
                        <input
                          type="text"
                          name="country"
                          class="w-full bg-gray-100 p-2 mt-2 mb-3"
                          v-bind:class="{ 'border-red-700': errors.country }"
                          :value="company.country"
                        />
                        <span v-if="errors.country" class="block text-sm text-red-700 -mt-2 mb-4">{{
                          errors.country[0]
                        }}</span>
                      </div>
                      <div class="bg-gray-200 px-4 py-3 text-right">
                        <button
                          type="button"
                          class="py-2 px-4 bg-gray-500 text-white rounded hover:bg-gray-700 mr-2"
                          @click="toggleModal2('modal' + company.id)"
                        >
                          <i class="fas fa-times"></i> Cancel
                        </button>
                        <button type="submit" class="py-2 px-4 bg-blue-500 text-white rounded hover:bg-blue-700 mr-2">
                          <i class="fas fa-plus"></i>
                          {{ btnText }}
                        </button>
                      </div>
                    </form>
                  </div>
                </div>
              </div>
            </tr>
          </tbody>
        </table>

        <div class="fixed z-10 overflow-y-auto top-0 w-full left-0 hidden" id="modal">
          <div class="flex items-center justify-center min-height-100vh pt-4 px-4 pb-20 text-center sm:block sm:p-0">
            <div class="fixed inset-0 transition-opacity">
              <div class="absolute inset-0 bg-gray-900 opacity-75" />
            </div>
            <span class="hidden sm:inline-block sm:align-middle sm:h-screen">&#8203;</span>
            <div
              class="
                inline-block
                align-center
                bg-white
                rounded-lg
                text-left
                overflow-hidden
                shadow-xl
                transform
                transition-all
                sm:my-8 sm:align-middle sm:max-w-lg sm:w-full
              "
              role="dialog"
              aria-modal="true"
              aria-labelledby="modal-headline"
            >
              <form @submit.prevent="store">
                <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                  <label>Company Name</label>
                  <input
                    type="text"
                    class="w-full bg-gray-100 p-2 mt-2 mb-3"
                    v-bind:class="{ 'border-red-700': errors.name }"
                    v-model="name"
                  />
                  <span v-if="errors.name" class="block text-sm text-red-700 -mt-2 mb-4">{{ errors.name[0] }}</span>
                  <label>Company Email</label>
                  <input
                    type="email"
                    class="w-full bg-gray-100 p-2 mt-2 mb-3"
                    v-bind:class="{ 'border-red-700': errors.email }"
                    v-model="email"
                  />
                  <span v-if="errors.email" class="block text-sm text-red-700 -mt-2 mb-4">{{ errors.email[0] }}</span>
                  <label>Company Location</label>
                  <input
                    type="text"
                    class="w-full bg-gray-100 p-2 mt-2 mb-3"
                    v-bind:class="{ 'border-red-700': errors.country }"
                    v-model="country"
                  />
                  <span v-if="errors.country" class="block text-sm text-red-700 -mt-2 mb-4">{{
                    errors.country[0]
                  }}</span>
                </div>
                <div class="bg-gray-200 px-4 py-3 text-right">
                  <button
                    type="button"
                    class="py-2 px-4 bg-gray-500 text-white rounded hover:bg-gray-700 mr-2"
                    @click="toggleModal()"
                  >
                    <i class="fas fa-times"></i> Cancel
                  </button>
                  <button type="submit" class="py-2 px-4 bg-blue-500 text-white rounded hover:bg-blue-700 mr-2">
                    <i class="fas fa-plus"></i>
                    {{ btnText }}
                  </button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import VueApexCharts from 'vue3-apexcharts'
import { TabGroup, TabList, Tab, TabPanels, TabPanel } from '@headlessui/vue'
import topAuthors from '@/data/home/topAuthors.json'
import recentSales from '@/data/home/recentSales.json'
import Auth from '../../Auth.js'
import Swal from 'sweetalert2'

export default {
  components: {
    apexchart: VueApexCharts,
    TabGroup,
    TabList,
    Tab,
    TabPanels,
    TabPanel,
  },
  mounted() {
    this.getCompanies()
  },
  methods: {
    toggleModal() {
      document.getElementById('modal').classList.toggle('hidden')
    },
    toggleModal2(id) {
      document.getElementById(id).classList.toggle('hidden')
    },
    showMessage(icon, message, title) {
      Swal.fire({
        icon: icon,
        title: title,
        text: message,
      })
    },
    showDeleteDailog(id) {
      Swal.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!',
      }).then((result) => {
        if (result.isConfirmed) {
          this.RemoveCompany(id)
        }
      })
    },
    store() {
      // POST request using fetch with error handling
      let url = import.meta.env.VITE_API_BASE_URL + 'company/store'
      // alert(import.meta.env.VITE_API_BASE_URL)
      this.disableButton = true
      this.btnText = 'Please wait...'
      const requestOptions = {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          Accept: 'application/json',
          Authorization: 'Bearer ' + Auth.token,
        },
        body: JSON.stringify({
          email: this.email,
          name: this.name,
          country: this.country,
        }),
      }
      fetch(url, requestOptions)
        .then(async (response) => {
          const data = await response.json()
          this.disableButton = false
          this.btnText = 'Submit'
          // check for error response
          if (!response.ok) {
            this.btnText = 'Submit'
            console.log(data.errors)
            this.errors = data.errors
            // get error message from body or default to response status
            const error = (data && data.message) || response.status
            return Promise.reject(error)
          }

          this.loginFailed = false
          this.showMessage('success', 'Company Added', 'success')
          setTimeout(function () {
            window.location.href = '/'
          }, 3000)
        })
        .catch((error) => {
          //alert(process.env.VUE_APP_API_BASE_URL);
          this.btnText = 'Submit'
          this.loginFailed = true
          // this.presentAlert(error);
          // alert(error)
          this.showMessage('error', error, 'sorry')
          this.errorMessage = error
          console.log(error)
        })
    },

    update(formId) {
      // POST request using fetch with error handling
      let url = import.meta.env.VITE_API_BASE_URL + 'company/update'
      // alert(import.meta.env.VITE_API_BASE_URL)
      let myForm = document.getElementById(formId)
      let formData = new FormData(myForm)
      this.disableButton = true
      this.btnText = 'Please wait...'
      const requestOptions = {
        method: 'POST',
        headers: {
          //  'Content-Type': 'application/json',
          Accept: 'application/json',
          Authorization: 'Bearer ' + Auth.token,
        },
        body: formData,
      }
      fetch(url, requestOptions)
        .then(async (response) => {
          const data = await response.json()
          this.disableButton = false
          this.btnText = 'Submit'
          // check for error response
          if (!response.ok) {
            this.btnText = 'Submit'
            console.log(data.errors)
            this.errors = data.errors
            // get error message from body or default to response status
            const error = (data && data.message) || response.status
            return Promise.reject(error)
          }

          this.loginFailed = false
          this.showMessage('success', 'Company updated', 'success')
          setTimeout(function () {
            window.location.href = '/'
          }, 2000)
        })
        .catch((error) => {
          //alert(process.env.VUE_APP_API_BASE_URL);
          this.btnText = 'Submit'
          this.loginFailed = true
          // this.presentAlert(error);
          this.showMessage('error', error, 'sorry')
          this.errorMessage = error
          console.log(error)
        })
    },
    getCompanies() {
      // POST request using fetch with error handling
      let url = import.meta.env.VITE_API_BASE_URL + 'companies'
      const requestOptions = {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json',
          Accept: 'application/json',
          Authorization: 'Bearer ' + Auth.token,
        },
      }
      fetch(url, requestOptions)
        .then(async (response) => {
          const data = await response.json()
          // check for error response
          if (!response.ok) {
            console.log(data.errors)
            this.errors = data.errors
            // get error message from body or default to response status
            const error = (data && data.message) || response.status
            return Promise.reject(error)
          }

          this.companies = data.data
        })
        .catch((error) => {
          this.errorMessage = error
          console.log(error)
        })
    },
    RemoveCompany(id) {
      // POST request using fetch with error handling
      let url = import.meta.env.VITE_API_BASE_URL + 'company/delete/' + id
      

      this.disableButton = true
      this.btnText = 'Please wait...'
      const requestOptions = {
        method: 'DELETE',
        headers: {
          Accept: 'application/json',
          Authorization: 'Bearer ' + Auth.token,
        },
        body: '',
      }
      fetch(url, requestOptions)
        .then(async (response) => {
          const data = await response.json()
          this.disableButton = false
          this.btnText = 'Submit'
          // check for error response
          if (!response.ok) {
            this.btnText = 'Submit'
            console.log(data.errors)
            this.errors = data.errors
            // get error message from body or default to response status
            const error = (data && data.message) || response.status
            return Promise.reject(error)
          }

          this.loginFailed = false
          this.showMessage('success', 'Record Deleted', 'success')
          setTimeout(function () {
            window.location.href = '/'
          }, 2000)
        })
        .catch((error) => {
          this.btnText = 'Submit'
          this.loginFailed = true
          
          alert(error)
          this.errorMessage = error
          console.log(error)
        })
    },
  },

  data: () => ({
    errors: [],
    loggedUser: Auth.user,
    loginFailed: false,
    // admission_number: "",
    btnText: 'Submit',
    email: '',
    name: '',
    country: '',
    companies: [],
  }),
  setup() {
    const pageViewOptions = {
      chart: {
        id: 'pageview-chart',
        toolbar: {
          show: false,
        },
      },
      dataLabels: {
        enabled: false,
      },
      xaxis: {
        categories: ['Aug 1', 'Aug 2', 'Aug 3', 'Aug 4', 'Aug 5', 'Aug 6', 'Aug 7'],
      },
    }

    const pageViewSeries = [
      {
        name: 'Page view',
        data: [3087, 4403, 3456, 2864, 4956, 6023, 7063],
      },
    ]

    const sessionsOptions = {
      chart: {
        id: 'sessions-chart',
      },
      labels: ['Desktop', 'Mobile', 'Tablet'],
      legend: {
        position: 'bottom',
      },
    }

    const sessionsSeries = [70.9, 27.2, 1.9]

    return {
      pageViewOptions,
      pageViewSeries,
      sessionsOptions,
      sessionsSeries,
      topAuthors,
      recentSales,
    }
  },
}
</script>
