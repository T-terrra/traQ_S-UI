<template>
  <modal-frame
    title="グループ"
    :subtitle="groupName"
    icon-name="group"
    return-button
  >
    <user-list-item
      v-for="id in groupMember"
      :key="id"
      :user-id="id"
      :class="$style.item"
    />
  </modal-frame>
</template>

<script lang="ts">
import { defineComponent, computed } from '@vue/composition-api'
import store from '@/store'
import ModalFrame from '../Common/ModalFrame.vue'
import UserListItem from '../Common/UserListItem.vue'
import { UserAccountState } from '@traptitech/traq'

export default defineComponent({
  name: 'GroupModal',
  components: {
    ModalFrame,
    UserListItem
  },
  props: {
    groupId: {
      type: String,
      required: true
    }
  },
  setup(props) {
    const group = computed(() => store.state.entities.userGroups[props.groupId])
    const groupName = computed(() => group.value?.name)
    const groupMember = computed(
      () =>
        group.value?.members
          .filter(
            member =>
              store.state.entities.users[member.id]?.state ===
              UserAccountState.active
          )
          .map(member => member.id) ?? []
    )
    return { groupName, groupMember }
  }
})
</script>

<style lang="scss" module>
.item {
  margin: 16px 0;
  &:first-child {
    margin-top: 0;
  }
  &:last-child {
    margin-bottom: 0;
  }
}
</style>
