<script lang="ts" setup>
import EmojiField from "@/components/EmojiField.vue";
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg";
import type Emoji from "@/types/Emoji";
import type Entry from "@/types/Entry";
import { ref, computed } from "vue";

defineEmits<{
  (e: "@create", entry: Entry): void;
}>();

const body = ref("");
const emoji = ref<Emoji | null>(null);
const charCount = computed<number>(() => {
  return body.value.length;
});
const MAX_CHARS = 280;
const handleTextInput = (e: Event) => {
  const textarea = e.target as HTMLTextAreaElement;
  body.value = "";
  if (textarea.value.length <= MAX_CHARS) {
    body.value = textarea.value;
  } else {
    body.value = textarea.value.substring(0, MAX_CHARS);
    console.log(body.value);
  }
};
</script>
<template>
  <form
    class="entry-form"
    @submit.prevent="
      $emit('@create', {
        body,
        emoji,
        createdAt: new Date(),
        userId: 1,
        id: Math.random(),
      })
    "
  >
    <textarea
      :value="body"
      @input="handleTextInput"
      placeholder="New Journal Entry for danielkelly_io"
    ></textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ MAX_CHARS }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>
