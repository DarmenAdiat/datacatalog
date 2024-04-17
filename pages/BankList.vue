<template>
  <v-container fluid>
    <v-row justify="center" style="margin: 30px">
    <h2 style="margin-bottom: 20px">Список объектов</h2>
    <v-row align="stretch">
      <v-col
          v-for="item in items"
          :key="item.id"
          cols="12"
          sm="6"
          md="4"
          d-flex
      >
        <v-card class="mx-auto d-flex flex-column" outlined @click="navigateToBank(item)">
          <v-list-item three-line>
            <v-list-item-avatar tile size="80" color="#29537A">
              <span class="white--text headline">{{ item.name.split(' ')[0] }}</span>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title class="headline">{{ item.name }}</v-list-item-title>
              <v-list-item-subtitle>{{ item.npa }}</v-list-item-subtitle>
              <v-list-item-subtitle>{{ formatDate(item.created_at) }}</v-list-item-subtitle>
              <v-chip small color="amber lighten-4" text-color="deep-orange accent-4" style="flex: none">
                {{ item.choice_type_data }}
              </v-chip>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn icon @click="item.show = !item.show">
                <v-icon>{{ item.show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>

          <v-expand-transition>
            <div v-show="item.show" class="mt-auto">
              <v-divider></v-divider>
              <v-card-text>
                <div>Тип данных: {{ item.choice_type_data }}</div>
                <!-- Дополнительные данные можно раскрыть здесь -->
              </v-card-text>
              <v-card-actions>
                <v-btn color="deep-purple accent-4" text>
                  Подробнее
                </v-btn>
              </v-card-actions>
            </div>
          </v-expand-transition>
        </v-card>
      </v-col>
    </v-row>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'InspirePage',
  data() {
    return {
      items: '',
    };
  },
  created(){
    this.init()
  },
  methods:{
    async init(){
      const response = await this.$axios.get('https://datacatalogtest.finreg.kz/api/passports/passport/')
      this.items = response.data
      this.items.map((item) => ({
        ...item,
        show: false, // Это новое свойство для управления видимостью расширяемой панели
      }))
      console.log(response.data)
    },
    navigateToBank(item) {
      this.$router.push({
        name: 'Bank', // Используйте имя маршрута для страницы Bank
        query: { item }
      });
    },
    formatDate(dateString) {
      var date = new Date(dateString);
      var day = date.getDate();
      var year = date.getFullYear();
      var month = date.getMonth()+1;
      var dateStr = month+"/"+day+"/"+year;
      return dateStr
    }
  },

}
</script>
<style scoped>
.headline {
  font-weight: bold;
}
</style>
