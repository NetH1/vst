<script>
import { ref } from "@vue/reactivity";
export default {
  data() {
    const inputStr = ref('')
    const sorted_name = ref(false)
    const sorted_amount = ref(false)
    const sorted_distance = ref(false)
    const testsPerPage = ref(6)
    const SelectedSort = ref('')
    const CurrentPage = ref(1)
    const show = ref(false)
    const tests = ref(
      [
        {id: 1, date:"02-05-1985", name: 'Albania', amount:300, distance:6420},
        {id: 2, date:"04-03-1283", name: 'KZ', amount: 200, distance:2020},
        {id: 3, date:"16-01-1002", name: 'USA', amount: 500, distance:5040},
        {id: 4, date:"27-07-1645", name: 'Bulgaria', amount: 100, distance:4060},
        {id: 5, date:"02-05-1985", name: 'UA', amount:500, distance:6100},
        {id: 6, date:"04-03-1283", name: 'Japan', amount: 50, distance:2400},
        {id: 7, date:"16-01-1002", name: 'England', amount: 1000, distance:2500},
        {id: 8, date:"27-07-1645", name: 'Russia', amount: 4002, distance:7400},
        {id: 9, date:"02-05-1985", name: 'Malazia', amount:150, distance:6800},
        {id: 10, date:"04-03-1283", name: 'China', amount: 4000, distance:2200},
        {id: 11, date:"16-01-1002", name: 'Brazil', amount: 502, distance:1500},
        {id: 12, date:"27-07-1645", name: 'Horvatia', amount: 101, distance:4200},
        {id: 13, date:"02-05-1985", name: 'Austria', amount:4200, distance:7421},
        {id: 14, date:"04-03-1283", name: 'Australia', amount: 7200, distance:2921},
        {id: 15, date:"16-01-1002", name: 'Vietnam', amount: 550, distance:5282},
        {id: 16, date:"27-07-1645", name: 'Germany', amount: 940, distance:5472},
        {id: 17, date:"02-05-1985", name: 'Greenland', amount:760, distance:9652},
        {id: 18, date:"04-03-1283", name: 'Egypt', amount: 586, distance:4656},
        {id: 19, date:"16-01-1002", name: 'Zimbabwe', amount: 904, distance:3872},
        {id: 20, date:"27-07-1645", name: 'Israel', amount: 5591, distance:7777},
        {id: 21, date:"02-05-1985", name: 'India', amount:123, distance:2671},
        {id: 22, date:"04-03-1283", name: 'Spain', amount: 259, distance:6342},
        {id: 23, date:"16-01-1002", name: 'Canada', amount: 236, distance:6995},
        {id: 24, date:"27-07-1645", name: 'Kenya', amount: 965, distance:1542},
        ],
    )
    const options = ref([
      {value: 'title', name: "По названию"},
      {value: 'amount', name: "По кол-ву"},
      {value: 'distance', name: "По расстоянию"},
    ])
    const sortName = (sorted_name) =>{
      sorted_name
      ?this.tests.sort((a,b) => a.name.localeCompare(b.name))
      :this.tests.sort((a,b) => b.name.localeCompare(a.name));
    }
    const sortAmount = (sorted_amount) =>{
      sorted_amount 
      ?this.tests.sort((a,b) => a.amount-b.amount)
      :this.tests.sort((a,b) => b.amount-a.amount);
    }
    const sortDistance = (sorted_distance) =>{
      sorted_distance
      ?this.tests.sort((a,b) => a.distance-b.distance)
      :this.tests.sort((a,b) => b.distance-a.distance);
    }
    const resetAll = () =>{
      this.tests.slice()
    }
    const pageClick = (page) => {
      this.CurrentPage = page
    }
    return{
        inputStr,
        sorted_name,
        sorted_amount,
        sorted_distance,
        tests,
        testsPerPage,
        CurrentPage,
        show,
        SelectedSort,
        options,
        sortName,
        sortAmount,
        sortDistance,
        resetAll,
        pageClick,
    }
  },
  computed:{
    //filter
    // testsFilter() {
    //   return this.tests.filter(item => item.name.toLowerCase().indexOf(this.inputStr.toLowerCase()) !== -1)
    // },
    pages() {
      return Math.ceil(this.tests.length/ 6)
    },
    // Пагинация
    // paginateTests() {
    //   let from =(this.CurrentPage - 1) * this.testsPerPage;
    //   let to =from + this.testsPerPage;
    //   return this.tests.slice(from,to)
    // },
    SortedTests() {
          return [...this.tests].sort((a,b) => a[this.SelectedSort] - b[this.SelectedSort])
    },
    SortAndSearchAndPaginate(){
      let from =(this.CurrentPage - 1) * this.testsPerPage;
      let to =from + this.testsPerPage;
      return this.SortedTests.filter(item => item.name.toLowerCase().indexOf(this.inputStr.toLowerCase()) !== -1).slice(from,to)
    }
  },
  methods: {
    changeOption(event) {
      console.log(event);
      this.$emit('update:SelectedSort', event.target.value)
    },
  },
}
</script>

