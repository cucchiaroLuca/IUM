<template>
  <section class="cardContainer">
    <div class="cardHeader">
      <p class="bold nameContainer">
        NAME: <span class="normal">{{ mName }}</span>
      </p>

      <div class="subCardHeader space">
        <div class="expireType">
          <p class="bold">
            EXPIRE: <span class="normal">{{ expireDs }} - </span>
            <span id="expireSpan">{{ expireI }} DAYS</span>
          </p>

          <div class="space">
            <p class="bold">
              ESTIMATED TIME:

              <span class="normal days" :style="checkEstimatedTime"
                >{{ estimated }} days</span
              >
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

      <div class="spentOnContainer space" v-if="mSpentKey">
        <p class="bold">How long did it take?</p>
        <span class="inputBanner" v-if="inputKey">Please enter a value other than zero</span>
        <div class="spentOnInput">
          <input type="number" class="space" v-model="inputValue" />
          <p class="days">days</p>
        </div>
      </div>

      <div class="btnContainer">
        <button class="blackBtn" @click="changeDscText" v-if="!mSpentKey">
          {{ descriptionBtnText }}
        </button>
        <button class="yellowBtn" @click="changeTimeKey" v-else>BACK</button>

        <button class="greenBtn" @click="changeTimeKey" v-if="!mSpentKey">
          {{ doneBtnText }}
        </button>
        <button class="greenBtn" @click="timeSpent" v-else>CONFIRM</button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "Done",

  props: {
    details: Boolean,
    tag: String,
    mName: String,
    expireD: Date,
    expireDs: String,
    expireI: Number,
    asset: String,
    mDescription: String,
    estimated: Number,
    similar: String,
    mAppointee: String,
    mResponsible: String,
    mSpentKey: Boolean,
    mSpentOn: Number,
    inputKey: Boolean,
  },

  data() {
    return {
      inputValue: 0,
    };
  },

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

    //DONE
    descriptionBtnText() {
      return this.details ? "SEE LESS" : "SEE MORE";
    },

    //DONE
    doneBtnText() {
      return this.mSpentKey ? "CONFIRM" : "DONE";
    },

    //DONE
    checkEstimatedTime() {
      if (this.estimated >= this.expireI) {
        return { color: "red" };
      } else {
        return { color: "green" };
      }
    },
  },

  methods: {
    //DONE
    changeDscText() {
      this.$emit("change-dsc", {
        machineId: this.tag,
        machineDetail: this.details,
      });
    },

    //DONE
    changeTimeKey() {
      this.$emit("change-time", {
        machineId: this.tag,
        machineTimeSpentKey: this.mSpentKey,
      });
    },

    //DONE
    timeSpent() {
      if (this.inputValue == "") {
        this.$emit("change-input-key", {
          machineId: this.tag,
          inputK: this.validInput,
        })

      } else if (typeof this.mSpentOn === "number") {
        this.$emit("time-spent-on", {
          machineId: this.tag,
          inputV: this.inputValue,
          mName: this.mName,
          mDescription: this.mDescription,
          expireD: this.expireD,
          expireDtS: this.expireDtS,
          expireI: this.expireI,
          asset: this.asset,
          estimated: this.estimated,
          similar: this.similar,
          mAppointee: this.mAppointee,
          mResponsible: this.mResponsible,
          mSpentKey: this.mSpentKey,
          mSpentOn: this.mSpentOn,
          inputKey: this.inputKey,
        });
      }
    },

    //DONE
    updateExpireDay() {
      this.$emit("expire-format", {
        expireDate: this.expireD,
        expireIn: this.expireI,
      });
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

.inputBanner {
  margin-top: 1vh;
  font-size: 13px;
  color: red;
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
