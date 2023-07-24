<script setup lang='ts'>
import { onMounted, ref } from 'vue'
import { deleteKb, getKbsList } from '@/api/chat'
import { idStore } from '@/store/modules/knowledgebaseid/id'

const items = ref<any>([])
const choice = ref('')
const store = idStore()

onMounted(async () => {
  choice.value = store.knowledgeid
  const res = await getKbsList()
  res.data.data.forEach((item: any) => {
    items.value.push({
      value: item,
      show: false,
    })
  })
})
const formValue = ref({
  user: {
    name: '',
  },
})
const rules = {
  user: {
    name: {
      required: true,
      message: '请输入名称',
      trigger: 'blur',
    },
  },
}
const handleValidateClick = (item: any) => {
  choice.value = item
  store.knowledgeid = choice.value
  items.value.forEach((res: { value: any; show: boolean }) => {
    if (res.value === item)
      res.show = !res.show
  },

  )
}
const handleClick = () => {
  if (formValue.value.user.name.trim() !== '') {
    items.value.push({
      value: formValue.value.user.name.trim(),
      show: false,
    })
  }
}
async function handleDelete(item: any) {
  await deleteKb(item.value)
  const res = await getKbsList()
  items.value = []
  res.data.data.forEach((item: any) => {
    items.value.push({
      value: item,
      show: false,
    })
  })
}
</script>
