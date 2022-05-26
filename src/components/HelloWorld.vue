<template>
  <div class="hello">
    <div class="w-[600px] mx-auto relative">
      <span class="text-center uppercase font-bold text-2xl">Todolist</span>
      <div class="absolute border border-black w-[200px] h-[36px] top-0 right-0">
        <input
        v-model = "searchData"
        type="text" class=" h-full w-full outline-0 px-2 pr-8" placeholder="search todo..." />
        <span 
        @click = "handleSearchTodo"
        class="absolute right-0 top-[50%] translate-y-[-50%] p-2 cursor-pointer"
        ><svg width="21" height="21" viewBox="0 0 21 21" fill="none" xmlns="http://www.w3.org/2000/svg">
<path d="M20 20L15.514 15.506L20 20ZM18 9.5C18 11.7543 17.1045 13.9163 15.5104 15.5104C13.9163 17.1045 11.7543 18 9.5 18C7.24566 18 5.08365 17.1045 3.48959 15.5104C1.89553 13.9163 1 11.7543 1 9.5C1 7.24566 1.89553 5.08365 3.48959 3.48959C5.08365 1.89553 7.24566 1 9.5 1C11.7543 1 13.9163 1.89553 15.5104 3.48959C17.1045 5.08365 18 7.24566 18 9.5V9.5Z" stroke="black" stroke-width="2" stroke-linecap="round"/>
</svg>
</span>
        <span>

        </span>
      </div>
      <div class="flex gap-[10px] h-[42px] my-4">
        <input
          ref="todo"
          v-model="todo"
          placeholder="Add Todo ..."
          class="w-[540px] border border-[#59a3c8] outline-0 px-4"
        />
        <span
          @click="addTodo"
          class="text-[36px] bg-[#59a3c8] px-4 li text-white cursor-pointer leading-[42px]"
          >+</span
        >
      </div>
      <table>
        <thead>
          <tr class="">
            <td></td>
            <td class="w-[252px] flex justify-center gap-x-2">Name <span @click = "handleSearchByName"><svg width="16" height="20" viewBox="0 0 16 20" fill="none" xmlns="http://www.w3.org/2000/svg">
<path d="M4 14H0L6 20V0H4V14ZM10 3V20H12V6H16L10 0V3Z" fill="black"/>
</svg>
</span></td>
            <td class="px-[60px] relative ">Level
              <span @click = "handleSearchByLevel" class="absolute ml-2"><svg width="16" height="20" viewBox="0 0 16 20" fill="none" xmlns="http://www.w3.org/2000/svg">
