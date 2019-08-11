/* eslint-disable vue/no-unused-components */
<template>
  <div>
    <b-row class="app-row survey-app">
      <b-colxx xxs="12">
        <h1>My Investors List</h1>
        <div class="separator mb-5"></div>
        <b-row v-if="isLoad" key="itemList">
          <b-colxx xxs="12" class="mb-3" v-for="(data,index) in items" :key="index" :id="data.id">
            <b-card class="d-flex flex-row" no-body>
              <router-link :to="`?p=${data.id}`" class="d-flex">
                <img
                  :src="data.image"
                  class="img-thumbnail border-0 rounded-circle list-thumbnail align-self-center ml-4"
                >
              </router-link>
              <div class="card-body d-flex justify-content-between align-items-lg-center">
                <div>
                  <router-link :to="`?p=${data.id}`">
                    <span class="list-item-heading truncate">{{data.name}}</span>
                    <b-badge pill variant="primary">{{ data.type }}</b-badge>
                  </router-link>
                  <p class="text-muted">
                    <i class="simple-icon-location-pin"></i>
                    {{data.location}}
                  </p>
                  <div>
                    <a target="_blank" :href="data.website" class="btn icon-button btn-secondary">
                      <i class="simple-icon-link"></i>
                    </a>
                    <a target="_blank" :href="data.linkedin" class="btn icon-button btn-secondary">
                      <i class="simple-icon-social-linkedin"></i>
                    </a>
                    <a target="_blank" :href="data.twitter" class="btn icon-button btn-secondary">
                      <i class="simple-icon-social-twitter"></i>
                    </a>
                  </div>
                </div>
                <div>
                  <b-badge
                    pill
                    :variant="statuses[data.status].color"
                  >{{ statuses[data.status].label }}</b-badge>
                  <div class="mb-1 mt-3">
                    Emails sent&nbsp;
                    <span class="float-right text-muted">2/10</span>
                  </div>
                  <b-progress :value="(2 / 10) * 100"></b-progress>
                  <div class="mt-3">
                    Pitch Deck&nbsp;
                    <span class="float-right text-muted">{{ data.deck }}</span>
                  </div>
                </div>
                <div class="custom-control custom-checkbox pl-1 align-self-center pr-4">
                  <router-link to="investor-board" class="btn btn-primary">
                    <i class="iconsminds-mail-forward"/> Go To Board
                  </router-link>
                </div>
              </div>
            </b-card>
          </b-colxx>
        </b-row>

        <div v-else class="loading" key="itemLoading"></div>
      </b-colxx>
    </b-row>
    <application-menu>
      <vue-perfect-scrollbar :settings="{ suppressScrollX: true, wheelPropagation: false }">
        <div class="p-4">
          <p class="text-muted text-small mb-2">Filter Status</p>
          <div>
            <div class="d-sm-inline-block mb-1" v-for="(l,index) in statuses" :key="index">
              <b-badge pill :variant="`outline-${l.color}`" class="mb-1 mr-1">{{l.label}}</b-badge>
            </div>
          </div>
          <p class="text-muted text-small mt-4 mb-2">Board Overview</p>
          <ul class="list-unstyled mb-4">
            <li class="nav-item">
              <i class="iconsminds-business-man"/>
              Investors On Board
              <span class="float-right">342</span>
            </li>
            <li class="nav-item">
              <i class="iconsminds-mail-send"/>
              Emails Sent
              <span class="float-right">659</span>
            </li>
            <li class="nav-item">
              <i class="iconsminds-speach-bubbles"/>
              Investors Answered
              <span class="float-right">34</span>
            </li>
            <li class="nav-item">
              <i class="iconsminds-diploma-2"/>
              Pitchdeck Viewed
              <span class="float-right">18</span>
            </li>
          </ul>
        </div>
      </vue-perfect-scrollbar>
    </application-menu>
  </div>
</template>
<script>
import { mapGetters, mapMutations, mapActions } from "vuex";

import vSelect from "vue-select";
import ApplicationMenu from "@/components/Common/ApplicationMenu";

