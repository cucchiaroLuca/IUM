<template>
  <div class="q-pa-md row items-start q-gutter-md cardContainer">
    <q-card class="my-card cardShadow" bordered>
      <q-card-section class="sectionCardPadding">
        <div class="warningCtn">
          <span class="material-icons warningIcon" v-if="expireI === -1">
            error_outline
          </span>
          <q-tooltip> Tasks is expired! </q-tooltip>
        </div>
        <div class="text-body2" :style="checkTagColor">{{ asset }}</div>
        <div class="text-h4 q-mt-sm q-mb-xs" style="margin-bottom: 2vh">
          {{ mName }}
        </div>
        <div class="text-subtitle1">
          <p class="bold space10">
            EXPIRE: <span class="normal">{{ expireDs }} - </span>
            <span id="expireSpan" v-if="expireI === -1">(EXPIRED)</span>
            <span id="expireSpan" v-else>({{ expireI }} DAYS)</span>
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

      <q-card-section
        class="row justify-start items-start wrap"
        v-if="mSpentKey"
      >
        <div>
          <p class="bold text-h6 formatSp" style="margin-bottom: 1vh">
            How long did it take?
          </p>

          <p
            class="bold text-subtitle2 text-red"
            style="margin-bottom: 1vh"
            v-if="inputKey"
          >
            Please enter a value other than zero
          </p>

          <div class="row justify-start items-baseline text-body1">
            <q-input
              type="number"
              label="Time spent on"
              :dense="dense"
              v-model="inputValue"
            />
            <span class="daysInput">days</span>
          </div>
        </div>
      </q-card-section>

      <div class="flex row" style="padding: 8px 0; margin-top: 8px">
        <q-btn
          flat
          color="dark"
          label="Docs"
          class="btnDocs formatSp"
          v-if="!mSpentKey"
          @click="downloadFile"
        />

        <q-btn-dropdown
          flat
          color="primary"
          label="Details"
          class="formatSp btnDetails"
          v-if="!mSpentKey"
          @click="expanded = !expanded"
        />

        <q-space />

        <q-btn
          flat
          color="dark"
          label="Back"
          @click="
            changeTimeKey();
            resetInputStatus();
          "
          v-if="mSpentKey"
        />

        <q-btn
          color="primary"
          label="Done"
          class="btnDone"
          @click="changeTimeKey"
          v-if="!mSpentKey"
        />

        <q-btn
          color="primary"
          label="Confirm"
          class="btnConfirm"
          @click="
            timeSpent();
            setCardStatus();
          "
          v-else
        />
      </div>

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
      dense: "",
      expanded: ref(false),
      left: ref(false),
    };
  },

  name: "Card",

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
    done: Boolean,
  },

  emits: [
    "change-time",
    "change-input-key",
    "time-spent-on",
    "set-card-status",
    "download-file",
  ],

  data() {
    return {
      inputValue: "",
    };
  },

  computed: {
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

    checkEstimatedTime() {
      if (this.estimated >= this.expireI) {
        return { color: "red" };
      } else {
        return { color: "green" };
      }
    },
  },

  methods: {
    changeTimeKey() {
      this.$emit("change-time", {
        machineId: this.tag,
        machineTimeSpentKey: this.mSpentKey,
      });
    },

    timeSpent() {
      if (this.inputValue == "" || this.inputValue == "0") {
        this.inputValue = "";
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
          done: this.done,
        });
      }
    },

    setCardStatus() {
      this.$emit("set-card-status", {
        machineId: this.tag,
        cardSt: this.done,
      });
    },

    resetInputStatus() {
      this.$emit("reset-input-status", {
        machineId: this.tag,
      });
    },

    downloadFile() {
      this.$emit("download-file", {
        machineId: this.tag,
        asset: this.asset,
      });
    },
  },
};
</script>