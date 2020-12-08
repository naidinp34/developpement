<template>
  <div class="home">
    <v-container class="my-5">
      <v-container>
        <v-row no-gutters  >
          <v-col cols="12">
            <v-row justify="center">
            <div>
              <h1>Trouver des blogs où commenter</h1>
            </div>
          </v-row>
          </v-col>
        </v-row>
      </v-container>
      <v-toolbar flat class="my-5">
        <v-text-field
          class="font-weight-medium text-h5"
          prepend-icon="mdi-magnify"
          rounded
          hide-details
          single-line
          solo
          placeholder="Quels mots clefs rechercher ?"
          @keydown.enter="searchKeywordsOnEnter()"
          v-model="searchKeywords"
        ></v-text-field>
      </v-toolbar>
      <template v-if="loading">
        <v-container style="height: 400px;">
          <v-row class="fill-height" align-content="center" justify="center">
            <v-col class="subtitle-1 text-center" cols="12">Analyse des sites resultats en cours...</v-col>
            <v-col cols="6">
              <v-progress-linear color="primary" indeterminate rounded height="6"></v-progress-linear>
            </v-col>
          </v-row>
        </v-container>
      </template>
      <template v-if="errored">
        <v-snackbar v-model="errored" color="red" timeout="6000" top="true">
          <div
            font-weight-bold
            white--text
            text-h2
          >Le service rencontre une erreur, merci de retenter dans un moment.</div>
        </v-snackbar>
      </template>
      <template v-else>
        <template v-if="displayResultUIComponents">
          <v-toolbar flat class="my-5 mb-0 grey--light">
            <div>
              <v-checkbox hide-details v-model="selectAllRows" @click="selectAll()"></v-checkbox>
            </div>
            <v-btn small class="mx-3" @click="openSelectedInNewTab()">
              <v-icon left small>mdi-application-export</v-icon>
              <span>ouvrir tous les éléments sélectionnés</span>
            </v-btn>
            <v-btn flat small @click="hideCommentedUrls()" class="mx-3">
              <template v-if="filteredWithOnlyNoCommentedSites">
                <v-icon left small>mdi-filter-remove-outline</v-icon>
                <span>afficher tous les résultats</span>
              </template>
              <template v-else>
                <v-icon left small>mdi-filter-outline</v-icon>
                <span>cacher les sites déjà commentés</span>
              </template>
            </v-btn>
          </v-toolbar>
        </template>
        <v-card
          flat
          v-for="(result, key) in getFilteredSearchResults"
          :key="key.id"
          :class="`px-3 pt-2  result ${result.scrap_is_success}`"
        >
          <v-row no-gutters>
            <v-col>
              <v-row no-gutters wrap>
                <v-col cols="12" sm="12" md="12" mg="12" xl="12">
                  <v-row no-gutters align-center>
                    <v-checkbox
                      align-bottom
                      class="shrink mr-0 mt-0"
                      hide-details
                      v-model="result.selected"
                    ></v-checkbox>
                    <div class="text-h6">{{result.domain}}</div>
                  </v-row>
                </v-col>
              </v-row>
              <v-row class="pl-8 py-1" no-gutters wrap>
                <v-col cols="12" sm="12" md="12" mg="12" xl="12">
                  <div>
                    <v-btn
                      outlined
                      small
                      :href="`${result.url}`"
                      target="_blank"
                      class="no-uppercase"
                    >{{result.url}}</v-btn>
                  </div>
                </v-col>
              </v-row>
              <v-row class="pl-8 pb-2" no-gutters wrap>
                <template v-if="result.already_comment == 'oui'">
                  <v-chip
                    color="primary"
                    :class="`${result.already_comment} right font-weight-bold white--text caption my-2`"
                  >Déjà commenté</v-chip>
                </template>
                <template
                  v-if="result.already_comment == 'non' && result.scrap_is_success == 'yes'"
                >
                  <v-chip
                    color="green"
                    :class="`${result.already_comment} right font-weight-bold white--text caption my-2`"
                  >Possible de laisser un commentaire</v-chip>
                </template>
                <template v-if="result.already_comment == 'non' && result.scrap_is_success == 'no'">
                  <v-chip
                    color="red"
                    :class="`${result.already_comment} right font-weight-bold white--text caption my-2`"
                  >Analyse bloquée, ouvertue manuelle nécessaire</v-chip>
                </template>
                <div class="caption px-4 pt-3">Dernière analyse de la page : {{result.last_scrap}}</div>
              </v-row>
            </v-col>
          </v-row>
          <v-divider></v-divider>
        </v-card>
      </template>
    </v-container>
  </div>
</template>

<script>
import axios from "axios";
import KeywordsSearchResultAdapter from "../entities/KeywordsSearchResultAdapter";
export default {
  data() {
    return {
      searchResults: [],
      filteredWithOnlyNoCommentedSites: false,
      selectAllRows: false,
      json_response: "",
      searchKeywords: "",
      displayResultUIComponents: false,
      loading: false,
      errored: false,
    };
  },
  methods: {
    hideCommentedUrls() {
      this.filteredWithOnlyNoCommentedSites = !this
        .filteredWithOnlyNoCommentedSites;
    },
    openSelectedInNewTab() {
      this.getFilteredSearchResults.forEach((item) => {
        if (item.selected) {
          window.open(item.url, "_" + item.id);
        }
      });
    },
    selectAll() {
      if (this.selectAllRows) {
        this.getFilteredSearchResults.forEach((item) => {
          item.selected = true;
        });
      } else {
        this.getFilteredSearchResults.forEach((item) => {
          item.selected = false;
        });
      }
    },
    formatResults(results) {
      var adapterList = [];
      results.forEach(function (element) {
        adapterList.push(new KeywordsSearchResultAdapter(element));
      });
      this.displayResultUIComponents = true;
      this.searchResults = adapterList;
    },
    searchKeywordsOnEnter() {

      //reset display if existing search was made before
      this.searchResults = [];
      this.displayResultUIComponents = false;

      var requestUrl = "http://localhost:3000/search";
      var keywordsParameter = this.searchKeywords.split(" ");
      var parametersString = "?keywords=";

      for (const element of keywordsParameter) {
        if (element != "") {
          parametersString += element + ",";
        }
      }
      if (parametersString.lastIndexOf(",") == parametersString.length - 1) {
        parametersString = parametersString.substring(
          0,
          parametersString.length - 1
        );
      }
      requestUrl = requestUrl + parametersString;

      this.loading = true;

      axios
        .get(requestUrl)
        .then((response) => this.formatResults(response.data.results))
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
  },
  computed: {
    getFilteredSearchResults: function () {
      if (this.filteredWithOnlyNoCommentedSites) {
        return this.searchResults.filter(
          (result) => result.already_comment == "non"
        );
      } else {
        return this.searchResults;
      }
    },
  },

  mounted() {},
};
</script>

<style>
.result.yes {
  border-left: 5px solid #3cd1c2;
}
.result.no {
  border-left: 5px solid tomato;
}

.no-uppercase {
  text-transform: none;
}
</style>
