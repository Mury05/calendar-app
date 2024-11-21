<script setup>
import { ref, onMounted } from 'vue'

const openModal = ref(true)
const checkedDay = ref('Monday')
const libelle = ref('')
const update = ref(false)
const dayUpdate = ref()
const evUpdate = ref()

const calendar = ref()

const agenda = ref([
        { day: 'Monday', event: [] },
        { day: 'Tuesday', event: [] },
        { day: 'Wednesday', event: [] },
        { day: 'Thursday', event: [] },
        { day: 'Friday', event: [] },
        { day: 'Saturday', event: [] },
        { day: 'Sunday', event: [] },
      ])

onMounted(() => {
  calendar.value = persistCalendar.getPersistData('event')
    ? persistCalendar.getPersistData('event')
    : agenda.value
})

const submitEvent = () => {
  update.value = false
  calendar.value = persistCalendar.getPersistData('event')
    ? persistCalendar.getPersistData('event')
    : agenda.value

  if (libelle.value.trim()) {
    const day = calendar.value.find(d => d.day === checkedDay.value)
    if (day) {
      day.event.push({ libelle: libelle.value })
    } else {
      calendar.value.push({
        day: checkedDay.value,
        event: [{ libelle: libelle.value }],
      })
    }
    libelle.value = ''
    const Toast = Swal.mixin({
      toast: true,
      position: 'top-end',
      showConfirmButton: false,
      timer: 5000,
      timerProgressBar: true,
      didOpen: toast => {
        toast.onmouseenter = Swal.stopTimer
        toast.onmouseleave = Swal.resumeTimer
      },
    })
    Toast.fire({
      icon: 'success',
      title: 'Event added successfully !',
    })

    persistCalendar.setPersistData('event', calendar.value)
  }
}

const editEvent = (dayIndex, eventIndex) => {
  update.value = true
  dayUpdate.value = dayIndex
  evUpdate.value = eventIndex
  const event = calendar.value[dayIndex].event[eventIndex]
  libelle.value = event.libelle
  checkedDay.value = calendar.value[dayIndex].day // Set the checked day
  openModal.value = true // Open modal for editing
}

const updateEvent = () => {
  calendar.value = persistCalendar.getPersistData('event')
    ? persistCalendar.getPersistData('event')
    : agenda.value

  const day = calendar.value.find((_, i) => i === dayUpdate.value)

  if (day) {
    day.event[evUpdate.value].libelle = libelle.value
    update.value = false
    libelle.value = ''
    dayUpdate.value = ''
    evUpdate.value = ''

    persistCalendar.setPersistData('event', calendar.value)

    const Toast = Swal.mixin({
      toast: true,
      position: 'top-end',
      showConfirmButton: false,
      timer: 5000,
      timerProgressBar: true,
      didOpen: toast => {
        toast.onmouseenter = Swal.stopTimer
        toast.onmouseleave = Swal.resumeTimer
      },
    })
    Toast.fire({
      icon: 'success',
      title: 'Event updated successfully !',
    })
  }
}

const deleteEvent = (dayIndex, eventIndex) => {
  Swal.fire({
    title: 'Are you sure?',
    text: "You won't be able to revert this!",
    icon: 'warning',
    showCancelButton: true,
    confirmButtonColor: '#15803d',
    cancelButtonColor: '#ef4444',
    confirmButtonText: 'Yes, delete it!',
  }).then(result => {
    if (result.isConfirmed) {
      calendar.value = persistCalendar.getPersistData('event')
        ? persistCalendar.getPersistData('event')
        : agenda.value

      calendar.value[dayIndex].event.splice(eventIndex, 1)
      if (calendar.value[dayIndex].event.length === 0) {
        // calendar.value.splice(dayIndex, 1) // Remove the day if no events left

     

      }
      persistCalendar.setPersistData('event', calendar.value)

      Swal.fire({
        title: 'Deleted!',
        text: 'Your event has been deleted.',
        icon: 'success',
        confirmButtonColor: '#15803d',
      })
    } else {
      ;(libelle.value = ''), (update.value = false)
    }
  })
}

class PersistData {
  setPersistData(key, itemsList) {
    sessionStorage.setItem(key, JSON.stringify(itemsList))
  }

  getPersistData(key) {
    let data = sessionStorage.getItem(key)
    return JSON.parse(data)
  }
}

let persistCalendar = new PersistData()
</script>

