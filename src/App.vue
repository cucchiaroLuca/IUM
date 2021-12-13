<template>
  <header>
    <div class="dateContainer">
      <p class="bold">
        Today: <span class="normal">{{ currentDateTime() }}</span>
      </p>
    </div>
  </header>

  <div class="labelContainer">
    <h3>TASKS TO DO:</h3>
  </div>

  <section class="cardsContainer">
    <card
    v-for="card in cards"
    :key="card.machineTag"
    :details="card.moreDetails"
    :tag="card.machineTag"
    :mName="card.name"
    :expireD="card.expireDate"
    :expireI="card.expireIn"
    :asset="card.assetType"
    :mDescription="card.description"
    :estimated="card.estimatedTime"
    :similar="card.similarMachine"
    :mAppointee="card.appointee"
    :mResponsible="card.responsible"
    :mSpentKey="card.timeSpentKey"
    :mSpentOn="card.timeSpentOn"
    @change-dsc="changeDsc"
    ></card>
  </section>

  <div class="doneLabelContainer">
    <h3>TASKS DONE:</h3>
  </div>
</template>

<script>
import Card from "./components/Card.vue";

export default {
  name: "App",
  components: {
    Card,
  },

  data() {
    return {
      currentDay: Date,
      cards: [
        {
          moreDetails: false,
          machineTag: "3894",
          name: "Name1",
          expireDate: new Date(2021, 11, 13).toDateString(),
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
        },
        {
          moreDetails: false,
          machineTag: "8465",
          name: "Name2",
          expireDate: new Date(2021, 11, 18).toDateString(),
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
        },
        {
          moreDetails: false,
          machineTag: "8305",
          name: "Name3",
          expireDate: new Date(2021, 11, 23).toDateString(),
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
        },
      ],
      doneCards: [],
    };
  },

  methods: {
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
      console.log(mObject);

      
    }
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
  width: 90%;
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
  width: 90%;
  height: 8vh;
  display: flex;
  align-items: center;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  color: white;
  background-color: var(--customGreen);
}

@media only screen and (min-width: 768px) {
  .dateContainer {
    width: 30%;
  }

  .cardsContainer {
    flex-flow: row wrap;
    align-items: flex-start;
    justify-content: space-between;
  }

  .cardContainer {
    width: 30%;
  }

  #machineType {
    width: auto;
  }
}
</style>
