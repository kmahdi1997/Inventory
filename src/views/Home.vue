<template >
  <v-container>
    <v-data-table
      :headers="headers"
      :items="desserts"
      sort-by="date"
      class="elevation-1"
      :page.sync="page"
      :items-per-page="itemsPerPage"
      hide-default-footer
      @page-count="pageCount = $event"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn dark outlined color="blue-grey " class="darken-3" v-bind="attrs" v-on="on">
                کالای جدید
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="headline">{{ formTitle }}</span>
              </v-card-title>
              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.name"
                        label="نام کالا"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.index"
                        label="تعداد"
                        type="number"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-menu
                        ref="menu"
                        v-model="menu"
                        :close-on-content-click="false"
                        :return-value.sync="date"
                        transition="scale-transition"
                        offset-y
                        min-width="290px"
                      >
                        <template v-slot:activator="{ on, attrs }">
                          <v-text-field
                            class="farsidatefield"
                            v-model="editedItem.date"
                            label="تاریخ"
                            prepend-icon="mdi-calendar"
                            readonly
                            v-bind="attrs"
                            v-on="on"
                          ></v-text-field>
                        </template>
                        <v-date-picker
                          class="farsidate"
                          v-model="editedItem.date"
                          no-title
                          scrollable
                        >
                          <v-spacer></v-spacer>
                          <v-btn text color="primary" @click="menu = false">
                            لغو
                          </v-btn>
                          <v-btn
                            text
                            color="primary"
                            @click="$refs.menu.save(date)"
                          >
                            ذخیره
                          </v-btn>
                        </v-date-picker>
                      </v-menu>
                    </v-col>
                    <v-col cols="12" sm="6" md="4" lg="12">
                      <v-text-field
                        v-model="editedItem.description"
                        label="توضیحات"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  لغو
                </v-btn>
                <v-btn color="blue darken-1" text @click="save">
                  ذخیره
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="deletItem" max-width="500px">
            <v-card>
              <v-card-title class="headline"> آیا این کالا از لیست حذف شود؟ </v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete"> خیر </v-btn>
                <v-btn color="blue darken-1" text @click="deleteItemConfirm"> بله </v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)">
          mdi-pencil
        </v-icon>
        <v-icon small @click="deleteItem(item)">
          mdi-delete
        </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize">
          Reset
        </v-btn>
      </template>
    </v-data-table>
    <div class="text-center pt-2">
      <v-pagination class="pagination"
        v-model="page"
        :length="pageCount"
      ></v-pagination>
    </div>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    page: 1,
    pageCount: 0,
    dialog: false,
    deletItem: false,
    headers: [
      { text: "نام کالا", align: "start", sortable: false, value: "name" },
      { text: "تعداد", value: "index" },
      { text: "تاریخ", value: "date" },
      { text: "توضیحات", value: "description", sortable: false },
      { text: "ویرایش/حذف", value: "actions", sortable: false },
    ],

    desserts: [],
    editedIndex: -1,
    editedItem: {
      name: "",
      index: "",
      date: "",
      description: "",
    },
    defaultItem: {
      name: "",
      index: "",
      date: "",
      description: "",
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "کالای جدید" : "ویرایش کالا";
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    deletItem(val) {
      val || this.closeDelete();
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    initialize() {
      this.desserts = [
        {
          name: "هارد",
          index: 159,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 237,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 262,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 305,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 356,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 375,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 392,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 408,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 452,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
        {
          name: "هارد",
          index: 518,
          date: "۲۰۲۰-۱۲-۱۰",
          description: "این کالا توسط شرکت ...",
        },
      ];
    },

    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.deletItem = true
    },

    deleteItemConfirm() {
      this.desserts.splice(this.editedIndex, 1)
      this.closeDelete()
    },

    close() {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete() {
      this.deletItem = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
      } else {
        this.desserts.push(this.editedItem)
      }
      this.close()
    },
  },
}
</script>

<style scoped>

.farsidate,
.farsidatefield {
  font-family: IRANSans;
}

.v-application .headline {
  font-family: Vazir!important;
}

.pagination {
  font-family: IRANsans;
}
</style>


