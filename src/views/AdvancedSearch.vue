<template>
  <div class='advancedsearch'>
    <h2>Advanced Search</h2>
    <div style="display: flex;">
        <button @click='upperCaseNames'>To Upper Case!!!</button>
        <button @click='lowerCaseNames'>To Lower Case!!!</button>
        <button @click='normalCaseNames'>To Normal Case!!!</button>
    </div>
    <BullListing :data='tableData' :column-defs='columnDefs' :styles='tableStyle'/>
  </div>
</template>

<script>
    import BullListing from '@/components/BullListing.vue'
    import http from '@/services/httpClient.js'

    export default {
        data() { 
            return {
                tableData: [],
                columnDefs: [],
                tableStyle: { height: '700' }
            }
        },
        methods: {
            upperCaseNames: function() {
                this.columnDefs = this.columnDefs.map(item => Object.assign(item, {label: item.label.toUpperCase()}))
            },
            lowerCaseNames: function() {
                this.columnDefs = this.columnDefs.map(item => Object.assign(item, {label: item.label.toLowerCase()}))
            },
            normalCaseNames: function() {
                this.columnDefs = this.columnDefs.map(item => {
                    let splitlabel = item.label.split('')
                    return {...item, label: splitlabel[0].toUpperCase() + splitlabel.slice(1).join('').toLowerCase()}
                })
            }
        },
        mounted() {
            http.get('/bulls/search/all_bulls?$filter=((breed%20eq%20HO)%20or%20(breed%20eq%20AY)%20or%20(breed%20eq%20JE)%20or%20(breed%20eq%20GU)%20or%20(breed%20eq%20BS)%20or%20(breed%20eq%20MS)%20or%20(breed%20eq%20MO))%20and%20(substringof(%27Frost%27%2Ccodename)%20eq%20true)&$orderby=codename&$top=50&fuzzy=%7B%22textFilter%22%3A%5B%5D%2C%22sonsof%22%3A%7B%7D%2C%22mgs%22%3A%7B%7D%2C%22qualified%22%3Afalse%2C%22sexed%22%3Afalse%2C%22scr%22%3Afalse%2C%22sonsofprovengenomic%22%3A%22All%22%2C%22mgsofprovengenomic%22%3A%22All%22%7D&options=%7B%22orgs%22%3A%5B7%2C14%2C250%2C507%2C509%2C550%2C559%2C614%2C714%2C7%2C14%2C507%2C509%2C614%2C714%2C250%2C550%2C559%2C29%2C529%2C11%2C511%2C1%2C501%2C70%2C200%2C76%2C151%2C551%2C203%2C147%2C187%2C587%2C513%2C202%2C21%2C8%2C289%2C9%2C122%2C204%2C2193%2C23%2C94%2C44%2C2165%2C202%2C%22%20%22%2C%22all%22%5D%2C%22allOrgsChecked%22%3Atrue%7D&format=grid&breeds=HO%2CAY%2CJE%2CGU%2CBS%2CMS%2CMO&sonsoffirst=true&mgsoffirst=true').then( res => {
                this.tableData = res.results
                this.columnDefs = Object.keys(res.results[0]).map(key => {
                    return {prop: key, label: key, fixed: key == 'codeno', width: key == 'codeno' ? '150' : 'auto'}
                })
            })
        },
        components: { BullListing }
    }
</script>

<style lang='scss' scoped>
  .advancedsearch {
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
  }
</style>