<path d="M4 14H0L6 20V0H4V14ZM10 3V20H12V6H16L10 0V3Z" fill="black"/>
</svg>
</span>
              </td>
            <td class="px-[60px]">Action</td>
          </tr>
        </thead>
        <tbody>
            <tr
            v-for="(todo, index) in todoList"
            :key="index">
            <td class="w-[20px]"><input type="checkbox" v-model="todo.status" class="w-[16px] h-[16px] border-2"  /></td>
            <td><input
            v-model="todoEdit.name"
            v-if="todoEdit.id === todo.id"
            class="w-[250px] text-center text-[#59a3c8] outline-0 border"
          />
          <p :class="{'line-through': todo.status }" v-else class="text-[#59a3c8]">{{ todo.name }}</p></td>
            <td class="px-[60px]"><input
            v-model="todoEdit.level"
            v-if="todoEdit.id === todo.id"
            class=" w-[20px] text-[#59a3c8] outline-0 border text-center"
          />
          <p :class="{'line-through': todo.status }" v-else class="text-[#59a3c8]">{{ todo.level }}</p>
          
          </td>
            <td><div class="flex gap-x-4 ml-[50%] translate-x-[-50%]">
          
          <span @click="handleEditTodo" v-if="todoEdit.id === todo.id"  class="cursor-pointer">
            <svg
              width="24"
              height="24"
              viewBox="0 0 18 14"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M1 7L7 13L17 1"
                stroke="#41b883"
                stroke-width="3"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
            </span>
            <span @click="setEditTodo(todo)"  class="group cursor-pointer" v-else>
            <svg
              width="24"
              height="24"
              viewBox="0 0 34 34"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                class="group-hover:fill-[#59a3c8]"
                d="M23.3277 1.95447L11.6232 13.6588C6.72561 18.5688 3.01672 24.5345 0.780635 31.0991C0.673999 31.4096 0.657016 31.7439 0.731632 32.0636C0.806248 32.3833 0.969444 32.6755 1.20251 32.9067L1.20863 32.9129C1.37081 33.0763 1.56371 33.206 1.77622 33.2946C1.98873 33.3832 2.21666 33.4289 2.4469 33.4291C2.64064 33.429 2.83304 33.397 3.01643 33.3346C9.58108 31.0985 15.5468 27.3896 20.4567 22.4919L32.1608 10.7876C33.3321 9.61628 33.9902 8.0276 33.9902 6.37107C33.9902 4.71454 33.3321 3.12586 32.1608 1.95451C31.5808 1.37451 30.8923 0.914438 30.1345 0.600546C29.3767 0.286654 28.5645 0.125095 27.7442 0.125092C26.0877 0.125085 24.499 0.783133 23.3277 1.95447V1.95447ZM18.8654 20.9009C14.4416 25.3132 9.11334 28.7134 3.24818 30.8671C5.40179 25.0019 8.80197 19.6737 13.2142 15.2498L20.1865 8.27753L25.8377 13.9287L18.8654 20.9009ZM30.5698 9.19666L27.4287 12.3377L21.7775 6.68657L24.9186 3.54543C25.2895 3.17349 25.7301 2.8783 26.2151 2.67677C26.7002 2.47524 27.2202 2.37131 27.7454 2.37093C28.2707 2.37055 28.7909 2.47373 29.2762 2.67456C29.7616 2.87539 30.2025 3.16994 30.5739 3.54135C30.9454 3.91276 31.2399 4.35375 31.4407 4.8391C31.6415 5.32444 31.7447 5.84461 31.7443 6.36987C31.7439 6.89512 31.64 7.41514 31.4385 7.90019C31.2369 8.38524 30.9417 8.82579 30.5698 9.19666Z"
                fill="#ccc"
              />
            </svg>
          </span>
          <span @click = "handleDeleteTodo(todo.id)"
           class="group cursor-pointer"
            ><svg
              width="24"
              height="24"
              viewBox="0 0 26 32"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                class="group-hover:fill-[#59a3c8]"
                d="M2 10V30C2 31.1 2.9 32 4 32H22C23.1 32 24 31.1 24 30V10H2ZM8 28H6V14H8V28ZM12 28H10V14H12V28ZM16 28H14V14H16V28ZM20 28H18V14H20V28ZM24.5 4H18V1.5C17.9984 1.10266 17.8399 0.722048 17.5589 0.441085C17.278 0.160122 16.8973 0.00157989 16.5 0L9.5 0C9.10266 0.00157989 8.72205 0.160122 8.44108 0.441085C8.16012 0.722048 8.00158 1.10266 8 1.5V4H1.5C1.1025 4.00106 0.721582 4.15943 0.440506 4.44051C0.15943 4.72158 0.00105578 5.1025 0 5.5V8H26V5.5C25.9989 5.1025 25.8406 4.72158 25.5595 4.44051C25.2784 4.15943 24.8975 4.00106 24.5 4ZM16 4H10V2.026H16V4Z"
                fill="#ccc"
              />
            </svg>
          </span>
        </div></td>
          </tr>
          <p class="absolute ml-[50%] mt-4 translate-x-[-50%] text-[red]" v-if="todoList.length <= 0">Don't have task</p>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: 0,
      btnClass:
        "bg-[#9b4dca] hover:bg-[#000] text-white rounded-md px-2 py-1 mx-2",
      todoList: [
        { id: 1, name: "Learn English",level:1, status: false },
        { id: 2, name: "Call Parents",level:3, status: false },
        { id: 3, name: "Study Code",level:2, status: false },
      ],
      todoEdit: {},
      todo: "",
      checkedItems : false,
      searchData:'',
      isSearchByName : false,
      isSearchByLevel : false,
    };
  },
  methods: {
    addTodo() {
      if (this.todo.length > 0) {
        const newTodo = {
          id: Math.floor(Math.random() * 10000),
          name: this.todo,
          level: Math.floor(Math.random() * 3)+1,
          status: false,
        };
        this.todoList = [...this.todoList, newTodo];
        this.todo = "";
      }
      this.$refs.todo.focus();
    },
    setEditTodo(todo) {
      this.todoEdit = this.todoList.find((item) => item.id === todo.id);
    },
    handleEditTodo(){
      const objIndex = this.todoList.findIndex(item => item.id === this.todoEdit.id)
      let todos = [...this.todoList];
      todos[objIndex] = this.todoEdit;
      this.todoList = [...todos]
      this.todoEdit = {}
    },
    handleDeleteTodo(id){
      let todos = [...this.todoList];
      todos = todos.filter(todo => todo.id !== id);
      this.todoList = [...todos];
    },
    handleSearchTodo(){
      if(this.searchData.length >0 ){
        let todos = [...this.todoList];
        todos = todos.filter(todo => todo.name.includes(this.searchData) || todo.level === Number.parseInt(this.searchData));
        this.todoList = [...todos];
      }
    },
    handleSearchByName(){
      if(this.isSearchByName){
        this.todoList = this.todoList.reverse()
      }else{
        let todos = [...this.todoList];
      todos = todos.sort((a,b)=> {
        if(a.name > b.name) {return 1;}
        if(a.name < b.name) {return -1;}
        return 0;
      })
      this.todoList  = [...todos]
      this.isSearchByName = true
      }
    },
    handleSearchByLevel(){
      if(this.isSearchByLevel){
        this.todoList = this.todoList.reverse()
      }else{
        let todos = [...this.todoList];
      todos = todos.sort((a,b)=> {
        if(a.level > b.level) {return 1;}
        if(a.level < b.level) {return -1;}
        return 0;
      })
      this.todoList  = [...todos]
      this.isSearchByLevel = true
      }
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
