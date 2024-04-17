<template>
  <v-container>
    <v-row justify="center" align="center">
      <v-col cols="8" style="margin: 20px">
        <v-card style="align-content: center">
          <v-card-title class="headline">
            Поиск
          </v-card-title>
          <v-text-field
              v-model="searchQuery"
              prepend-icon="mdi-magnify"
              label="Поиск"
              single-line
              hide-details
              @input="performSearch"
              style="margin: 15px"
          ></v-text-field>
          <template v-if="searchResults.length>0">
            <v-list>
              <v-list-item
                  v-for="(item, index) in searchResults"
                  :key="index"
                  @click="goToItem(item)"
              >
                <v-list-item-content>
                  <v-list-item-title>{{ item.name }}</v-list-item-title>
                  <v-list-item-subtitle>{{ item.npa }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </template>
<!--          <template v-else>-->
<!--            <v-list>-->
<!--              <v-list-item-->
<!--                  v-for="(item, index) in emptyResults"-->
<!--                  :key="index"-->
<!--              >-->
<!--                <v-list-item-content>-->
<!--                  <v-list-item-title>{{ item.title }}</v-list-item-title>-->
<!--                </v-list-item-content>-->
<!--              </v-list-item>-->
<!--            </v-list>-->
<!--          </template>-->
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      searchQuery: '',  // Данные ввода
      searchResults: [],  // Массив для результатов поиска
      show: false,
      emptyResults: [],
      result: [],
    };
  },
  created(){
    this.empty()
    this.init()
  },
  methods: {
    async init(){
      console.log(this.searchResults)
      const response = await this.$axios.get('https://datacatalogtest.finreg.kz/api/passports/passport/')
      this.result = response.data
    },
    performSearch() {
      if (this.searchQuery) {
        // Фильтрация на основе запроса
        this.searchResults = this.result.filter((item) =>
            item.name.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      } else {
        // Очистка результатов поиска, если запрос пустой
        this.searchResults = [];
      }
    },
    empty() {
      this.emptyResults = [
        {title: 'Введите данные для поиска'},
        // Предполагаем, что это данные, которые вы бы загрузили
      ]
    },
    goToItem(id) {
      this.$router.push({
        path: 'Bank',
        query: id
      }); // Переход на новую страницу с передачей ID
    }
  },
  watch: {
    // Добавляем watcher для searchQuery
    searchQuery() {
      // Вызываем performSearch каждый раз, когда searchQuery изменяется
      this.performSearch();
    }
  }
}
</script>
