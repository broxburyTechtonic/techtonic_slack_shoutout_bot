<template>
  <v-card>
    <v-carousel
      cycle
      height="500"
      interval="6000"
      :show-arrows="false"
      v-model="model"
    >
      <v-carousel-item v-for="(shout, i) in shouts" :key="i">
        <v-sheet height="100%" :style="bgImgStyle(i)" tile>
          <v-row
            align="center"
            class="carousel__company-value"
            justify="center"
          >
            {{ shout.company_value }}
          </v-row>
          <v-row
            align="center"
            class="carousel__shoutout-message"
            justify="center"
            :style="{ fontSize: shout.fontSize + 'rem' }"
          >
            {{ shout.message }}
          </v-row>
          <v-row align="end" justify="center">
            <div class="display-1 carousel__shoutout-user">
              <span class="carousel__shoutout-identifier">
                Shoutee:
              </span>
              {{ capitalizeName(shout.shoutee) }}
            </div>
            <div class="display-1 carousel__shoutout-user">
              <span class="carousel__shoutout-identifier">
                Shouter:
              </span>
              {{ capitalizeName(shout.shouter) }}
            </div>
          </v-row>
        </v-sheet>
      </v-carousel-item>
    </v-carousel>
    <v-list dark>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title
            >Above are the 5 most recent shoutouts, don't forget to check back
            for new ones!</v-list-item-title
          >
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-card>
</template>

<script>
export default {
  name: 'Carousel',
  props: ['recentShouts'],
  data() {
    return {
      model: 0,
      shouts: [
        {
          company_value: '',
          message: '',
          shoutee: ' ', // NEEDS TO HAVE AN EMPTY SPACE IN ORDER TO RENDER SKELETON LOADER
          shouter: ' ', // NEEDS TO HAVE AN EMPTY SPACE IN ORDER TO RENDER SKELETON LOADER
        },
      ],
    };
  },
  mounted() {
    this.shouts = this.$props.recentShouts.map((shout) => {
      shout.fontSize = this.getFontSizeBy(shout.message.length);
      return shout;
    });
  },
  methods: {
    capitalizeName(name) {
      return name.split('')[0].toUpperCase() + name.slice(1);
    },
    getFontSizeBy(length) {
      if (length < 165) return 3;
      else if (length >= 165 && length < 410) return 2;
      else if (length >= 410 && length < 775) return 1.5;
      else if (length >= 775 && length < 1070) return 1.25;
      else if (length >= 1070 && length < 1825) return 1;
      else return 0.75;
    },
    bgImgStyle(index) {
      return {
        background:
          "url('" +
          require(`@/assets/images/carousel_${index}.jpg`) +
          "') no-repeat center center",
        backgroundSize: 'cover',
      };
    },
  },
};
</script>

<style lang="scss">
@use "@/assets/styles/variables.scss" as v;

.v-sheet {
  display: flex;
  flex-direction: column;
}
.v-carousel__controls {
  background-color: v.$main-black-overlay !important;
}
.carousel {
  &__shoutout-identifier {
    color: v.$accent-blue;
  }
  &__company-value {
    background-color: v.$main-black-overlay;
    border-radius: 15px;
    font-size: 3rem;
    margin: 10px 50px 0;
    padding: 0 10px;
  }
  &__shoutout-message {
    background-color: v.$main-black-overlay;
    border-radius: 15px;
    margin: 10px 50px;
    padding: 10px 20px;
  }
  &__shoutout-user {
    background-color: v.$main-black-overlay;
    border-radius: 15px;
    margin: 0 50px 55px;
    padding: 10px 20px;
  }
}
</style>
