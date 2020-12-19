<template>
    <nav>
      <v-app-bar class="blue-grey darken-3" dark>
                <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
                <span>انبارداری</span>
      </v-app-bar>
        <v-navigation-drawer v-model="drawer" dark app class="blue-grey darken-3 py-3">
          <v-layout column align-center>
            <v-flex class="mt-5">
              <v-avatar size="70">
                <v-img src="/shopping.png"></v-img>
              </v-avatar>
              <p class="white--text subheading mt-1 text-center">فروشگاه افق</p>
            </v-flex>
          </v-layout>
            <v-list class="navbar" nav>
                <v-list-item router to="/">
                    <v-list-item-action>
                        <v-icon small>fas fa-tachometer-alt</v-icon>
                    </v-list-item-action>
                    <v-list-item-content>
                        <v-list-item-title>محصولات</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
            <v-layout row style="position: absolute; bottom:0; left: 0; right: 0;">
                <v-flex md12>
                    <div class="clock">
                        <slot :time="time">
                            <span>{{ time.toLocaleTimeString() }}</span>
                        </slot>
                    </div>
                </v-flex>
            </v-layout>
        </v-navigation-drawer>
    </nav>
</template>

<script>
export default {
    data: () => ({
      drawer: true,
      value: 0,
      time: new Date()
    }),
    created() {
      setInterval(() => {
        this.time = new Date(this.time.getTime() + 1000);
      }, 1000);
    }
}
</script>

<style>

    .v-data-footer__pagination {
        direction: ltr;
    }

    .clock {
        background-color:#546E7A;
        padding: 20px 0px;
        direction: ltr;
        text-align: center;
        color: #fff;
        font-size: 20px;
        font-family: IRANsans;
    }

</style>