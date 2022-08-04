<template>
  <div class="p-10 bg-white">
    <div class="h-screen">
      <div class="sm:flex sm:items-center">
        <div class="sm:flex-auto">
          <h1 class="text-xl font-semibold text-gray-900">Registered Users</h1>
        </div>
        <div class="mt-4 sm:mt-0 sm:ml-16 sm:flex-none">
          <button
            type="button"
            @click="$router.push('/create')"
            class="inline-flex items-center justify-center rounded-md border border-transparent bg-indigo-600 px-4 py-2 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 sm:w-auto"
          >
            Register User
          </button>
        </div>
      </div>
      <div class="mt-8 flex flex-col">
        <div class="-my-2 -mx-4 overflow-x-auto sm:-mx-6 lg:-mx-8">
          <div
            class="inline-block min-w-full py-2 align-middle md:px-6 lg:px-8"
          >
            <div
              class="overflow-hidden shadow ring-1 ring-black ring-opacity-5 md:rounded-lg"
            >
              <table class="min-w-full divide-y divide-gray-300">
                <thead class="bg-gray-50">
                  <tr>
                    <th
                      scope="col"
                      class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-6"
                    >
                      Name
                    </th>
                    <th
                      scope="col"
                      class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900"
                    >
                      Mobile
                    </th>
                    <th
                      scope="col"
                      class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900"
                    >
                      Email
                    </th>
                    <th
                      scope="col"
                      class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900"
                    >
                      Actions
                    </th>

                    <!-- <th scope="col" class="relative py-3.5 pl-3 pr-4 sm:pr-6">
                      <span class="sr-only">Edit</span>
                    </th> -->
                  </tr>
                </thead>
                <tbody class="divide-y divide-gray-200 bg-white">
                  <tr v-for="user in getAllUsers" :key="user.email">
                    <td
                      class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-6"
                    >
                      {{ user.name }}
                    </td>
                    <td
                      class="whitespace-nowrap px-3 py-4 text-sm text-gray-500"
                    >
                      {{ user.mobile }}
                    </td>
                    <td
                      class="whitespace-nowrap px-3 py-4 text-sm text-gray-500"
                    >
                      {{ user.email }}
                    </td>
                    <td
                      class="relative whitespace-nowrap py-4 pl-3 text-right text-sm font-medium sm:pr-6"
                    >
                      <div class="flex items-center justify-between">
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          class="h-6 w-6 text-blue-500 hover:text-blue-400"
                          fill="none"
                          viewBox="0 0 24 24"
                          stroke="currentColor"
                          stroke-width="2"
                          @click="$router.push(`/${user.id}`)"
                        >
                          <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
                          />
                          <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"
                          />
                        </svg>

                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          class="h-6 w-6 text-indigo-500 hover:text-indigo-400"
                          fill="none"
                          viewBox="0 0 24 24"
                          stroke="currentColor"
                          stroke-width="2"
                          @click="$router.push(`/${user.id}/edit`)"
                        >
                          <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"
                          />
                        </svg>

                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          class="h-6 w-6 text-red-500 hover:text-red-400"
                          fill="none"
                          viewBox="0 0 24 24"
                          stroke="currentColor"
                          stroke-width="2"
                          @click="removeUser(user.id)"
                        >
                          <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                          />
                        </svg>
                      </div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
      <div v-if="getAllUsers.length" class="mt-5">
        <div id="map" style="width: 100%; height: 500px;"></div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'
