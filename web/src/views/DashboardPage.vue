<template>
  <v-container class="flex-column">
    <v-col mx="2" v-if="unarchivedBoards.length !== 0">
      <h1 class="mb-5">Активные проекты:</h1>

      <v-row>
        <v-col
          v-for="board in unarchivedBoards"
          :key="board.id"
          xs="12"
          sm="6"
          md="3"
          xl="3"
        >
          <dashboard-item :board="board"></dashboard-item>
        </v-col>
      </v-row>
    </v-col>

    <v-col mx="2" v-if="this.archivedBoards.length !== 0">
      <h2 class="mb-5">Проекты в архиве:</h2>
      <v-row>
        <v-col
          v-for="board in archivedBoards"
          :key="board.id"
          xs="12"
          sm="6"
          md="3"
          xl="3"
        >
          <dashboard-item :board="board"></dashboard-item>
        </v-col>
      </v-row>
    </v-col>

    <v-card
      v-if="this.boards.length == 0 && this.isLoading == false"
      outlined
      id="preview"
      class="mx-auto my-10 d-flex justify-space-between"
      style="border:none"
    >
      <div>
        <v-card-title class="text-h3 mb-5">
          Добро пожаловать в  <i>ТаскУм!</i>
        </v-card-title>

        <v-card-subtitle class="text-h6">
       Создайте свой первый проект прямо сейчас
        </v-card-subtitle>

        <v-card-text class="text-subtitle-1">
     ТаскУм удобный инстурмент для коллабораций с другими людьми, чтобы вместе рабоать над общими задачами
        </v-card-text>

        <DashboardEdit class="pl-4" color="black" />
      </div>
      <v-img src="../assets/main.png" max-width="700px" height="auto"></v-img>
    </v-card>
  </v-container>
</template>

<script>
import DashboardItem from "../components/Dashboard/DashboardItem.vue";
import DashboardEdit from "../components/Dashboard/DashboardEdit.vue";

export default {
  created() {
    this.$store.commit("setActiveBoard", null);
  },
  computed: {
    boards() {
      return this.$store.state.boards;
    },
    isLoading() {
      return this.$store.state.isLoading;
    },
    unarchivedBoards() {
      return this.$store.getters.unarchivedBoards;
    },
    archivedBoards() {
      return this.$store.getters.archivedBoards;
    },
  },
  components: { DashboardItem, DashboardEdit },
};
</script>

<style scoped>
@media (max-width: 1000px) {
  #preview {
    flex-wrap: wrap !important;
  }
}
</style>
