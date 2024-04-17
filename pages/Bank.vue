<template>
  <v-container>
    <v-row justify="center" style="margin: 30px">
      <v-col cols="8">
        <v-card>
          <v-card-title class="header">Отчет о ценных бумагах</v-card-title>
          <v-tabs v-model="tab" style="display: flex; justify-content: center" align-tabs="center">
            <v-tabs-slider color="black"></v-tabs-slider>
            <v-tab
                v-for="item in items"
                :key="item"
                style="font-size: 13px"
            >{{ item }}
            </v-tab>
          </v-tabs>
          <v-tabs-items v-model='tab' content-class="ma-0">
            <v-tab-item>
              <v-card-text width="50%">
                <v-list dense>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="title">Название:</v-list-item-title>
                      <v-list-item-subtitle class="subtitle">{{ reportData.name }}</v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="title">НПА:</v-list-item-title>
                      <v-list-item-subtitle class="subtitle" style="word-break: break-all; white-space: pre-wrap">{{ reportData.npa }}</v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="title">Дата создания:</v-list-item-title>
                      <v-list-item-subtitle class="subtitle">{{ reportData.created_at }}</v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="title">Владелец:</v-list-item-title>
                      <v-list-item-subtitle class="subtitle">{{ reportData.owner }}</v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>
                </v-list>
              </v-card-text>
            </v-tab-item>
            <v-tab-item>

            </v-tab-item>
            <v-tab-item>

            </v-tab-item>
            <v-tab-item>
              <v-card-text width="50%">
                <v-list dense>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="title">Название:</v-list-item-title>
                      <v-list-item-subtitle class="subtitle">{{ table_structure.name }}</v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="title">НПА:</v-list-item-title>
                      <v-list-item-subtitle class="subtitle" style="word-break: break-all; white-space: pre-wrap">{{ table_structure.termin }}</v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="title">Дата создания:</v-list-item-title>
                      <v-list-item-subtitle class="subtitle">{{ table_structure.alternative_name }}</v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="title">Владелец:</v-list-item-title>
                      <v-list-item-subtitle class="subtitle">{{ table_structure.details }}</v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>
                </v-list>
              </v-card-text>
            </v-tab-item>
          </v-tabs-items>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'InspirePage',
  data() {
    return {
      items: [
        'Описание', 'Связи', 'Метрики', 'Структура таблицы',
      ],
      reportData: {},
      tab:'/',
      table_structure: {},
    };
  },
  created(){
    this.init()
  },
  methods:{
    async init(){
      this.reportData = this.$route.query
      const response = await this.$axios.get('https://datacatalogtest.finreg.kz/api/passports/business-glossary/',{
        params:{
          passport_data_structure: this.reportData.id
        }
      })
      this.table_structure = response.data[0]
      console.log(this.table_structure)
    },
  },

}
</script>
<style scoped>
.title{
  font-size: 18px!important; /* Увеличиваем размер шрифта */
}
.subtitle{
  font-size: 16px!important;
  line-height: 1.4rem!important;
}
.header{
  font-size: 22px!important;
}
</style>
