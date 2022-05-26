<template>
  <div class="absolute 
  -bottom-64 
  lg:-bottom-20 
  flex flex-col 
  lg:flex-row 
  bg-white 
  gap-y-4 
  gap-x-12 
  px-8 pt-8 pb-12
  rounded-md" >
    <div class="flex flex-col">
        <h3 class="text-xs mb-2 uppercase">Adres IP</h3>
        <span class="text-2xl">{{ipInfo.address}}</span>
    </div>
    <div class="flex flex-col">
        <h3 class="text-xs mb-2 uppercase">Lokalizacja</h3>
        <span class="text-2xl">{{ipInfo.state}}</span>
    </div>
    <div class="flex flex-col">
        <h3 class="text-xs mb-2 uppercase">Strefa czasowa</h3>
        <span class="text-2xl">UTC {{ipInfo.timezone}}</span>
    </div>
    <div class="flex flex-col">
        <h3 class="text-xs mb-2 uppercase">Dostawca</h3>
        <span class="text-2xl">{{ipInfo.isp}}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: ['ipInfo'],
}
</script>
