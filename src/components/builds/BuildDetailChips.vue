<template>
  <v-item-group :class="metaClass">
    <v-chip class="mr-2 mb-2" v-if="build.isDraft" label color="error" :size="chipSize"
      ><v-icon start icon="mdi-pencil-circle"></v-icon>Draft</v-chip
    >
    <v-chip
      class="mr-2 mb-2"
      v-if="isNew(build.timeCreated.toDate())"
      label
      color="accent"
      :size="chipSize"
      ><v-icon start icon="mdi-alert-decagram"></v-icon>NEW</v-chip
    >
    <v-chip
      class="mr-2 mb-2"
      v-if="showAuthorInMeta"
      label
      :size="chipSize"
      :to="{
        name: 'Builds',
        query: { author: build.authorUid },
      }"
      ><v-icon start icon="mdi-account-edit"></v-icon>{{ build.author }}</v-chip
    >
    <v-chip
      class="mr-2 mb-2"
      v-if="build.civ"
      label
      color="accent"
      :size="chipSize"
      :to="{ name: 'Builds', query: { civ: build.civ } }"
      clickable
    >
      <v-icon start icon="mdi-earth"></v-icon>
      {{ civLabelText }}
    </v-chip>
    <v-chip color="accent" class="mr-2 mb-2" v-if="build.season" label :size="chipSize"
      ><v-icon start icon="mdi-trophy"></v-icon>{{ build.season }}</v-chip
    >
    <v-chip
      class="mr-2 mb-2"
      v-if="build.creatorId"
      variant="plain"
      :size="chipSize"
      :href="'https://www.youtube.com/channel/' + build.creatorId"
      target="_blank"
      rel="noopener noreferrer"
      clickable
    >
      <v-icon color="accent" start icon="mdi-youtube"></v-icon>
      {{ build.creatorName }}
    </v-chip>
  </v-item-group>
  <v-item-group :class="mapClass">
    <v-chip class="mr-2 mb-2" color="accent" v-if="build.map" label :size="chipSize"
      ><v-icon start icon="mdi-map"></v-icon>{{ build.map }}</v-chip
    >
    <v-chip color="accent" class="mr-2 mb-2" v-if="build.strategy" label :size="chipSize"
      ><v-icon start icon="mdi-strategy"></v-icon>{{ build.strategy }}</v-chip
    >
  </v-item-group>
  <v-item-group :class="statsClass">
    <v-chip
      class="mr-2 mb-2"
      v-if="showAuthorInStats"
      label
      :size="chipSize"
      :to="{
        name: 'Builds',
        query: { author: build.authorUid },
      }"
      ><v-icon start icon="mdi-account-edit"></v-icon>{{ build.author }}</v-chip
    >
    <v-chip class="mr-2 mb-2" label :size="chipSize" v-show="build.views">
      <v-icon start icon="mdi-eye"></v-icon>{{ build.views }}</v-chip
    >
    <v-chip v-if="build.comments > 0" class="mr-2 mb-2" label :size="chipSize"
      ><v-icon start icon="mdi-message"></v-icon>{{ build.comments }}</v-chip
    >
    <v-chip v-show="build.upvotes" class="mr-2 mb-2" label :size="chipSize">
      <v-icon start icon="mdi-thumb-up"></v-icon>
      {{ build.upvotes }}</v-chip
    >
    <v-chip class="mr-2 mb-2" v-if="build.timeCreated" label :size="chipSize"
      ><v-icon start icon="mdi-clock-edit-outline"></v-icon
      >{{ timeSince(build.timeCreated.toDate()) }}</v-chip
    >
    <v-chip class="mr-2 mb-2" v-if="build.timeCreated" label :size="chipSize"
      ><v-icon start icon="mdi-update"></v-icon
      >{{ timeSince(build.timeUpdated.toDate()) }}</v-chip
    >
  </v-item-group>
</template>

<script>
import { computed } from "vue";
import { getCivById } from "@/composables/filter/civDefaultProvider";
import useTimeSince from "@/composables/useTimeSince";

export default {
  name: "BuildDetailChips",
  props: {
    build: {
      type: Object,
      required: true,
    },
    chipSize: {
      type: String,
      default: "small",
    },
    civLabel: {
      type: String,
      default: "title",
    },
    metaClass: {
      type: String,
      default: "",
    },
    mapClass: {
      type: String,
      default: "",
    },
    statsClass: {
      type: String,
      default: "",
    },
    showAuthorInMeta: {
      type: Boolean,
      default: false,
    },
    showAuthorInStats: {
      type: Boolean,
      default: true,
    },
  },
  setup(props) {
    const { timeSince, isNew } = useTimeSince();
    const civLabelText = computed(() => {
      if (!props.build?.civ) {
        return "";
      }
      const civ = getCivById(props.build.civ);
      return props.civLabel === "short" ? civ?.shortName : civ?.title;
    });

    return {
      civLabelText,
      timeSince,
      isNew,
    };
  },
};
</script>
