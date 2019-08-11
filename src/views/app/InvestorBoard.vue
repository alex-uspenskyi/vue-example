<template>
  <div v-if="investor">
    <b-row class="app-row">
      <b-colxx xxs="12" class="chat-app">
        <h3>{{investor.name}} Emails</h3>
        <div class="separator mb-4"/>
        <vue-perfect-scrollbar
          class="scroll"
          :settings="{ suppressScrollX: true, wheelPropagation: false }"
          ref="chatArea"
        >
          <div v-for="(message,index) in messages" :key="`message${index}`">
            <b-card
              no-body
              :class="{'d-inline-block mb-3' : true, 'float-left':message.sender==investor.id, 'float-right':message.sender==currentUser.id}"
            >
              <div class="position-absolute pt-1 pr-2 r-0">
                <span class="text-extra-small text-muted">{{message.time}}</span>
              </div>
              <b-card-body>
                <div class="d-flex flex-row pb-1" v-if="message.sender==currentUser.id">
                  <img
                    :alt="currentUser.name"
                    :src="currentUser.image"
                    class="img-thumbnail border-0 rounded-circle mr-3 list-thumbnail align-self-center xsmall"
                  >
                  <div class="d-flex flex-grow-1 min-width-zero">
                    <div
                      class="m-2 pl-0 align-self-center d-flex flex-column flex-lg-row justify-content-between min-width-zero"
                    >
                      <div class="min-width-zero">
                        <p class="mb-0 truncate list-item-heading">{{currentUser.name}}</p>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="d-flex flex-row pb-1" v-else>
                  <img
                    :alt="investor.name"
                    :src="investor.image"
                    class="img-thumbnail border-0 rounded-circle mr-3 list-thumbnail align-self-center xsmall"
                  >
                  <div class="d-flex flex-grow-1 min-width-zero">
                    <div
                      class="m-2 pl-0 align-self-center d-flex flex-column flex-lg-row justify-content-between min-width-zero"
                    >
                      <div class="min-width-zero">
                        <p class="mb-0 truncate list-item-heading">{{investor.name}}</p>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="chat-text-left">
                  <p class="mb-0 text-semi-muted">{{message.text}}</p>
                </div>
              </b-card-body>
            </b-card>
            <div class="clearfix"/>
          </div>
        </vue-perfect-scrollbar>
      </b-colxx>
    </b-row>
    <div class="chat-input-container d-flex justify-content-between align-items-center">
      <b-form-textarea v-model="message" placeholder="Email content..." rows="6"></b-form-textarea>
      <b-button-group vertical>
        <b-button class="mb-2" size="sm" variant="primary rounded" v-b-modal.templatemodal>
          <i class="simple-icon-badge"/> Use Template
        </b-button>
        <b-button class="mb-2" size="sm" variant="secondary rounded">
          <i class="iconsminds-diploma-2"/> Add Pitch Deck
        </b-button>
        <b-button class="mb-2" size="sm" variant="success rounded">
          <i class="iconsminds-mail-send"/> Send
        </b-button>
      </b-button-group>
    </div>
    <application-menu>
      <vue-perfect-scrollbar :settings="{ suppressScrollX: true, wheelPropagation: false }">
        <div class="p-4">
          <div class="d-flex flex-row">
            <div class="d-flex">
              <img
                :alt="investor.name"
                :src="investor.image"
                class="img-thumbnail border-0 rounded-circle ml-0 mr-4 list-thumbnail align-self-center small"
              >
            </div>
            <div class="align-items-lg-center">
              <router-link :to="`?p=${investor.id}`">
                <span class="list-item-heading truncate">{{investor.name}}</span>
                <b-badge pill variant="primary">{{ investor.type }}</b-badge>
              </router-link>
              <p class="text-muted">
                <i class="simple-icon-location-pin"></i>
                {{investor.location}}
              </p>
              <div class="mb-4">
                <a target="_blank" :href="investor.website" class="btn icon-button btn-secondary">
                  <i class="simple-icon-link"></i>
                </a>
                <a target="_blank" :href="investor.linkedin" class="btn icon-button btn-secondary">
                  <i class="simple-icon-social-linkedin"></i>
                </a>
                <a target="_blank" :href="investor.twitter" class="btn icon-button btn-secondary">
                  <i class="simple-icon-social-twitter"></i>
                </a>
              </div>
            </div>
          </div>
          <h5>
            <i class="simple-icon-chart"></i>
            {{ investor.industry }}
          </h5>
          <p>{{investor.comments}}</p>
          <p class="text-muted text-small mt-4 mb-2">Activity Overview</p>
          <ul class="list-unstyled mb-4">
            <li class="nav-item">
              <i class="iconsminds-mail-send"/>
              Emails Sent
              <span class="float-right">5</span>
            </li>
            <li class="nav-item">
              <i class="iconsminds-speach-bubbles"/>
              Emails Answered
              <span class="float-right">2</span>
            </li>
            <li class="nav-item">
              <i class="iconsminds-diploma-2"/>
              Pitchdeck Viewed
              <span class="float-right">2</span>
            </li>
          </ul>
        </div>
      </vue-perfect-scrollbar>
    </application-menu>
    <b-modal id="templatemodal" ref="templatemodal" title="Choose Template">
      <resize-observer @notify="handleSwiperSingleResize"/>
      <swiper :options="swiperSingleOption" ref="swiperSingle">
        <swiper-slide>
          <div class="pr-3 pl-3">
            <b-card class="flex-row" no-body>
              Lorem ipsum dolor sit amet, consectetur adipisicing elit,
              sed do eiusmod tempor incididunt ut labore et dolore magna
              aliqua. Ut enim ad minim veniam, quis nostrud exercitation
              ullamco laboris nisi ut aliquip ex ea commodo consequat.
              Duis aute irure dolor in reprehenderit in voluptate velit
              esse cillum dolore eu fugiat nulla pariatur. Excepteur
              sint occaecat cupidatat non proident, sunt in culpa qui
              officia deserunt mollit anim id est laborum.
            </b-card>
          </div>
        </swiper-slide>
        <swiper-slide>
          <div class="pr-3 pl-3">
            <b-card class="flex-row" no-body>
              Hi Mike!
              Duis aute irure dolor in reprehenderit in voluptate velit
              esse cillum dolore eu fugiat nulla pariatur. Excepteur
              sint occaecat cupidatat non proident, sunt in culpa qui
              officia deserunt mollit anim id est laborum.
            </b-card>
          </div>
        </swiper-slide>
      </swiper>
      <div class="text-center slider-nav">
        <button class="left-arrow btn btn-link swiper-single-prev">
          <i class="simple-icon-arrow-left"></i>
        </button>
        <div class="slider-dot-container swiper-single-nav"></div>
        <button class="left-arrow btn btn-link swiper-single-next">
          <i class="simple-icon-arrow-right"></i>
        </button>
      </div>
      <template slot="modal-footer">
        <b-button variant="primary" @click="hideModal('templatemodal')" class="mr-1">Use Template</b-button>
        <b-button variant="secondary" @click="hideModal('templatemodal')">Cancel</b-button>
      </template>
    </b-modal>
  </div>