<template>
  <div>
    <div>
      <!--Искать по названиям-->
      <input 
      class="sort_input" 
      type="text" 
      placeholder="Сортировка по названию" 
      v-model="inputStr">
      <!--Выпадающий список-->
        <div class="div_list">
          <p @click="show =!show" class="p_sort">Сортировка</p>
          <p>Все работает которая сверху, а снизу не работает только по названия условия ставил не отрабатывает не знаю в чем проблема</p>
          <div :class="{'active': show}" class="div_option">
          <p @click="resetAll">Все</p> <!--Вернет массив-->
          <p @click="sortName(sorted_name = !sorted_name)">По названию </p> <!--кликните два раза метод sort изменится-->
          <p @click="sortAmount(sorted_amount =!sorted_amount)">По кол-ву</p><!--кликните два раза метод sort изменится-->
          <p @click="sortDistance(sorted_distance =!sorted_distance)">По расстоянию</p><!--кликните два раза метод sort изменится-->
        </div>
        </div>
        <select v-model="SelectedSort" @change="changeOption">
          <option disabled value="">Сортировка</option>
          <option 
          v-for="option in options" 
          :key="option.value"
          :value="option.value">
          {{option.name}}
          </option>
        </select>
        <!--Таблица-->
      <table class="table_class">
        <tr>
          <th>Date</th>
          <th>Name</th>
          <th>Population</th>
          <th>Distance</th>
        </tr>
        <tr 
        v-for="test in SortAndSearchAndPaginate" 
        :key="test.id"> 
          <td class="td_date">{{test.date}}</td>
          <td class="td_name">{{test.name}}</td>
          <td class="td_amount">{{test.amount}}</td>
          <td class="td_distance">{{test.distance}}.m</td>
        </tr>
      </table>
      <!--Пагинация-->
      <div class="div_paginaion">
        <div 
        v-for="page in pages" 
        :key="page" 
        class="div_pages"
        @click="pageClick(page)">
          {{page}}
        </div>
      </div>
    </div>
  </div>
</template>


<style lang="scss" scoped >
$mycolor: red;
$bgcolor: black;

.div_list{
  font-size: 18px;
  cursor: pointer;
  .p_sort{
    border: 2px solid black;
    width: 120px;
  }
}
.div_option{
    display: none;
    :hover{
      transition: all 0.5s;
    background: green;
    width: 120px;
    color: white;
  }
    >p{
      border: 2px solid black;
    width: 120px;
    margin:2px;
    }
  }
  .active{
    display: block;
  }
.table_class{
  $mycolor: green ;
  color: $mycolor;
  width: fit-content;
  font-size: 24px;
  text-align: center;
  td{
    border: 2px solid black;
    width: 1000px;
  }
  th{
    border: 4px solid black;
    font-weight: 700;
    font-size: 36px;
  }
  .td_date{
    color: black;
  }
  .td_name{
    color: red;
  }
  .td_amount{
    color: rgb(115, 255, 0);
  }
  .td_distance{
    color: rgb(0, 102, 255);
  }
}
.sort_input{
  max-width: 470px;
  width: 350px;
  font-size: 24px;
  padding: 10px 20px;
  margin-bottom: 3rem;
}
.tr_sort{
  margin-top: 2rem;

}
.div_paginaion{
  display: flex;
  justify-content: center;
  margin-top: 24px;
  .div_pages{
    color: white;
    border: 1px solid black;
    background: #000;
    margin-right: 8px;
    padding: 2px 3px ;
    cursor: pointer;
  }
}

</style>
