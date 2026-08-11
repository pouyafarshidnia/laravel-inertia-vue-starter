<script setup lang="ts">
import type { SelectItemProps } from "reka-ui"
import type { HTMLAttributes, Ref, ComponentPublicInstance } from "vue"
import { Check } from "@lucide/vue"
import { computed, inject, nextTick, ref, watch } from "vue"
import { reactiveOmit } from "@vueuse/core"
import {
  SelectItem,
  SelectItemIndicator,
  SelectItemText,
  useForwardProps,
} from "reka-ui"
import { cn } from "@/lib/utils"

const props = defineProps<SelectItemProps & { class?: HTMLAttributes["class"] }>()

const delegatedProps = reactiveOmit(props, "class")

const forwardedProps = useForwardProps(delegatedProps)

const selectSearch = inject<Ref<string> | null>("selectSearch", null)
const itemRef = ref<ComponentPublicInstance | null>(null)
const itemText = ref("")

const isVisible = computed(() => {
  if (!selectSearch?.value) return true
  return itemText.value.toLowerCase().includes(selectSearch.value.toLowerCase())
})

watch(() => itemRef.value, async (el) => {
  await nextTick()
  if (el?.$el) {
    itemText.value = el.$el.textContent?.trim() ?? ""
  }
}, { immediate: true })
</script>

<template>
  <SelectItem ref="itemRef" v-show="isVisible" data-slot="select-item" v-bind="forwardedProps" :class="cn(
    'focus:bg-accent focus:text-accent-foreground [&_svg:not([class*=\'text-\'])]:text-muted-foreground relative flex w-full cursor-default items-center gap-2 rounded-sm py-1.5 pr-8 pl-2 text-sm outline-hidden select-none data-[disabled]:pointer-events-none data-[disabled]:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*=\'size-\'])]:size-4 *:[span]:last:flex *:[span]:last:items-center *:[span]:last:gap-2',
    props.class,
  )
    ">
    <span class="absolute right-2 flex size-3.5 items-center justify-center">
      <SelectItemIndicator>
        <slot name="indicator-icon">
          <Check class="size-4" />
        </slot>
      </SelectItemIndicator>
    </span>

    <SelectItemText>
      <slot />
    </SelectItemText>
  </SelectItem>
</template>
