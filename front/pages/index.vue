<template>
  <div>
    <b-container class="text-center" fluid>
      <b-row class="d-flex justify-content-center">
        <b-col md="6" class="my-2">
          <!-- Imagem 1 -->
          <b-img
            src="https://via.placeholder.com/300"
            fluid
            alt="Imagem 1"
          ></b-img>
        </b-col>
        <b-col md="6" class="mt-2">
          <!-- Imagem 2 -->
          <b-img
            src="https://via.placeholder.com/300"
            fluid
            alt="Imagem 2"
          ></b-img>
        </b-col>
      </b-row>
      <!-- Segunda linha -->
      <b-row class="mt-4 d-flex justify-content-center">
        <b-col md="12">
          <b-table
            :items="launches"
            :fields="fields"
            :current-page="currentPage"
            :per-page="perPage"
            :filter="searchQuery"
            :sort-by.sync="sortBy"
            :sort-desc.sync="sortDesc"
            @filtered="onFiltered"
          >
            <template #cell(flight_number)="row">
              <b-link :href="`/launches/${row.item.flight_number}`">{{
                row.item.flight_number
              }}</b-link>
            </template>
            <template #cell(youtube_link)="row">
              <b-link
                :href="`https://youtu.be/${row.item.youtube_link}`"
                target="_blank"
                >{{ row.item.youtube_link }}</b-link
              >
            </template>
            <template #cell(links_patch_small)="row">
              <b-link :href="row.item.links_patch_small" target="_blank"
                ><img
                  :src="row.item.links_patch_small"
                  alt="Patch"
                  style="width: 50px; height: 50px"
              /></b-link>
            </template>
          </b-table>

          <b-pagination
            v-if="totalPages > 1"
            v-model="currentPage"
            :total-rows="totalDocs"
            :per-page="perPage"
            @input="onPageChange"
            :no-prev="!hasPrev"
            :no-next="!hasNext"
          ></b-pagination>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      launches: [],
      fields: [
        { key: "flight_number", label: "Flight Number" },
        { key: "name", label: "Name" },
        { key: "date_utc", label: "Date (UTC)" },
        { key: "youtube_link", label: "YouTube Link" },
        { key: "rocket.name", label: "Rocket Name" },
        { key: "links_patch_small", label: "Image",
        },
      ],
      currentPage: 1,
      perPage: 5,
      totalDocs: 0,
      totalPages: 0,
      searchQuery: "",
      sortBy: "",
      sortDesc: false,
      hasNext: false,
      hasPrev: false,
    };
  },
  mounted() {
    this.fetchLaunches();
  },
  watch: {
    currentPage: {
      immediate: true,
      handler: "fetchLaunchesOnPageChange",
    },
  },
  methods: {
    async fetchLaunchesOnPageChange() {
      this.fetchLaunches();
    },
  async fetchLaunches() {
      this.$axios
        .get("/launches", {
          params: {
            page: this.currentPage,
            perPage: this.perPage,
            search: this.searchQuery,
          },
        })
        .then((res) => {
          this.launches = res.data.results;
          this.totalDocs = res.data.totalDocs;
          this.totalPages = res.data.totalPages;
          this.hasNext = res.data.hasNext;
          this.hasPrev = res.data.hasPrev;
        });
    },
   async onFiltered(filter: string) {
      this.currentPage = 1;
      this.searchQuery = filter;
      this.fetchLaunches();
    },
   async onPageChange(newPage: number) {
      this.currentPage = newPage;
    },
  },
};
</script>
