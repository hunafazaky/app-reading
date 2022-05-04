<template>
  <v-card>
    <v-card-text>
      <HashtagButton />
      <v-row class="pa-2">
        <template>
          <v-col 
            class="pa-1"
            v-for="work in works"
            :cols="(12 / cardLimit)"
            :key="work.id" 
          >
            <ArticleCard
              :work="work"
              :limit="{'text':60, 'hashtag':(Math.ceil(2 / cardLimit))}"
            />
          </v-col>
        </template>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
import HashtagButton from "~/components/HashtagButton.vue";
import ArticleCard from "~/components/ArticleCard.vue";
export default {
  components: { HashtagButton, ArticleCard },
  name: "IndexPage",
  layout: 'unregisted',
  data: () => ({
  }),
  computed: {
    works () {
      return this.$store.state.works.list
    }
  },
}
</script>