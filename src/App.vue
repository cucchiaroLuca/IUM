<template>
  <header>
    <div class="dateContainer">
      <p class="bold">
        Today: <span class="normal">{{ currentDateTime() }}</span>
      </p>
    </div>
  </header>

  <section class="cardsContainer">
    <div class="labelContainer">
      <h3>TASKS TO DO:</h3>
    </div>

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
      @change-dsc="changeDsc"
      @change-time="changeTime"
      @time-spent-on="timeSpentOn"
      @change-input-key="changeInputKey"
    ></card>
  </section>

  <section class="cardsContainer">
    <div class="doneLabelContainer">
      <h3>TASKS DONE:</h3>
    </div>

    <done
      v-for="doneCard in doneCards"
      :key="doneCard.machineTag"
      :details="doneCard.moreDetails"
      :tag="doneCard.machineTag"
      :mName="doneCard.name"
      :expireD="doneCard.expireDate"
      :expireDs="doneCard.expireDateString"
      :expireI="doneCard.expireIn"
      :asset="doneCard.assetType"
      :mDescription="doneCard.description"
      :estimated="doneCard.estimatedTime"
      :similar="doneCard.similarMachine"
      :mAppointee="doneCard.appointee"
      :mResponsible="doneCard.responsible"
      :mSpentKey="doneCard.timeSpentKey"
      :mSpentOn="doneCard.timeSpentOn"
      :inputKey="doneCard.validInput"
      @change-dsc="changeDsc"
      @change-time="changeTime"
      @time-spent-on="timeSpentOn"
      @change-input-key="changeInputKey"
    ></done>
  </section>
</template>

<script>
import Card from "./components/Card.vue";
import Done from "./components/Done.vue";

export default {
  name: "App",
  components: {
    Card,
    Done,
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

    changeDsc(mObject) {
      this.index = this.cards.findIndex(
        (element) => element.machineTag === mObject.machineId
      );
      mObject.machineDetail = !mObject.machineDetail;
      this.cards[this.index].moreDetails = mObject.machineDetail;
    },

    changeTime(mObject) {
      this.index = this.cards.findIndex(
        (element) => element.machineTag === mObject.machineId
      );
      mObject.machineTimeSpentKey = !mObject.machineTimeSpentKey;
      this.cards[this.index].timeSpentKey = mObject.machineTimeSpentKey;
    },

    timeSpentOn(mObject) {
      this.index = this.cards.findIndex(
        (element) => element.machineTag === mObject.machineId
      );

      this.cards[this.index].timeSpentOn = mObject.inputV;
      this.doneCards.push(this.cards[this.index]);

      this.cards.splice(this.index, 1);
    },

    changeInputKey(mObject) {
      this.index = this.cards.findIndex(
        (element) => element.machineTag === mObject.machineId
      );

      this.cards[this.index].validInput = true;
    },
  },
};
</script>

<style>
:root {
  --primaryColor: #181818;
  --lightGray: #ececec;
  --customYellow: #ffbf5f;
  --vehicleColor: #dd4cf4;
  --machineColor: #1c5dbe;
  --eletronicsColor: #d75e2b;
  --customGreen: #009217;
}

* {
  margin: 0;
  border: 0;
}

html {
  font-family: "Jost", sans-serif;
}

.bold {
  font-weight: bold;
}

.normal {
  font-weight: normal;
}

.space {
  margin-top: 2vh;
}

#app {
  display: flex;
  flex-flow: column;
  align-items: center;
  justify-content: center;
}

header {
  width: 100%;
  height: 10vh;
  position: sticky;
  top: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: var(--primaryColor);
}

.dateContainer {
  width: 50%;
  height: 70%;
  display: flex;
  flex-flow: row;
  align-items: center;
  justify-content: center;
  border-radius: 20px;
  background: white;
}

.cardsContainer {
  width: 90%;
  padding-bottom: 3vh;
  display: flex;
  flex-flow: column;
  align-items: center;
  justify-content: center;
}

.labelContainer {
  width: 100%;
  height: 8vh;
  margin-top: 3vh;
  display: flex;
  align-items: center;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  background-color: var(--customYellow);
}

.labelContainer h3,
.doneLabelContainer h3 {
  margin-left: 2vh;
}

.doneLabelContainer {
  width: 100%;
  height: 8vh;
  display: flex;
  align-items: center;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  color: white;
  background-color: var(--customGreen);
}

@media only screen and (min-width: 900px) {
  .dateContainer {
    width: 30%;
  }

  .cardsContainer {
    flex-flow: row wrap;
    align-items: flex-start;
    justify-content: space-around;
  }

  .cardContainer {
    width: 30%;
  }

  #machineType {
    width: auto;
  }
}
</style>
