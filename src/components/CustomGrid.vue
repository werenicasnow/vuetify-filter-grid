<template>
  <v-data-table
    :items="filterDesserts"
    item-key="id"
    :headers-length="headers.length"
  >
    <template v-slot:header>
      <thead class="v-data-table-header">
        <tr>
          <th
            v-for="header in headers"
            :class="[
              'text-start sortable',
              sortDescending ? 'desc' : 'asc',
              {active: header.value === pagination.sortBy || filtersEnabled.includes(header.value)},
            ]"
          >
            <v-layout>
              <v-flex>
                <v-icon small class="v-data-table-header__icon">
                  {{ sortDescending(header) ? 'arrow_upward' : 'arrow_downward' }}
                </v-icon>
                <span @click="changeSort(header.value)">{{header.text}}</span>
              </v-flex>
              <v-flex>
                <v-menu :close-on-content-click="false" absolute offset-x offset-y>
                  <template v-slot:activator="{ on }">
                    <v-btn icon v-on="on">
                      <v-icon small>arrow_drop_down</v-icon>
                    </v-btn>
                  </template>

                  <v-list dense>
                    <v-menu offset-x open-on-click :close-on-content-click="false">
                      <template v-slot:activator="{ on }">
                        <v-list-item v-on="on">
                          <v-list-item-action>
                            <v-checkbox
                              @change="enableFilter(header.value, $event)"
                              v-model="filtersEnabled"
                              :value="header.value"
                            ></v-checkbox>
                          </v-list-item-action>

                          <v-list-item-content>
                            <v-list-item-title>
                              <v-icon>filter_alt</v-icon>Filter
                            </v-list-item-title>
                          </v-list-item-content>

                          <v-list-item-action class="justify-end">
                            <v-icon>play_arrow</v-icon>
                          </v-list-item-action>
                        </v-list-item>
                      </template>
                      <v-list dense class="number__list">
                        <div v-if="header.type === 'number'">
                          <v-list-item>
                            <v-text-field
                              placeholder="Equally"
                              @input="changeFilter(header, $event, 'eq')"
                              type="number"
                              hide-details
                            ></v-text-field>
                          </v-list-item>
                          <v-list-item>
                            <v-text-field
                              placeholder="More"
                              @input="changeFilter(header, $event, 'gt')"
                              type="number"
                              hide-details
                            ></v-text-field>
                          </v-list-item>
                          <v-list-item>
                            <v-text-field
                              placeholder="Less"
                              @input="changeFilter(header, $event, 'lt')"
                              type="number"
                              hide-details
                            ></v-text-field>
                          </v-list-item>
                        </div>

                        <v-list-item v-else>
                          <v-text-field
                            placeholder="Contains"
                            @keyup="changeFilter(header, $event, 'like')"
                          ></v-text-field>
                        </v-list-item>
                      </v-list>
                    </v-menu>

                    <v-list-item @click="changeSort(header.value, false)">
                      <v-icon>sort_by_alpha</v-icon>
                      <v-icon>arrow_upward</v-icon>
                      <v-list-item-title>
                        Sort ascending
                      </v-list-item-title>
                    </v-list-item>

                    <v-list-item @click="changeSort(header.value, true)">
                      <v-icon>sort_by_alpha</v-icon>
                      <v-icon>arrow_downward</v-icon>
                      <v-list-item-title>
                        Sort descending
                      </v-list-item-title>
                    </v-list-item>
                  </v-list>
                </v-menu>
              </v-flex>
            </v-layout>
          </th>
        </tr>
      </thead>
    </template>

    <template v-slot:item="{ item }">
      <tr>
        <td v-for="header in headers">
          {{item[header.value]}}
        </td>
      </tr>
    </template>
  </v-data-table>
</template>

