<template>
  <v-card
  dense
    class="mx-auto"
  >
      <v-row>
        <v-col cols="12">
          <v-card
          dense
            color="#00695C"
            dark
          >
            <v-card-title class="headline">
              Unlimited music now
            </v-card-title>

            <v-card-subtitle>Listen to your favorite artists and albums whenever and wherever, online and offline.</v-card-subtitle>
              <v-img src="https://firebasestorage.googleapis.com/v0/b/image-server-65bbd.appspot.com/o/restaurant%2Fimage_1609620325390.png?alt=media&amp;token=0e18dc48-ed57-468d-89bb-9625efa31da3&quot"></v-img>
            <v-card-actions>
              <v-btn text>
                Listen Now
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>

        <v-col
          v-for="(item, i) in items"
          :key="i"
          cols="12"
        >
          <v-card
            :color="item.color"
            dark
          >
            <div class="d-flex flex-no-wrap justify-space-between">
              <div>
                <v-card-title
                  class="headline"
                  v-text="item.title"
                ></v-card-title>

                <v-card-subtitle v-text="item.artist"></v-card-subtitle>

                <v-card-actions>
                  <v-btn
                    v-if="item.artist === 'Ellie Goulding'"
                    class="ml-2 mt-3"
                    fab
                    icon
                    height="40px"
                    right
                    width="40px"
                  >
                    <v-icon>mdi-play</v-icon>
                  </v-btn>

                  <v-btn
                    v-else
                    class="ml-2 mt-5"
                    outlined
                    rounded
                    small
                  >
                    START RADIO
                  </v-btn>
                </v-card-actions>
              </div>

              <v-avatar
                class="ma-3"
                size="125"
                tile
              >
                <v-img :src="item.src"></v-img>
              </v-avatar>
            </div>
          </v-card>
        </v-col>
      </v-row>
  </v-card>
</template>

<script>
  export default {
    data: () => ({
      items: [
        {
          color: '#0277BD',
          src: 'https://firebasestorage.googleapis.com/v0/b/image-server-65bbd.appspot.com/o/restaurant%2Fimage_1609620325390.png?alt=media&amp;token=0e18dc48-ed57-468d-89bb-9625efa31da3&quot',
          title: 'Supermodel',
          artist: 'Listen to your favorite artists and albums whenever and wherever, online and offline. Listen to your favorite artists and albums whenever and wherever, online and offline.',
        },
        {
          color: '#00838F',
          src: 'https://cdn.vuetifyjs.com/images/cards/halcyon.png',
          title: 'Halcyon Days',
          artist: 'Ellie Goulding',
        },
                {
          color: '#689f38',
          src: 'https://firebasestorage.googleapis.com/v0/b/image-server-65bbd.appspot.com/o/restaurant%2Fimage_1609620325390.png?alt=media&amp;token=0e18dc48-ed57-468d-89bb-9625efa31da3&quot',
          title: 'Halcyon Days',
          artist: 'Listen to your favorite artists and albums whenever and wherever, online and offline. Listen to your favorite artists and albums whenever and wherever, online and offline. Listen to your favorite artists and albums whenever and wherever, online and offline.',
        },
                {
          color: '#1565C0',
          src: 'https://cdn.vuetifyjs.com/images/cards/foster.jpg',
          title: 'Supermodel',
          artist: 'Foster the People',
        },
                {
          color: '#00E676',
          src: 'https://cdn.vuetifyjs.com/images/cards/halcyon.png',
          title: 'Halcyon Days',
          artist: 'Ellie Goulding',
        },
      ],
    }),
  }
</script>