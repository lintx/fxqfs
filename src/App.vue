<template>
  <n-config-provider>
    <n-loading-bar-provider>
      <n-message-provider>
        <n-notification-provider>
          <n-dialog-provider>
            <n-layout>
              <n-layout-content content-style="padding: 24px;">
                <n-space vertical>
                  <n-grid item-responsive responsive="screen" x-gap="12">
                    <template v-for="(v,k) in list">
                      <n-form-item-gi :label="k + '数据'" span="xs:24 s:24 m:24 l:12 xl:12 xxl:12">
                        <n-input-group>
                          <template v-for="(vv,kk) in list[k]">
                            <n-popover trigger="hover">
                              <template #trigger>
                                <n-input-number v-model:value="list[k][kk]" :placeholder="kk" />
                              </template>
                              <span>{{kk}}</span>
                            </n-popover>
                          </template>
                        </n-input-group>
                      </n-form-item-gi>
                    </template>
                  </n-grid>
                  <n-data-table
                      :columns="columns"
                      :data="result"
                      :bordered="false"
                  />
                </n-space>
              </n-layout-content>
            </n-layout>
          </n-dialog-provider>
        </n-notification-provider>
      </n-message-provider>
    </n-loading-bar-provider>
    <n-global-style />
  </n-config-provider>
</template>

<script setup>
const list = ref({
  "红":{"踩黄":0,"踩蓝":0,"踩绿":0,"进":0,"漏":0},
  "黄":{"踩红":0,"踩蓝":0,"踩绿":0,"进":0,"漏":0},
  "蓝":{"踩红":0,"踩黄":0,"踩绿":0,"进":0,"漏":0},
  "绿":{"踩红":0,"踩黄":0,"踩蓝":0,"进":0,"漏":0},
})
const result = computed(()=>{
  const data = {
    "红":{"踩人":0,"被踩":0,"漏踩":0,"进机":0,"总分":0},
    "黄":{"踩人":0,"被踩":0,"漏踩":0,"进机":0,"总分":0},
    "蓝":{"踩人":0,"被踩":0,"漏踩":0,"进机":0,"总分":0},
    "绿":{"踩人":0,"被踩":0,"漏踩":0,"进机":0,"总分":0},
  }
  for (const k in list.value) {
    for (const kk in list.value[k]) {
      if (kk === "进") {
        for (const kkk in data) {
          if (kkk === k) {
            data[kkk]["进机"] += list.value[k][kk] * 3
          }else {
            data[kkk]["进机"] -= list.value[k][kk]
          }
        }
      }else if (kk === "漏") {
        for (const kkk in data) {
          if (kkk === k) {
            data[kkk]["进机"] -= list.value[k][kk] * 3
          }else {
            data[kkk]["进机"] += list.value[k][kk]
          }
        }
      }else {
        const color = kk.replace("踩","")
        for (const kkk in data) {
          if (kkk === k) {
            data[kkk]["踩人"] += list.value[k][kk]
          }else if (kkk === color)  {
            data[kkk]["被踩"] -= list.value[k][kk]
          }
        }
      }
    }
  }
  for (const k in data) {
    let c = 0
    for (const kk in data[k]) {
      c += data[k][kk]
    }
    data[k]["总分"] = c
  }
  const result = []
  for (const k in data) {
    result.push({...data[k],"颜色":k})
  }
  return result
})
const columns = [
  {title:"颜色",key:"颜色"},
  {title:"踩人",key:"踩人"},
  {title:"被踩",key:"被踩"},
  {title:"漏踩",key:"漏踩"},
  {title:"进机",key:"进机"},
  {title:"总分",key:"总分"},
]
</script>

<style>

</style>