<script>
  import _ from 'lodash';
  export default {
    data () {
      return {
        headers: [
          {text: 'Dessert (100g serving)', value: 'name', type: 'text'},
          {text: 'Calories', value: 'calories', type: 'number'},
          {text: 'Fat (g)', value: 'fat', type: 'number'},
          {text: 'Carbs (g)', value: 'carbs', type: 'number'},
          {text: 'Protein (g)', value: 'protein', type: 'number'},
          {text: 'Iron (%)', value: 'iron', type: 'text'}
        ],
        desserts: [
          {
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: '1%'
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: '1%'
          },
          {
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            iron: '7%'
          },
          {
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
            iron: '8%'
          },
          {
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
            iron: '16%'
          },
          {
            name: 'Jelly bean',
            calories: 375,
            fat: 0.0,
            carbs: 94,
            protein: 0.0,
            iron: '0%'
          },
          {
            name: 'Lollipop',
            calories: 392,
            fat: 0.2,
            carbs: 98,
            protein: 0,
            iron: '2%'
          },
          {
            name: 'Honeycomb',
            calories: 408,
            fat: 3.2,
            carbs: 87,
            protein: 6.5,
            iron: '45%'
          },
          {
            name: 'Donut',
            calories: 452,
            fat: 25.0,
            carbs: 51,
            protein: 4.9,
            iron: '22%'
          },
          {
            name: 'KitKat',
            calories: 518,
            fat: 26.0,
            carbs: 65,
            protein: 7,
            iron: '6%'
          }
        ],
        filterDesserts: [],
        pagination: {
          sortBy: 'id',
          descending: false,
          page: 1,
          rowsPerPage: 10
        },
        filters: {},
        filtersEnabled: [],
      }
    },

    computed: {
      sortDescending () {
        return header => {
          if (header.value === this.pagination.sortBy) {
            return this.pagination.descending
          } else {
            return false
          }
        }
      }
    },

    mounted() {
      this.filterDesserts = this.desserts
    },

    methods: {
      changeSort (column, sort) {
        if (this.pagination.sortBy === column) {
          this.pagination.descending = sort !== undefined ? sort : !this.pagination.descending
        } else {
          this.pagination.sortBy = column;
          this.pagination.descending = sort !== undefined ? sort : false
        }
        this.desserts.sort((a, b) => (this.pagination.descending)
          ? (a[this.pagination.sortBy] < b[this.pagination.sortBy]) ? 1 : -1
          : (a[this.pagination.sortBy] > b[this.pagination.sortBy]) ? 1 : -1
        )
      },

      changeFilters (filters) {
        this.filterDesserts = Object.assign(this.desserts);
        Object.entries(filters).forEach(([key, filter]) => {
          if (filter.enabled && filter.value) {
            const column = filter.column,
              value = (filter.type === 'number') ? parseInt(filter.value) : filter.value.toLowerCase();
            switch (filter.operator) {
              case 'like':
                this.filterDesserts = this.filterDesserts.filter(d => d[column].toLowerCase().includes(value));
                break;
              case 'eq':
                this.filterDesserts = this.filterDesserts.filter(d => d[column] === value);
                break;
              case 'gt':
                this.filterDesserts = this.filterDesserts.filter(d => d[column] > value);
                break;
              case 'lt':
                this.filterDesserts = this.filterDesserts.filter(d => d[column] < value);
                break;
              default:
                break;
            }
          }
        });
      },

      enableFilter(column, event) {
        let filtersKeys = Object.keys(this.filters);
        filtersKeys.forEach(key => {
          this.filters[key].enabled = event.includes(key.substring(0, key.lastIndexOf('_')))
        });
        this.changeFilters(this.filters);
      },

      changeFilter: _.debounce(function(header, event, operator) {
        let column = header.value,
          value = (event.target) ? event.target.value : event;
        if (!this.filtersEnabled.includes(column)) {
          this.filtersEnabled.push(column);
        }

        this.filters[column + "_" + operator] = {
          'column': column, 'operator': operator, 'value': value, 'type': header.type, 'enabled': (value !== '')
        };
        this.changeFilters(this.filters);
      }, 500),
    }
  }
</script>

<style scoped>

</style>
