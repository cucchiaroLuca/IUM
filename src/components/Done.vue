<template>
  <!-- <section class="cardContainer">
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
  </section> -->

  <div class="q-pa-md row items-start q-gutter-md cardContainer">
    <q-card class="my-card cardShadow" bordered>
      <q-card-section class="sectionCardPadding">
        <div class="text-body2" :style="checkTagColor">{{ asset }}</div>
        <div class="text-h4 q-mt-sm q-mb-xs" style="margin-bottom: 2vh">
          {{ mName }}
        </div>
        <div class="text-subtitle1">
          <p class="bold space10">
            EXPIRE: <span class="normal">{{ expireDs }} - </span>
            <span id="expireSpan">{{ expireI }} DAYS</span>
          </p>

          <p class="bold space10">
            ESTIMATED TIME:
            <span class="normal days" :style="checkEstimatedTime"
              >{{ estimated }} days</span
            >
          </p>
        </div>

        <div class="text-body1 text-grey q-btn--actionable description">
          {{ mDescription }}
        </div>
      </q-card-section>

      <q-card-actions class="justify-end doneCardAction">
        <div
          class="row justify-end items-center q-btn--actionable"
          @click="expanded = !expanded"
        >
          DETAILS
          <q-btn
            color="grey"
            round
            flat
            dense
            :icon="expanded ? 'keyboard_arrow_up' : 'keyboard_arrow_down'"
          />
        </div>
      </q-card-actions>

      <q-slide-transition>
        <div v-show="expanded">
          <q-separator />
          <q-card-section class="text-body2">
            <p class="bold">
              SIMILAR MACHINE: <span class="normal">{{ similar }}</span>
            </p>
            <p class="bold">
              APPOINTEE: <span class="normal">{{ mAppointee }}</span>
            </p>
            <p class="bold formatSp">
              RESPONSIBLE: <span class="normal">{{ mResponsible }}</span>
            </p>
          </q-card-section>
        </div>
      </q-slide-transition>
    </q-card>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    return {
      expanded: ref(false),
      lorem:
        "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
    };
  },

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
      inputValue: "",
    };
  },

  computed: {
    //DONE
    checkTagColor() {
      if (this.asset === "Vehicle") {
        return { color: "var(--vehicleColor)", fontWeight: "bold" };
      } else if (this.asset === "Machine") {
        return { color: "var(--machineColor)", fontWeight: "bold" };
      } else if (this.asset === "Eletronics") {
        return {
          color: "var(--eletronicsColor)",
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
        });
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

<style lang="scss"></style>
