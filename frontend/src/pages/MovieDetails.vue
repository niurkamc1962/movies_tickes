<template>
  <div>
    <h1 class="text-gray-900 font-bold text-[32px]">Oppenheimer</h1>
    <div class="mt-11 flex flex-row items-center justify-between">
      <div class="flex flex-col space-y-3">
        <h2 class="text-gray-700 text-base font-bold uppercase">Director</h2>
        <p class="text-gray-600 text-xl font-semibold">Christopher Noilan</p>
      </div>
      <div class="flex flex-col space-y-3">
        <h2 class="text-gray-700 text-base font-bold uppercase">
          Release Date
        </h2>
        <p class="text-gray-600 text-xl font-semibold">21st, July 2023</p>
      </div>
    </div>

    <div class="max-w-full">
      <div class="mx-12" v-if="currentStep === 0">
        <div class="p-2 mt-7 bg-white shadow-2xl">
          <img src="/frontend/public/oppenheimer.jpeg" alt="Movie Poster" />
        </div>

        <div class="w-full flex items-center justify-between mt-7">
          <Button size="xl" variant="solid" @click="currentStep++"
            >Book Ticket</Button
          >
        </div>

        <div class="flex flex-col space-y-3 mt-16">
          <h2 class="text-gray-700 text-base font-bold uppercase">Synopsis</h2>
          <p class="text-gray-600 text-lg font-normal">
            Lorem ipsum, dolor sit amet consectetur adipisicing elit. Quaerat,
            excepturi. Unde ex ab sunt pariatur nemo! Modi sequi recusandae,
            repudiandae atque fuga a, adipisci quisquam vitae possimus quasi
            obcaecati reiciendis.
          </p>
        </div>
      </div>

      <div v-else-if="currentStep === 1">
        <h2 class="font-medium text-xl mt-7 text-gray-900">How many seats?</h2>
        <div class="flex flex-col w-full space-y-5 mt-6">
          <Button
            size="lg"
            :variant="index === bookingData.numberOfSeats ? 'subtle' : 'white'"
            class="shadow-lg"
            v-for="index in 8"
            @click="setNumberOfSeats(index)"
            >{{ index }}</Button
          >
        </div>
      </div>

      <div v-else-if="currentStep === 2">
        <div class="flex flex-col space-y-4">
          <h2 class="font-medium text-xl mt-7 text-gray-900">Date</h2>
          <input type="date" v-model="bookingData.date" />
        </div>

        <div class="flex flex-col space-y-4">
          <h2 class="font-medium text-xl mt-7 text-gray-900">Cinema & Show</h2>

          <div>
            <div class="bg-white shadow-xl p-4 rounded">
              <h3 class="text-sm font-bold text-gray-800">Star Talkies</h3>
              <div>
                <Button size="sm" variant="outline">12:30 PM</Button>
                <Button size="sm" variant="outline">03:30 PM</Button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div v-else-if="currentStep === 3">
        <h2 class="font-medium text-xl mt-7 text-gray-900">Select Seats</h2>

        <div>
          <div
            :key="row"
            class="flex flex-row"
            v-for="row in Object.keys(seatStructure)"
          >
            <span
              @click="selectSeat(row, seat[0])"
              v-for="seat in seatStructure[row]"
              class="w-6 h-8 m-2 rounded-[2px]"
              :class="
                seat[1] === 'Available'
                  ? 'bg-blue-300'
                  : seat[1] === 'Selected'
                  ? 'bg-blue-600'
                  : 'bg-gray-300'
              "
            ></span>
          </div>
        </div>
      </div>

      <div v-else-if="currentStep === 4">
        <div class="w-full flex items-center flex-col mt-7">
          <h1 class="text-[110px]">🍿</h1>
          <h2 class="font-medium text-xl mt-7 text-gray-900">
            Enjoy the movie!
          </h2>
        </div>
      </div>
    </div>

    <div class="flex flex-row mt-6 space-x-2">
      <Button
        size="lg"
        variant="subtle"
        v-if="currentStep !== 0 && currentStep != 4"
        @click="currentStep--"
        >Go Back</Button
      >

      <Button
        v-if="currentStep !== 0 && currentStep != 4"
        size="lg"
        variant="solid"
        @click="currentStep++"
        >Next</Button
      >
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'

function getSeatStructure(alphabets, numbers) {
  const structure = {}
  for (const alphabet of alphabets) {
    structure[alphabet] = []
    for (const number of numbers) {
      structure[alphabet].push([number, 'Available'])
    }
  }
  return structure
}

// const seatStructure = {
//   A: [
//     [1, 'Booked'],
//     [2, 'Available'],
//     [3, 'Available'],
//     [4, 'Available'],
//     [5, 'Available'],
//     [6, 'Available'],
//     [7, 'Available'],
//   ],
//   B: [
//     [1, 'Available'],
//     [2, 'Available'],
//     [3, 'Available'],
//     [4, 'Available'],
//     [5, 'Available'],
//     [6, 'Available'],
//     [7, 'Available'],
//   ],
//   C: [
//     [1, 'Available'],
//     [2, 'Available'],
//     [3, 'Available'],
//     [4, 'Available'],
//     [5, 'Available'],
//     [6, 'Available'],
//     [7, 'Available'],
//   ],
//   D: [
//     [1, 'Available'],
//     [2, 'Available'],
//     [3, 'Available'],
//     [4, 'Available'],
//     [5, 'Available'],
//     [6, 'Available'],
//     [7, 'Available'],
//   ],
//   E: [
//     [1, 'Available'],
//     [2, 'Available'],
//     [3, 'Available'],
//     [4, 'Available'],
//     [5, 'Available'],
//     [6, 'Available'],
//     [7, 'Available'],
//   ],
// }

// data structure to store seats
// {
//    id: 1,
//    row: 1,
//    column: 1,
//    status: 'available' | 'booked'| 'selected'
// }

// console.log(
//   getSeatStructure(['A', 'B', 'C', 'D', 'E'], [1, 2, 3, 4, 5, 6, 7, 8])
// )

const seatStructure = reactive(
  getSeatStructure(['A', 'B', 'C', 'D', 'E'], [1, 2, 3, 4, 5, 6, 7])
)

const today = new Date().toISOString().substr(0, 10)
const currentStep = ref(3)
const bookingData = reactive({
  numberOfSeats: 0,
  selectedSeats: [],
  date: today,
})

function setNumberOfSeats(n) {
  bookingData.numberOfSeats = n
}

function selectSeat(row, number) {
  // mark the seat booked in structure
  const seat = seatStructure[row].find((seat) => seat[0] === number)
  seat[1] = 'Selected'
  bookingData.selectedSeats.push('${row}${number}')
}
</script>
