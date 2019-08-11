<template>
  <b-colxx class="disable-text-selection">
    <b-row>
      <b-colxx xxs="12">
        <h1>Find Investors</h1>
        <b-form @submit.prevent="onInlineSubmit" inline>
          <label class="form-group has-float-label">
            <b-form-input type="text" v-model="name" class="mb-2 mr-sm-2 mb-sm-0"/>
            <span>Company/Investor Name</span>
          </label>
          <label class="form-group has-float-label">
            <b-form-input type="text" v-model="location" class="mb-2 mr-sm-2 mb-sm-0"/>
            <span>Location</span>
          </label>
          <!--<div class="form-group has-float-label">
            <v-select v-model="type" :options="types" class="mb-2 mr-sm-2 mb-sm-0"/>
            <span>Type</span>
          </div>-->
          <div class="form-group has-float-label">
            <v-select
              v-model="focus"
              multiple
              :options="industries"
              class="mb-2 mr-sm-2 mb-sm-0"
              style="min-width: 172px;"
            />
            <span>Industry</span>
          </div>
          <b-button @click="loadItems">Search</b-button>
        </b-form>
        <div class="separator mb-5 mt-5"/>
      </b-colxx>
    </b-row>
    <template v-if="isLoad">
      <b-row key="thumb">
        <b-colxx xxs="12" class="mb-3" v-for="(data,index) in items" :key="index" :id="data.id">
          <b-card class="d-flex flex-row" no-body>
            <img
              v-if="data.image"
              :src="data.image"
              class="d-flex img-thumbnail border-0 rounded-circle list-thumbnail align-self-center ml-4"
            >
            <div class="d-flex flex-grow-1">
              <div
                class="card-body align-self-center d-flex flex-column flex-lg-row justify-content-between min-width-zero align-items-lg-center"
              >
                <div class="w-90 w-sm-100 mr-4">
                  <a target="_blank" :href="data.firmLink">
                    <span class="list-item-heading truncate">{{data.name}}</span>
                    <b-badge
                      v-if="data.firmName"
                      pill
                      variant="primary"
                      style="position: relative; top: -2px;"
                    >{{ data.firmName }}</b-badge>
                  </a>
                  <p class="text-muted">
                    <i class="simple-icon-location-pin"></i>
                    {{data.location}}
                  </p>
                  <div>
                    <a
                      v-if="data.angelList"
                      target="_blank"
                      :href="data.angelList"
                      class="btn icon-button btn-secondary"
                    >
                      <i class="simple-icon-link"></i>
                    </a>
                    <a
                      v-if="data.linkedin"
                      target="_blank"
                      :href="data.linkedin"
                      class="btn icon-button btn-secondary"
                    >
                      <i class="simple-icon-social-linkedin"></i>
                    </a>
                    <a
                      v-if="data.twitter"
                      target="_blank"
                      :href="data.twitter"
                      class="btn icon-button btn-secondary"
                    >
                      <i class="simple-icon-social-twitter"></i>
                    </a>
                  </div>
                </div>
                <div>
                  <h5>
                    <i class="simple-icon-chart"></i>
                    {{ data.focus }}
                  </h5>
                  <p>{{data.description}}</p>
                </div>
              </div>
              <div class="custom-control custom-checkbox pl-1 align-self-center pr-4">
                <state-button variant="success" :click="successButtonClick">Add To List</state-button>
              </div>
            </div>
          </b-card>
        </b-colxx>
      </b-row>
    </template>
    <template v-else>
      <div class="loading"></div>
    </template>
  </b-colxx>
</template>
<script>
import { DataListIcon, ThumbListIcon, ImageListIcon } from "@/components/Svg";
import StateButton from "@/components/Common/StateButton";
import vSelect from "vue-select";
import axios from "axios";

import ImageListItem from "@/components/Listing/ImageListItem";
import ThumbListItem from "@/components/Listing/ThumbListItem";
import DataListItem from "@/components/Listing/DataListItem";
import { apiUrl } from "@/constants/config";

export default {
  components: {
    DataListIcon,
    ThumbListIcon,
    ImageListIcon,
    vSelect,
    ImageListItem,
    ThumbListItem,
    DataListItem,
    StateButton
  },
  data() {
    return {
      isLoad: false,
      types: ["VC", "Accelerator/Incubator", "Corporate", "Angel"],
      industries: [
        "Finance",
        "E-Commerce",
        "Social",
        "Mobile",
        "AI/ML",
        "Healthcare",
        "Big Data",
        "SaaS",
        "Energy",
        "Security",
        "Enterprise",
        "Robotics",
        "Retail",
        "Consumer",
        "Education",
        "Life Sci",
        "Energy",
        "Real Estate",
        "Enterprise",
        "Travel",
        "Government",
        "Food & Drink"
      ],
      name: null,
      location: null,
      type: null,
      focus: [],
      items: []
    };
  },
  methods: {
    successButtonClick() {
      // eslint-disable-next-line promise/param-names
      return new Promise((success, fail) => {
        setTimeout(() => {
          success("Everything went right!");
        }, 2000);
      });
    },
    loadItems() {
      this.isLoad = false;
      axios
        .post(apiUrl + "/investors/search", {
          name: this.name,
          location: this.location,
          focus: this.focus
        })
        .then(res => {
          this.items = res.data;
          this.isLoad = true;
        });
    }
  },
  mounted() {
    this.loadItems();
  }
};
</script>
