<template>
  <div>
    <q-layout
      view="lHh lpr lFf"
      container
      style="height: 100vh"
      class="shadow-2 rounded-borders"
    >
      <q-header bordered class="text-primary ghostWhite">
        <q-toolbar style="padding: 10px 32px">
          <q-toolbar-title
            class="flex wrap items-center justify-between header"
          >
            <img
              style="width: 150px; height: auto"
              src="./images/logo.png"
              alt="plastic technologies logo"
            />
            <div
              class="flex row items-center justify-center headerSb"
            >
              <p class="text-body1 text-center text-bold text-italic formatSp">
                Today: {{ currentDateTime() }}
              </p>
              <!-- <span class="material-icons" style="font-size: 28px; margin-left: 1vh;"> today </span> -->
            </div>
          </q-toolbar-title>
        </q-toolbar>
      </q-header>

      <q-footer bordered class="ghostWhite text-primary flex column items-center justify-center">
        <div class="flex items-center justify-center text-body1 text-bold text-italic footerDataCt" style="width: 100%; padding: 10px 0; border-bottom: 1px solid lightgray">
          Today: {{ currentDateTime() }}
        </div>
        
        <q-tabs
          no-caps
          active-color="primary"
          indicator-color="transparent"
          class="text-grey"
          v-model="tab"
        >
          <div class="flex column items-center justify-center">
            <q-tab name="cards" label="Tasks">
              <span class="material-icons headerIcon"> calendar_month </span>
            </q-tab>
          </div>

          <div class="flex column items-center justify-center">
            <q-tab name="done" label="Done">
              <span class="material-icons headerIcon"> event_available </span>
            </q-tab>
          </div>
        </q-tabs>
      </q-footer>

      <q-page-container>
        <q-page class="q-pa-md">
          <!-- TASKS TO DO -->
          <div id="tasks" v-if="tab == 'cards'">
            <card
              v-for="card in cards"
              :key="card.machineTag"
              :details="card.moreDetails"
              :tag="card.machineTag"
              :mName="card.name"
              :expireD="card.expireDate"
              :expireDs="card.expireDateString"
              :expireI="card.expireIn"
              :asset="card.assetType"
              :mDescription="card.description"
              :estimated="card.estimatedTime"
              :similar="card.similarMachine"
              :mAppointee="card.appointee"
              :mResponsible="card.responsible"
              :mSpentKey="card.timeSpentKey"
              :mSpentOn="card.timeSpentOn"
              :inputKey="card.validInput"
              :done="card.dnCard"
              @change-time="changeTime"
              @time-spent-on="timeSpentOn"
              @change-input-key="changeInputKey"
              @reset-input-status="resetInputStatus"
            ></card>
          </div>

          <!-- DONE TASKS -->
          <div id="doneTasks" v-else>
            <done
              v-for="done in doneCards"
              :key="done.machineTag"
              :details="done.moreDetails"
              :tag="done.machineTag"
              :mName="done.name"
              :expireD="done.expireDate"
              :expireDs="done.expireDateString"
              :expireI="done.expireIn"
              :asset="done.assetType"
              :mDescription="done.description"
              :estimated="done.estimatedTime"
              :similar="done.similarMachine"
              :mAppointee="done.appointee"
              :mResponsible="done.responsible"
              :mSpentKey="done.timeSpentKey"
              :mSpentOn="done.timeSpentOn"
              :inputKey="done.validInput"
              :done="done.dnCard"
              @change-time="changeTime"
              @time-spent-on="timeSpentOn"
              @change-input-key="changeInputKey"
              @reset-input-status="resetInputStatus"
            ></done>
          </div>
        </q-page>
      </q-page-container>
    </q-layout>
  </div>
</template>

<script>
import { ref } from "vue";
import Card from "./components/Card.vue";
import Done from "./components/Done.vue";