</template>
<script>
import { mapGetters, mapActions } from "vuex";
import "swiper/dist/css/swiper.css";
import { swiper, swiperSlide } from "vue-awesome-swiper";
import { ResizeObserver } from "vue-resize";
import ApplicationMenu from "@/components/Common/ApplicationMenu";
import axios from "axios";
import { apiUrl } from "@/constants/config";

export default {
  components: {
    ApplicationMenu,
    swiper,
    swiperSlide,
    ResizeObserver
  },
  data() {
    return {
      tabIndex: 0,
      message: "",
      searchKey: "",
      currentUser: {
        image: "/assets/img/profile-pic-l.jpg",
        name: "Mark Zuckerberg",
        location: "San Francisco, USA",
        id: 5
      },
      isLoadCurrentConversation: true,
      investor: {
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
        deck: "Viewed",
        id: 1
      },
      messages: [
        {
          sender: 1,
          time: "09:25",
          text: "What do you think about our plans for this product launch?"
        },
        {
          sender: 5,
          time: "09:29",
          text:
            "It looks to me like you have a lot planned before your deadline. I would suggest you push your deadline back so you have time to run a successful advertising campaign."
        },
        {
          sender: 5,
          time: "09:36",
          text:
            "How do you think we should move forward with this project? As you know, we are expected to present it to our clients next week."
        },
        {
          sender: 1,
          time: "10:12",
          text:
            "I would suggest you discuss this further with the advertising team."
        },
        {
          sender: 5,
          time: "10:30",
          text:
            "I am very busy at the moment and on top of everything, I forgot my umbrella today."
        }
      ],
      swiperSingleOption: {
        slidesPerView: 1,
        loop: true,
        pagination: {
          el: ".swiper-single-nav",
          clickable: true,
          bulletClass: "slider-dot",
          bulletActiveClass: "active",
          dynamicBullets: true,
          renderBullet(index, className) {
            return `<button class="${className}"></button>`;
          }
        },
        navigation: {
          nextEl: ".swiper-single-next",
          prevEl: ".swiper-single-prev"
        }
      }
    };
  },
  computed: {
    ...mapGetters([
      "isLoadContacts",
      "isLoadConversations",
      "error",
      "contacts",
      "contactsSearchResult",
      "conversations"
    ])
  },
  methods: {
    hideModal(refname) {
      this.$refs[refname].hide();
      console.log("hide modal:: " + refname);

      if (refname === "modalnestedinline") {
        this.$refs["modalnested"].show();
      }
    },
    somethingModal(refname) {
      this.$refs[refname].hide();
      console.log("something modal:: " + refname);

      if (refname === "modalnestedinline") {
        this.$refs["modalnested"].show();
      }
    },
    handleSwiperSingleResize() {
      this.$refs.swiperSingle.update();
    },
    getConversations({ commit }, userId) {
      axios.get(`${apiUrl}/conversations`).then(r => {
        this.messages = r.data;
      });
    },
    selectConversation(data, messages) {
      this.data = data;
      this.messages = messages;
    },
    selectContact(userId) {
      this.data = this.contacts.find(x => x.id === userId);
      const conversation = this.conversations.find(
        x => x.users.includes(userId) && x.users.includes(this.currentUser.id)
      );
      if (conversation) {
        console.log("change selected conversation");
        this.messages = conversation.messages;
      } else {
        console.log("create new conversation");
        const date = new Date();
        this.conversations.splice(0, 0, {
          users: [userId, this.currentUser.id],
          messages: [],
          lastMessageTime: date.getHours() + ":" + date.getMinutes()
        });
        this.messages = [];
      }
      this.tabIndex = 0;
      this.message = "";
      this.searchKey = "";
    },
    sendMessage() {
      console.log("add message to conversation");
      const date = new Date();
      this.messages.push({
        sender: this.currentUser.id,
        text: this.message,
        time: date.getHours() + ":" + date.getMinutes()
      });
      this.message = "";
    }
  },
  mounted() {
    //this.getConversations(this.currentUser.id);
  }
};
</script>
