<template>
  <section class="cardContainer">
    <div class="cardHeader">
      <p class="bold nameContainer">
        NAME: <span class="normal">{{ mName }}</span>
      </p>

      <div class="subCardHeader space">
        <div class="expireType">
          <p class="bold">
            EXPIRE: <span class="normal">{{ expireI }} - </span>
            <!-- <span id="expireSpan">{{ updateExpireDay(card) }} DAYS</span> -->
          </p>

          <div class="space">
            <p class="bold">
              ESTIMATED TIME:
              <!-- <span class="normal days" :style="checkEstimatedTime(card)"
                >{{ card.estimatedTime }} days</span
              > -->
            </p>
          </div>
        </div>
      </div>

      <span id="machineType" class="assetType space" :style="checkTagColor">{{
        asset
      }}</span>
    </div>

    <div class="cardDescription">
      <p>{{ mDescription }}</p>

      <div class="moreDetails" v-if="details">
        <p class="bold">
          SIMILAR MACHINE: <span class="normal">{{ similar }}</span>
        </p>
        <p class="bold">
          APPOINTEE: <span class="normal">{{ mAppointee }}</span>
        </p>
        <p class="bold">
          RESPONSIBLE: <span class="normal">{{ mResponsible }}</span>
        </p>
      </div>

      <!-- <div class="spentOnContainer space" v-if="card.timeSpentKey"> -->
      <div class="spentOnContainer space">
        <p class="bold">How long did it take?</p>
        <div class="spentOnInput">
          <!-- <input type="number" class="space" v-model="card.timeSpentOn" /> -->
          <p class="days">days</p>
        </div>
      </div>

      <div class="btnContainer">
        <!-- <button
          class="blackBtn"
          @click="changeDescText(card)"
          v-if="!card.timeSpentKey"
        >
          {{ descriptionBtnText(card) }}
        </button>
        <button class="yellowBtn" @click="changeTimeKey(card)" v-else>
          BACK
        </button>

        <button
          class="greenBtn"
          @click="changeTimeKey(card)"
          v-if="!card.timeSpentKey"
        >
          DONE
        </button>
        <button class="greenBtn" @click="setTimeSpentOn(card)" v-else>
          CONFIRM
        </button> -->

        <button class="blackBtn" @click="changeDscText" v-if="!mSpentKey">DIO *****</button>
        <button class="yellowBtn" v-else>BACK</button>

        <button class="greenBtn" v-if="!mSpentKey">DONE DIO *****</button>
        <button class="greenBtn" v-else>CONFIRM</button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "Card",

  props: {
    details: Boolean,
    tag: String,
    mName: String,
    expireD: Date,
    expireI: Number,
    asset: String,
    mDescription: String,
    estimated: Number,
    similar: String,
    mAppointee: String,
    mResponsible: String,
    mSpentKey: Boolean,
    mSpentOn: Number,
  },

  data() {
    return {
      today: new Date(),
    };
  },

  watch: {},

  computed: {
    //DONE
    checkTagColor() {
      if (this.asset === "Vehicle") {
        return { backgroundColor: "var(--vehicleColor)", fontWeight: "bold" };
      } else if (this.asset === "Machine") {
        return { backgroundColor: "var(--machineColor)", fontWeight: "bold" };
      } else if (this.asset === "Eletronics") {
        return {
          backgroundColor: "var(--eletronicsColor)",
          fontWeight: "bold",
        };
      } else {
        return false;
      }
    },
  },

  methods: {
    checkEstimatedTime(card) {
      if (card.estimatedTime >= card.expireIn) {
        return { color: "red" };
      } else {
        return { color: "green" };
      }
    },

    descriptionBtnText(card) {
      return card.moreDetails ? "SEE LESS" : "SEE MORE";
    },

    changeDescText(card) {
      card.moreDetails = !card.moreDetails;
    },

    changeDscText() {
      this.$emit('change-dsc', {'machineId': this.tag, 'machineDetail': this.details});
    },

    updateExpireDay(card) {
      this.expireDateFormat = new Date(card.expireDate);
      this.oneDay = 24 * 60 * 60 * 1000;
      card.expireIn =
        Math.round(
          Math.abs((this.today - this.expireDateFormat) / this.oneDay)
        ) + 1;

      return card.expireIn;
    },

    changeTimeKey(card) {
      card.timeSpentKey = !card.timeSpentKey;
    },

    doneBtnText(card) {
      return card.timeSpentKey ? "CONFIRM" : "DONE";
    },

    setTimeSpentOn(card) {
      if (typeof card.timeSpentOn === "string") {
        console.log("Il valore inserito non è supportato =(");
      } else if (typeof card.timeSpentOn === "number") {
        this.cardIndex = this.cards.indexOf(card);
        this.cards.splice(this.cardIndex, 1);
        console.log(this.cardIndex);
      }
    },
  },
};
</script>

<style>
.cardContainer,
.doneCardsContainer {
  width: 100%;
  padding-top: 3vh;
  display: flex;
  flex-flow: column;
}

.cardHeader {
  padding: 2vh;
  border-radius: 10px 10px 0 0;
  border: 0.5px solid darkgray;
  background: var(--lightGray);
}

.subCardHeader {
  display: flex;
  flex-flow: column;
}

.expireType {
  display: flex;
  flex-flow: column;
}

.days {
  margin-left: 1vh;
}

#expireSpan {
  color: black;
}

#machineType {
  width: 20%;
  padding: 0.5vh 3vh;
  display: flex;
  flex-flow: row;
  align-items: center;
  justify-content: center;
  color: white;
}

.cardDescription {
  display: flex;
  flex-flow: column;
  padding: 3vh 2vh;
  border-radius: 0 0 10px 10px;
  border-bottom: 0.5px solid darkgray;
  border-left: 0.5px solid darkgray;
  border-right: 0.5px solid darkgray;
}

.moreDetails {
  margin-top: 1vh;
}

.moreDetails p {
  margin-top: 2vh;
}

.spentOnContainer {
  padding: 2vh 1vh;
  display: flex;
  flex-flow: column;
  align-items: center;
  justify-content: center;
  border-radius: 20px;
  background: var(--lightGray);
}

.spentOnInput {
  display: flex;
  flex-flow: row;
  align-items: baseline;
  justify-content: center;
}

.spentOnInput input {
  width: 50%;
  text-align: center;
  font-size: 15px;
  border-bottom: 2px solid #c1c1c1;
  color: #c1c1c1;
  background: transparent;
}

.btnContainer {
  margin-top: 2vh;
  display: flex;
  justify-content: space-between;
}

.blackBtn,
.greenBtn,
.yellowBtn {
  width: 49%;
  padding: 2vh 4vh;
  border-radius: 20px;
  font-weight: bold;
  font-style: italic;
}

.blackBtn {
  border: 2px solid white;
  color: white;
  background-color: var(--primaryColor);
}

.greenBtn {
  border: 2px solid var(--customGreen);
  color: var(--customGreen);
  background-color: white;
}

.yellowBtn {
  border: 2px solid var(--primaryColor);
  color: var(--primaryColor);
  background-color: var(--customYellow);
}

.doneLabel {
  width: 100%;
  height: 8vh;
  display: flex;
  align-items: center;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  color: white;
  background-color: var(--customGreen);
}

.doneLabel h3 {
  margin-left: 2vh;
}
</style>