export default {
  name: "App",
  components: {
    Card,
    Done,
  },

  setup() {
    return {
      tab: ref("cards"),
    };
  },

  data() {
    return {
      currentDay: new Date(),
      cards: [
        {
          moreDetails: false,
          machineTag: "3894",
          name: "Name1",
          expireDate: new Date(2021, 11, 13),
          expireDateString: new Date(2021, 11, 13).toDateString(),
          expireIn: Number,
          assetType: "Vehicle",
          description:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam efficitur molestie erat vitae tempor. Curabitur vel nisl in mi pretium venenatis. Etiam vel turpis ultricies, fermentum quam id, iaculis sapien. Phasellus quis ex vel nunc fermentum tristique.",
          estimatedTime: 5,
          similarMachine: "#0923",
          appointee: "Frankie Osusu",
          responsible: "Luigi Praticò",
          timeSpentKey: false,
          timeSpentOn: 0,
          dnCard: false,
          validInput: false,
        },
        {
          moreDetails: false,
          machineTag: "8465",
          name: "Name2",
          expireDate: new Date(2021, 11, 18),
          expireDateString: new Date(2021, 11, 18).toDateString(),
          expireIn: Number,
          assetType: "Machine",
          description:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam efficitur molestie erat vitae tempor. Curabitur vel nisl in mi pretium venenatis. Etiam vel turpis ultricies, fermentum quam id, iaculis sapien. Phasellus quis ex vel nunc fermentum tristique.",
          estimatedTime: 3,
          similarMachine: "#0989",
          appointee: "Frankie Osusu",
          responsible: "Luigi Praticò",
          timeSpentKey: false,
          timeSpentOn: 0,
          dnCard: false,
          validInput: false,
        },
        {
          moreDetails: false,
          machineTag: "8305",
          name: "Name3",
          expireDate: new Date(2021, 11, 23),
          expireDateString: new Date(2021, 11, 23).toDateString(),
          expireIn: Number,
          assetType: "Eletronics",
          description:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam efficitur molestie erat vitae tempor. Curabitur vel nisl in mi pretium venenatis. Etiam vel turpis ultricies, fermentum quam id, iaculis sapien. Phasellus quis ex vel nunc fermentum tristique.",
          estimatedTime: 30,
          similarMachine: "#0900",
          appointee: "Frankie Osusu",
          responsible: "Luigi Praticò",
          timeSpentKey: false,
          timeSpentOn: 0,
          dnCard: false,
          validInput: false,
        },
      ],
      doneCards: [],
    };
  },

  created: function () {
    this.updateDates();
  },

  methods: {
    updateDates() {
      for (this.i = 0; this.i < this.cards.length; this.i++) {
        this.expireDateFormat = new Date(this.cards[this.i].expireDate);

        this.oneDay = 24 * 60 * 60 * 1000;
        this.firstDate = this.currentDay;
        this.secondDate = this.cards[this.i].expireDate;

        this.diffDays = Math.round(
          Math.abs((this.firstDate - this.secondDate) / this.oneDay)
        );

        this.cards[this.i].expireIn = this.diffDays;
      }
    },

    currentDateTime() {
      const current = new Date();
      const date =
        current.getDate() +
        "/" +
        (current.getMonth() + 1) +
        "/" +
        current.getFullYear();

      this.currentDay = date;

      return this.currentDay;
    },

    findObj(mObject) {
      this.index = this.cards.findIndex(
        (element) => element.machineTag === mObject.machineId
      );

      return this.cards[this.index];
    },

    changeTime(mObject) {
      this.findObj(mObject);

      mObject.machineTimeSpentKey = !mObject.machineTimeSpentKey;
      this.cards[this.index].timeSpentKey = mObject.machineTimeSpentKey;
    },

    timeSpentOn(mObject) {
      this.findObj(mObject);

      this.cards[this.index].timeSpentOn = mObject.inputV;

      mObject.cardSt = !mObject.cardSt;
      this.cards[this.index].dnCard = mObject.cardSt;
      this.doneCards.push(this.cards[this.index]);

      this.cards.splice(this.index, 1);

      console.log(this.cards);
      console.log(this.doneCards);
    },

    changeInputKey(mObject) {
      this.findObj(mObject);

      this.cards[this.index].validInput = true;
    },

    resetInputStatus(mObject) {
      this.findObj(mObject);

      this.cards[this.index].validInput = false;
    },
  },
};
</script>