import axios from 'axios'
export default {
  data: () => ({}),

  computed: {
    ...mapGetters(['getAllUsers']),
  },

  mounted() {
    this.fetchAllUsers()
    if (this.getAllUsers) {
      axios.get('http://localhost:3000/users').then((res) => {
        if (res) {
          this.initMap(res.data)
        }
      })
    }
  },

  methods: {
    ...mapActions(['fetchAllUsers', 'deleteUser']),

    initMap(users) {
      mapboxgl.accessToken =
        'pk.eyJ1IjoiaGFzaWkiLCJhIjoiY2wwYWt4cjJ1MDF4YTNjbWptNzZwM2ZhZyJ9.9WUVDkgiL5lsqX95u0VAvA'

      const map = new mapboxgl.Map({
        container: 'map',
        style: 'mapbox://styles/mapbox/streets-v11',
        center: [-120.094292506456, 48.0250054263],
        zoom: 4,
      })
      let hoveredStateId = null

      var features = []

      for (let index = 0; index < users.length; index++) {
        const element = users[index]
        features.push({
          type: 'Feature',
          properties: {
            description: `
             <div class="flex justify-between space-x-4">
               <img src="${element.image}" class="w-10 h-10" />
               <div>
                 <strong> ${element.name}  </strong>
                 <p> ${element.address} </p>
               </div>
             </div>
           `,
            property: element,
          },
          geometry: {
            type: 'Point',
            coordinates: element.coordinates,
          },
        })
      }

      map.on('load', () => {
        map.addSource('places', {
          type: 'geojson',
          data: {
            type: 'FeatureCollection',
            features: features,
          },
        })

        // Add a layer showing the places.
        map.addLayer({
          id: 'places',
          type: 'circle',
          source: 'places',
          paint: {
            'circle-color': 'red',
            'circle-radius': 6,
            'circle-stroke-width': 2,
            'circle-stroke-color': '#ffffff',
          },
        })

        // Create a popup, but don't add it to the map yet.
        const popup = new mapboxgl.Popup({
          closeButton: false,
          closeOnClick: false,
        })

        map.on('mouseenter', 'places', (e) => {
          // Change the cursor style as a UI indicator.
          map.getCanvas().style.cursor = 'pointer'

          // Copy coordinates array.
          const coordinates = e.features[0].geometry.coordinates.slice()
          const description = e.features[0].properties.description

          // Ensure that if the map is zoomed out such that multiple
          // copies of the feature are visible, the popup appears
          // over the copy being pointed to.
          while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
            coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360
          }

          // Populate the popup and set its coordinates
          // based on the feature found.
          popup.setLngLat(coordinates).setHTML(description).addTo(map)
        })

        map.on('mouseleave', 'places', () => {
          map.getCanvas().style.cursor = ''
          popup.remove()
        })

        map.on('click', 'places', (e) => {
          this.show = true
          this.selectedProperty = JSON.parse(e.features[0].properties.property)
        })

        map.addSource('states', {
          type: 'geojson',
          data: 'https://docs.mapbox.com/mapbox-gl-js/assets/us_states.geojson',
        })

        // The feature-state dependent fill-opacity expression will render the hover effect
        // when a feature's hover state is set to true.

        map.addLayer({
          id: 'state-fills',
          type: 'fill',
          source: 'states',
          layout: {},

          paint: {
            'fill-color': 'rgba(0, 0, 0, 0.1)',
            'fill-opacity': [
              'case',
              ['boolean', ['feature-state', 'hover'], false],
              1,
              0.5,
            ],
          },
        })

        map.addLayer({
          id: 'state-borders',
          type: 'line',
          source: 'states',
          layout: {},
          paint: {
            'line-color': 'rgba(0, 0, 0, 0)',
            'line-width': 2,
          },
        })

        // When the user moves their mouse over the state-fill layer, we'll update the
        // feature state for the feature under the mouse.
        map.on('mousemove', 'state-fills', (e) => {
          if (e.features.length > 0) {
            if (hoveredStateId !== null) {
              map.setFeatureState(
                { source: 'states', id: hoveredStateId },
                { hover: false },
              )
            }
            hoveredStateId = e.features[0].id
            map.setFeatureState(
              { source: 'states', id: hoveredStateId },
              { hover: true },
            )
          }
        })

        // When the mouse leaves the state-fill layer, update the feature state of the
        // previously hovered feature.
        map.on('mouseleave', 'state-fills', () => {
          if (hoveredStateId !== null) {
            map.setFeatureState(
              { source: 'states', id: hoveredStateId },
              { hover: false },
            )
          }
          hoveredStateId = null
        })
      })
    },

    removeUser(id) {
      this.deleteUser(id)
        .then(() => {
          this.$toastr.s('User deleted successfully')
        })
        .catch((err) => {
          this.$toastr.e(err.response.data)
        })
    },
  },
}
</script>