export default {
  components: {
    vSelect,
    ApplicationMenu
  },
  data() {
    return {
      items: [
        {
          image: "/assets/img/profile-pic-l-4.jpg",
          name: "Radik Haziev",
          location: "Paris, France",
          industry: "SaaS, Mobile, Healthcare",
          comments:
            "Spark Capital is a venture capital firm that invests in startups led by creative thinkers. The firm invests in the media, software and technology sectors. It was founded in 2005 and is headquartered in Boston, Massachusetts.",
          type: "VC",
          website: "http://www.sparkcapital.com",
          linkedin: "http://www.linkedin.com/pub/kerri-golden/0/880/8",
          twitter: "http://www.linkedin.com/pub/kerri-golden/0/880/8",
          status: "unanswered",
          deck: "Viewed"
        },
        {
          image: "/assets/img/profile-pic-l.jpg",
          name: "Mark Zuckerberg",
          location: "San Francisco, USA",
          industry: "Social Media, Entertainment",
          comments:
            "Spark Capital is a venture capital firm that invests in startups led by creative thinkers. The firm invests in the media, software and technology sectors. It was founded in 2005 and is headquartered in Boston, Massachusetts.",
          type: "Angel",
          website: "http://www.sparkcapital.com",
          linkedin: "http://www.linkedin.com/pub/kerri-golden/0/880/8",
          twitter: "http://www.linkedin.com/pub/kerri-golden/0/880/8",
          status: "followup",
          deck: "Sent"
        }
      ],
      sort: { column: "title", label: "Title" },
      sortOptions: [
        { column: "title", label: "Title" },
        { column: "category", label: "Category" },
        { column: "label", label: "Label" },
        { column: "status", label: "Status" }
      ],
      search: "",
      selectedItems: [],
      categories: [
        { label: "Flexbox", value: "Flexbox" },
        { label: "Sass", value: "Sass" },
        { label: "Vue", value: "Vue" },
        { label: "React", value: "React" }
      ],
      statuses: {
        unanswered: { label: "Answer Required", color: "danger" },
        research: { label: "Research", color: "secondary" },
        followup: { label: "Follow Up", color: "primary" },
        discussion: { label: "Discussion", color: "success" },
        rejected: { label: "Said No", color: "dark" }
      },
      isLoad: true
    };
  },
  methods: {
    ...mapActions(["getTodoItems"]),
    ...mapMutations(["addTodoItem"]),
    isSelectedAll() {
      return this.selectedItems.length >= this.items.length;
    },
    isAnyItemSelected() {
      return (
        this.selectedItems.length > 0 &&
        this.selectedItems.length < this.items.length
      );
    },
    hideModal(refname) {
      this.$refs[refname].hide();
    },
    changeOrderBy(sort) {
      this.sort = sort;
    },
    addNewItem() {
      const date = new Date();
      this.addTodoItem({
        title: this.newItem.title,
        detail: this.newItem.detail,
        category: this.newItem.category.value,
        label: this.newItem.label.value,
        status: this.newItem.status,
        date:
          date.getDay() + "." + date.getMonth() + 1 + "." + date.getFullYear(),
        labelColor: this.newItem.label.color
      });
      this.newItem = {
        title: "",
        category: "",
        detail: "",
        label: "",
        status: ""
      };
      this.hideModal("modalright");
    },
    selectAll(isToggle) {
      if (this.selectedItems.length >= this.items.length) {
        if (isToggle) {
          this.selectedItems = [];
        }
      } else {
        this.selectedItems = this.items.map(x => x.id);
      }
    },
    keymap(event) {
      switch (event.srcKey) {
        case "select":
          this.selectAll(false);
          break;
        case "undo":
          this.selectedItems = [];
          break;
      }
    },
    getIndex(value, arr, prop) {
      for (var i = 0; i < arr.length; i++) {
        if (arr[i][prop] === value) {
          return i;
        }
      }
      return -1;
    },
    toggleItem(event, itemId) {
      if (event.shiftKey && this.selectedItems.length > 0) {
        let itemsForToggle = this.items;
        var start = this.getIndex(itemId, itemsForToggle, "id");
        var end = this.getIndex(
          this.selectedItems[this.selectedItems.length - 1],
          itemsForToggle,
          "id"
        );
        itemsForToggle = itemsForToggle.slice(
          Math.min(start, end),
          Math.max(start, end) + 1
        );
        this.selectedItems.push(
          ...itemsForToggle.map(item => {
            return item.id;
          })
        );
      } else {
        if (this.selectedItems.includes(itemId)) {
          this.selectedItems = this.selectedItems.filter(x => x !== itemId);
        } else {
          this.selectedItems.push(itemId);
        }
      }
    },
    handleContextmenu(vnode) {
      if (!this.selectedItems.includes(vnode.key)) {
        this.selectedItems = [vnode.key];
      }
    },
    onContextCopy() {
      console.log(
        "context menu item clicked - Copy Items: ",
        this.selectedItems
      );
    },
    onContextArchive() {
      console.log(
        "context menu item clicked - Move to Archive Items: ",
        this.selectedItems
      );
    },
    onContextDelete() {
      console.log(
        "context menu item clicked - Delete Items: ",
        this.selectedItems
      );
    }
  },
  mounted() {
    this.getTodoItems();
  }
};
</script>
