<template>
  <v-layout>
    <v-main>
      <Header />
      <v-form ref="form">
        <v-container class="d-flex flex-column px-0">
          <div class="d-flex justify-space-between">
            <v-select
              v-model="departureCity"
              :items="allTickets.map((ticket) => ticket.departureCity)"
              label="Вылет"
              class="mr-10"
            ></v-select>
            <v-select
              v-model="arrivalCity"
              :items="allTickets.map((ticket) => ticket.arrivalCity)"
              label="Прилет"
              class="ml-10"
            ></v-select>
          </div>
          <v-date-picker v-model="dates" range></v-date-picker>
          <v-range-slider
            v-model="range"
            :max="100000"
            :min="1000"
            hide-details
            class="align-center"
          >
            <template v-slot:prepend>
              <v-text-field
                :value="range[0]"
                class="mt-0 pt-0"
                hide-details
                single-line
                type="number"
                style="width: 60px"
                @change="$set(range, 0, $event)"
              ></v-text-field>
            </template>
            <template v-slot:append>
              <v-text-field
                :value="range[1]"
                class="mt-0 pt-0"
                hide-details
                single-line
                type="number"
                style="width: 60px"
                @change="$set(range, 1, $event)"
              ></v-text-field>
            </template>
          </v-range-slider>
          <v-btn color="success" class="mr-4" @click="submit">
            Применить
          </v-btn>
        </v-container>
      </v-form>

      <v-container fluid>
        <v-row dense>
          <v-col v-for="ticket in filtredTickets" :key="ticket" :cols="2">
            <v-card class="mx-auto" variant="outlined">
              <v-card-header>
                <div>
                  <div class="text-h6 mb-1">
                    {{ ticket.departureCity }} - {{ ticket.arrivalCity }}
                  </div>
                  <div class="text-h7 mb-1">
                    Вылет
                    {{
                      format(new Date(ticket.departureDateTime), "dd.MM.yyyy")
                    }}
                  </div>
                  <div class="text-h7 mb-1">
                    Прилет
                    {{ format(new Date(ticket.arrivaDateTime), "dd.MM.yyyy") }}
                  </div>
                  <div class="text-caption">Цена: {{ ticket.cost }}</div>
                </div>
              </v-card-header>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
      <Footer />
    </v-main>
  </v-layout>
</template>

<script>
import Header from "./Header";
import Footer from "./Footer";
import isWithinInterval from "date-fns/isWithinInterval";
import format from "date-fns/format";
export default {
  name: "v-Nav",

  components: {
    Header,
    Footer,
  },

  data: () => ({
    

    departureCity: null,
    arrivalCity: null,
    dates: null,
    format,
    allTickets: [
      {
        departureCity: "Москва",
        arrivalCity: "Дубай",
        departureDateTime: "2022-02-27",
        arrivaDateTime: "2022-02-27",
        cost: 21000,
      },
      {
        departureCity: "Москва",
        arrivalCity: "Таганрог",
        departureDateTime: "2022-03-01",
        arrivaDateTime: "2022-03-01",
        cost: 12000,
      },
      {
        departureCity: "Дубай",
        arrivalCity: "Москва",
        departureDateTime: "2022-02-28",
        arrivaDateTime: "2022-02-28",
        cost: 25000,
      },
      {
        departureCity: "Ростов",
        arrivalCity: "Москва",
        departureDateTime: "2022-03-02",
        arrivaDateTime: "2022-03-02",
        cost: 2000,
      },
      {
        departureCity: "Минск",
        arrivalCity: "Москва",
        departureDateTime: "2022-03-03",
        arrivaDateTime: "2022-03-03",
        cost: 7000,
      },
      {
        departureCity: "Москва",
        arrivalCity: "Минск",
        departureDateTime: "2022-03-05",
        arrivaDateTime: "2022-03-05",
        cost: 6000,
      },
      {
        departureCity: "Москва",
        arrivalCity: "Киев",
        departureDateTime: "2022-03-05",
        arrivaDateTime: "2022-03-05",
        cost: 60000,
      },
      {
        departureCity: "Москва",
        arrivalCity: "Польша",
        departureDateTime: "2022-03-06",
        arrivaDateTime: "2022-03-06",
        cost: 20000,
      },
      {
        departureCity: "Польша",
        arrivalCity: "Москва",
        departureDateTime: "2022-03-06",
        arrivaDateTime: "2022-03-06",
        cost: 22000,
      },
    ],
    filtredTickets: [],
    range: [0, 100000],
  }),
  methods: {
    submit() {
      const result = this.allTickets.filter((ticket) => {
        if (this.departureCity && ticket.departureCity !== this.departureCity) {
          return false;
        }
        if (this.arrivalCity && ticket.arrivalCity !== this.arrivalCity) {
          return false;
        }
        if (
          this.dates &&
          !isWithinInterval(new Date(ticket.departureDateTime), {
            start: new Date(this.dates[0]),
            end: new Date(this.dates[1]),
          })
        ) {
          return false;
        }
        if (ticket.cost < this.range[0] || ticket.cost > this.range[1]) {
          return false;
        }

        return true;
      });
      this.filtredTickets = result;
    },
  },
};
</script>
<style scoped>
</style>