<template>
  <div class="flex justify-between mx-auto w-3/4">
    <div>
      <svg
        data-name="Layer 1"
        xmlns="http://www.w3.org/2000/svg"
        width="701.33071"
        height="400.73993"
        viewBox="0 0 866.33071 605.73993"
        xmlns:xlink="http://www.w3.org/1999/xlink"
      >
        <path
          d="M898.30315,567.7887a9.14581,9.14581,0,0,1,1.93088-13.89051l-7.25159-31.682,15.8187,5.91773,4.46105,29.1785a9.19542,9.19542,0,0,1-14.959,10.47626Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#ffb8b8"
        />
        <path
          d="M897.20867,542.64389l-13.906-26.88477,1.88062-67.69726.89355,1.88281c1.041,2.19336,25.44019,53.84619,23.7,71.13819l2.78882,15.80224Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#3f3d56"
        />
        <polygon
          points="721.307 594.201 732.458 594.201 737.762 551.192 721.305 551.193 721.307 594.201"
          fill="#ffb8b8"
        />
        <path
          d="M885.29761,737.691l21.95908-.00089h.00155a13.99379,13.99379,0,0,1,13.99352,13.99352v.45476l-35.95371.00177Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#2f2e41"
        />
        <polygon
          points="611.134 585.727 621.877 588.714 638.511 548.699 622.655 544.29 611.134 585.727"
          fill="#ffb8b8"
        />
        <path
          d="M776.20352,728.58778,797.36,734.47044l.00149.00042a13.99378,13.99378,0,0,1,9.7326,17.23118l-.12184.43813-34.63966-9.63139Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#2f2e41"
        />
        <path
          d="M877.02434,724.5643l-22.50366-161.126L805.91619,723.65317l-38.21692-13.04981,54.77454-214.53271.36035-.02539,57.38061-3.99121,41.45679,219.48779Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#2f2e41"
        />
        <path
          d="M871.07293,420.14509s-20.91878-10.91415-49.11366-6.36659c0,0-15.007,36.205,0,50.61758l4.0928,36.24085s38.19952,22.7378,50.93269-3.63805l-1.819-30.01391s9.09512-14.39464,1.819-24.93281A34.82081,34.82081,0,0,1,871.07293,420.14509Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#15803d"
        />
        <path
          d="M873.72307,584.98666l3.67163-76.188-10.39135-90.63379.47705-.07227c13.63086-2.05517,18.00268,21.42774,18.18213,22.42822l23.72583,134.14209Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#3f3d56"
        />
        <path
          d="M827.46624,585.78061l-41.94336-4.66016.011-.45849c.14869-6.20948,3.75879-152.2041,13.87452-162.54395,10.17553-10.40137,29.37719-5.90088,30.18969-5.70312l.40479.09765-3.45142,61.77637Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#3f3d56"
        />
        <path
          d="M835.19144,498.52321a9.14584,9.14584,0,0,1-11.90719-7.409l-31.89489-6.24893,11.83539-12.04883,28.47343,7.7813a9.19542,9.19542,0,0,1,3.49326,17.92547Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#ffb8b8"
        />
        <path
          d="M818.17132,491.20932,807.77874,488.975c-15.34229,4.61425-42.12219-11.62989-52.94593-18.84522-2.03662-1.35742-2.69726-4.04736-1.9635-7.99365a23.30978,23.30978,0,0,1,8.567-13.915l38.11975-30.2251,9.62158-1.60889,2.85791,16.19483L787.74822,458.6683l35.75244,19.46Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#3f3d56"
        />
        <path
          d="M814.06094,400.06826v-21a33.5,33.5,0,1,1,67,0v21a4.50508,4.50508,0,0,1-4.5,4.5h-58A4.50507,4.50507,0,0,1,814.06094,400.06826Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#2f2e41"
        />
        <circle cx="679.37983" cy="232.67051" r="24.56103" fill="#ffb8b8" />
        <path
          d="M820.97939,379.706a2.50023,2.50023,0,0,1-.5852-1.99317l2.90942-20.25976a2.50362,2.50362,0,0,1,1.41455-1.91895c14.85034-6.95019,29.90967-6.959,44.76-.02637a2.51921,2.51921,0,0,1,1.42871,2.03614L872.849,377.83a2.49954,2.49954,0,0,1-2.48877,2.73828h-4.92553a2.50966,2.50966,0,0,1-2.26539-1.44238l-2.12573-4.55469a1.49989,1.49989,0,0,0-2.84765.44824l-.41993,3.3584a2.50359,2.50359,0,0,1-2.48071,2.19043H822.8688A2.50013,2.50013,0,0,1,820.97939,379.706Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#2f2e41"
        />
        <rect y="40.3663" width="551" height="343.11356" fill="#e6e6e6" />
        <rect
          x="36.94894"
          y="106.46612"
          width="100.91574"
          height="65.59524"
          fill="#fff"
        />
        <rect
          x="162.34439"
          y="106.46612"
          width="100.91574"
          height="65.59524"
          fill="#fff"
        />
        <rect
          x="287.73987"
          y="106.46612"
          width="100.91577"
          height="65.59524"
          fill="#fff"
        />
        <rect
          x="413.13531"
          y="106.46612"
          width="100.91577"
          height="65.59524"
          fill="#fff"
        />
        <rect
          x="36.94894"
          y="199.30861"
          width="100.91574"
          height="65.59523"
          fill="#fff"
        />
        <rect
          x="162.34439"
          y="199.30861"
          width="100.91574"
          height="65.59523"
          fill="#fff"
        />
        <rect
          x="287.73987"
          y="199.30861"
          width="100.91577"
          height="65.59523"
          fill="#fff"
        />
        <rect
          x="413.13531"
          y="199.30861"
          width="100.91577"
          height="65.59523"
          fill="#fff"
        />
        <rect
          x="36.94894"
          y="292.15109"
          width="100.91574"
          height="65.59525"
          fill="#fff"
        />
        <rect
          x="162.34439"
          y="292.15109"
          width="100.91574"
          height="65.59525"
          fill="#fff"
        />
        <rect
          x="287.73987"
          y="292.15109"
          width="100.91577"
          height="65.59525"
          fill="#fff"
        />
        <rect
          x="413.13531"
          y="292.15109"
          width="100.91577"
          height="65.59525"
          fill="#fff"
        />
        <circle cx="144.30951" cy="65.59524" r="15.13736" fill="#fff" />
        <circle cx="394.58057" cy="65.59524" r="15.13734" fill="#fff" />
        <rect x="138.25458" width="12.10989" height="69.63187" fill="#e6e6e6" />
        <rect x="388.52563" width="12.10986" height="69.63187" fill="#e6e6e6" />
        <path
          d="M653.24859,286.39377a22.75657,22.75657,0,1,1-3.52686-12.19043A22.7566,22.7566,0,0,1,653.24859,286.39377Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#15803d"
        />
        <path
          d="M649.72173,274.20337l-22.46387,22.45975c-1.41186-2.18579-9.27765-12.34394-9.27765-12.34394A31.82748,31.82748,0,0,1,621.2,281.477l6.52259,8.69678,19.36853-19.36856A22.72652,22.72652,0,0,1,649.72173,274.20337Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#fff"
        />
        <path
          d="M402.45763,379.23627a22.75655,22.75655,0,1,1-3.52685-12.19043A22.7566,22.7566,0,0,1,402.45763,379.23627Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#15803d"
        />
        <path
          d="M398.93078,367.04584l-22.46387,22.45974c-1.41187-2.18579-9.27765-12.34393-9.27765-12.34393a31.82748,31.82748,0,0,1,3.21979-2.84222l6.52258,8.69677,19.36859-19.36856A22.72613,22.72613,0,0,1,398.93078,367.04584Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#fff"
        />
        <path
          d="M277.06218,379.23627a22.75667,22.75667,0,1,1-3.52685-12.19043A22.75667,22.75667,0,0,1,277.06218,379.23627Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#15803d"
        />
        <path
          d="M273.53533,367.04584l-22.46387,22.45974c-1.41186-2.18579-9.27765-12.34393-9.27765-12.34393a31.82748,31.82748,0,0,1,3.21979-2.84222l6.52258,8.69677,19.3686-19.36856A22.72673,22.72673,0,0,1,273.53533,367.04584Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#fff"
        />
        <path
          d="M1032.16535,752.87h-381a1,1,0,0,1,0-2h381a1,1,0,0,1,0,2Z"
          transform="translate(-166.83465 -147.13004)"
          fill="#3f3d56"
        />
      </svg>
    </div>

    <div
      v-if="openModal"
      class="h-1/2 self-center align-center bg-white rounded-lg px-4 pt-5 pb-4 mr-4 text-left overflow-hidden shadow-xl transition transform z-2 duration-500 delay-500 sm:my-8 sm:align-middle sm:max-w-lg sm:w-full popup"
      role="dialog"
      aria-modal="true"
      aria-labelledby="modal-headline"
    >
      <div class="flex justify-between m-4">
        <div class="text-2xl font-medium text-gray-700">Add a new event</div>

        <svg
          class="h-8 w-8 text-red-600 p-1 rounded-full bg-red-100 cursor-pointer"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          aria-hidden="true"
          @click="(openModal = false), (libelle = '')"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M6 18L18 6M6 6l12 12"
          />
        </svg>
      </div>
      <div class="m-4 text-orange-800 font-medium text-xl">
        {{ checkedDay }}
      </div>

      <form
        class="max-w-md mx-auto"
        id="my-form"
        @submit.prevent="!update ? submitEvent() : updateEvent()"
      >
        <div class="relative z-0 w-full mb-5 group">
          <input
            type="text"
            name="text"
            id="text"
            class="block py-2.5 px-0 w-full text-sm text-gray-900 bg-transparent border-0 border-b-2 border-gray-500 appearance-none focus:outline-none focus:ring-0 focus:border-green-600 peer"
            placeholder=""
            v-model="libelle"
            required
          />
          <label
            for="text"
            class="peer-focus:font-medium absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-6 scale-75 top-3 -z-10 origin-[0] peer-focus:start-0 rtl:peer-focus:translate-x-1/4 rtl:peer-focus:left-auto peer-focus:text-green-600 peer-focus:dark:text-green-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-6"
          >
            New event</label
          >
        </div>

        <button
          v-if="!update"
          type="submit"
          class="text-white bg-green-700 hover:bg-green-800 focus:ring-4 focus:outline-none focus:ring-green-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center hover:scale-110 transition-all duration-700"
        >
          Submit
        </button>

        <button
          v-else
          type="submit"
          class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center"
        >
          Update
        </button>
      </form>
    </div>
  </div>

  <div class="container mx-auto mt-5">
    <div class="wrapper bg-white rounded shadow w-full">
      <div class="header flex justify-between border-b p-2">
        <span class="text-3xl ml-[15%] mt-5 font-bold"> Calendar App </span>

        <button
          @click="(openModal = true), (update = false)"
          class="p-1 my-5 mx-8 flex justify-center items-center text-white bg-green-500 rounded-md cursor-pointer focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 hover:shadow-md shadow-lg transition-all duration-700 hover:scale-110"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="white"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="white"
            class="size-10"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M12 4.5v15m7.5-7.5h-15"
            />
          </svg>
          <span class="text-lg font-bold mr-2">New Event</span>
        </button>
      </div>
      <table class="w-full mb-10">
        <thead>
          <tr>
            <th
              class="cursor-pointer p-3 border-r h-10 xl:w-40 lg:w-30 md:w-30 sm:w-20 w-10 xl:text-xl text-xs"
              v-for="(even, index) in calendar"
              :key="index"
              @click="checkedDay = even.day"
            >
              <span class="xl:block lg:block md:block sm:hidden hidden">{{
                even.day
              }}</span>
              <span class="xl:hidden lg:hidden md:hidden sm:block block">{{
                even.day.slice(0, 3)
              }}</span>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr class="text-center z-0">
            <td
              class="border xl:w-40 lg:w-30 md:w-30 sm:w-20 w-10 overflow-auto transition cursor-pointer duration-500 ease-in-out"
              v-for="(even, index) in calendar"
              :key="index"
              @click="checkedDay = even.day"
              :class="[
                index % 2 !== 0 ? 'hover:bg-orange-50' : 'hover:bg-green-50',
              ]"
              @dblclick="openModal = true"
            >
              <div
                class="flex flex-col mx-auto snap-y xl:w-5/6 max-h-[331px] lg:w-30 md:w-30 sm:w-full w-10 mx-auto"
              >
                <!-- <div class="top h-5 w-full">
                  <span class="text-gray-500">{{ index + 1 }}</span>
                </div> -->

                <div class="bottom snap-center flex-grw h-30 py-1 w-full cursor-pointer">
                  <div
                    :class="[
                      'max-w-md mx-auto border rounded-xl shadow-md overflow-hidden mx-1 my-3 hover:transition-all ease-in-out delay-200 duration-700 hover:scale-110',
                      pIndex % 2 === 0
                        ? 'bg-green-50 border-green-400'
                        : 'bg-orange-50 border-orange-400',
                    ]"
                    v-for="(plan, pIndex) in even.event"
                    :key="pIndex"
                  >
                    <div
                      :class="[
                        'border rounded-xl m-0.5',
                        pIndex % 2 === 0
                          ? 'border-green-500'
                          : 'border-orange-500',
                      ]"
                    >
                      <div class="md:flex">
                        <div class="p-2 w-full">
                          <p
                            class="py-2 text-sm leading-tight font-medium text-black"
                          >
                            {{ plan.libelle }}
                          </p>

                          <div class="flex justify-between mx-2">
                            <svg
                              xmlns="http://www.w3.org/2000/svg"
                              fill="none"
                              viewBox="0 0 24 24"
                              stroke-width="1.5"
                              stroke="currentColor"
                              class="size-5 border border-transparent rounded-md hover:text-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500"
                              @click="editEvent(index, pIndex)"
                            >
                              <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L6.832 19.82a4.5 4.5 0 0 1-1.897 1.13l-2.685.8.8-2.685a4.5 4.5 0 0 1 1.13-1.897L16.863 4.487Zm0 0L19.5 7.125"
                              />
                            </svg>

                            <svg
                              xmlns="http://www.w3.org/2000/svg"
                              fill="none"
                              viewBox="0 0 24 24"
                              stroke-width="1.5"
                              stroke="currentColor"
                              class="size-5 border border-transparent rounded-md hover:text-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500"
                              @click="deleteEvent(index, pIndex)"
                            >
                              <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0"
                              />
                            </svg>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style>
body {
  background-color: #edf2f7;
}
</style>
