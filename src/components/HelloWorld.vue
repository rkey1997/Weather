<template>
  <div class="container">
    <v-card width="500">
      <v-card-title dark class="headline black">
        <div class="white--text">Weather for you</div>
        <v-spacer></v-spacer>
        <v-text-field
          name="name"
          v-model="city"
          label="serach here"
          append-icon="mdi-magnify"
          dark
          @keyup="check"
        ></v-text-field>
      </v-card-title>

      <v-card-text class="pa-4">
        <div v-if="content">
          <div class="information">
            <div class="content__title">
              <h1>{{ Location.name }}</h1>
            </div>
            <div>
              <h1>{{ Temp.temp_c }}&deg</h1>
            </div>
            <div>
              <img :src="Condition.icon" />
            </div>
          </div>
          <div class="content">
            <div class="content__item">
              <div class="mt-3 content__info">
                <span class="content__title">Country:</span>
                <span>{{ Location.country }}</span>
              </div>
              <div class="mt-3 content__info">
                <span class="content__title">Region:</span>
                <span>{{ Location.region }}</span>
              </div>
              <div class="mt-3 content__info">
                <span class="content__title">Local time:</span>
                <span>{{ Location.localtime }}</span>
              </div>

              <div class="mt-3 content__info">
                <span class="content__title">latitude:</span>
                <span>{{ Location.lat }}</span>
              </div>
              <div class="mt-3 content__info">
                <span class="content__title">longitude:</span>
                <span>{{ Location.lon }}</span>
              </div>
            </div>
            <div class="content__item pl-2">
              <div class="mt-3 content__info">
                <span class="content__title"> feels like:</span>
                <span>{{ Temp.temp_c }}&deg</span>
              </div>
              <div class="mt-3 content__info">
                <span class="content__title">Description:</span>
                <span>{{ Condition.text }}</span>
              </div>
              <div class="mt-3 content__info">
                <span class="content__title">wind direction:</span>
                <span>{{ Temp.wind_dir }}</span>
              </div>
              <div class="mt-3 content__info">
                <span class="content__title">gust kp/h:</span>
                <span>{{ Temp.gust_kph }}</span>
              </div>
              <div class="mt-3 content__info">
                <span class="content__title">gust mp/h:</span>
                <span>{{ Temp.gust_mph }}</span>
              </div>
              <div class="mt-3 content__info">
                <span class="content__title">wind kp/h:</span>
                <span>{{ Temp.wind_kph }}</span>
              </div>
              <div class="mt-3 content__info">
                <span class="content__title">wind mp/h:</span>
                <span>{{ Temp.wind_mph }}</span>
              </div>
              <div class="mt-3 content__info">
                <span class="content__title">humidity:</span>
                <span>{{ Temp.humidity }}</span>
              </div>
            </div>
          </div>
        </div>
        <div class="enter" v-if="info">
          <v-card outlined class="pa-3 yellow lighten-2 red--text" width="300">
            <span>Please enter 3 first letters of the city</span>
            <span class="ml-3"><v-icon  color="red">mdi-arrow-up</v-icon></span>
          </v-card>
        </div>
      </v-card-text>
      <v-footer dark color="black">
        <v-spacer></v-spacer>
        <span>RKEY 2020&copy</span>
        <v-spacer></v-spacer>
      </v-footer>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      Location: {},
      Condition: {},
      Temp: {},
      city: "",
      content: false,
      info: true,
    };
  },
  methods: {
    check() {
      const capitalizeFirstLetter = (string) => {
        return string.charAt(0).toUpperCase() + string.slice(1);
      };

      if (this.city.length < 3) {
        this.content = false;
        this.info = true;
        return false;
      }
      if (this.city.length > 2) {
        this.content = true;
        this.info = false;
      }

      this.axios
        .get(
          "https://api.weatherapi.com/v1/current.json?key=ef2e910952854482b64110233201910",
          {
            params: {
              q: capitalizeFirstLetter(this.city),
            },
          }
        )
        .then((response) => {
          console.log(response.data);
          this.Location = response.data.location;
          this.Condition = response.data.current.condition;
          this.Temp = response.data.current;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
  },
};
</script>
<style scoped>
.container,
.enter {
  display: flex;
  justify-content: center;
}

.information {
  display: flex;
  justify-content: space-between;
  border-bottom: 2px solid #333;
}
.content {
  display: flex;
  justify-content: space-between;
}

.content__item {
  width: 50%;
}
.content__item:last-child {
  border-left: 2px solid #333;
}

.content__info {
  display: flex;
  font-size: 20px;
}
.content__title {
  color: black;
}
</style>
