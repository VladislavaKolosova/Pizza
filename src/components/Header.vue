<template>
  <div class="page-container">
    <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawerRight"
      fixed 
      right
      app
    >
    <div class="title-order" v-if="!list"><h2>Корзина пуста</h2></div>
    
      <v-list dense>
        <md-list v-for="i in list"
          :key="i.id">
          <md-list-item> 
            <img class="small-img" :src="`${i.image}`" alt="none">
            
          </md-list-item>
          <md-list-item>
            <span class="md-list-item-text"><h2>Вес:</h2><h4>{{i.selected}}(г)</h4></span>
            <span class="md-list-item-text"><h2>Цена:</h2> <h4>{{i.price}}₽</h4></span>
          </md-list-item>
<v-btn
            absolute
            color="green"
            class="white--text"
            fab
            small
            right
            bottom
            @click="del(i.id)"
          >
            <v-icon>X</v-icon>
          </v-btn>
        </md-list>
        
        <v-btn v-if="list" block large top @click="pay" depressed color="green" class="white--text">К оплате: {{orderCost}}₽ Заказать?</v-btn>
      </v-list>
      
    </v-navigation-drawer>
    <v-toolbar
      color="green"
      dark
      fixed
      app
    >
      <!-- <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon> --> 
      <v-toolbar-title>Соберите пиццу самостоятельно используя наш конструктор пиццы</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-toolbar-title>Заказать столик</v-toolbar-title>
      <v-toolbar-title></v-toolbar-title>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-toolbar-title>Контакты</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-side-icon @click.stop="drawerRight = !drawerRight">
        <v-icon>shopping_cart</v-icon>
      </v-toolbar-side-icon>
    </v-toolbar>
    
    <!-- <v-navigation-drawer
      v-model="drawer"
      fixed
      app
    >
      <v-list dense>
        <v-list-tile @click.stop="left = !left">
          <v-list-tile-action>
            <v-icon>exit_to_app</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title><h1>Конструктор Колосова Влада </h1></v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer> -->
    <v-content>
      <v-container fluid fill-height>
        <v-layout justify-center align-center>
          <v-flex shrink>
            <md-card md-with-hover
              v-for="ingredient in ingredients"
              :key="ingredient.name">
              <md-ripple>   
              <md-card-header>
                <md-card-header-text>
                  <div class="md-title">{{ingredient.name}} {{ingredient.cost}}₽ (100g)</div>
                  <div class="md-subhead">Текущая стоимость:</div>
                  <div class="md-subhead">{{ingredient.cost*ingredient.selected*0.01}}₽</div>
                </md-card-header-text>
              
              </md-card-header>
                <md-card-media>
                  <img :src="`${ingredient.image}`" alt="none" class="img">
                </md-card-media>
              <md-card-actions>
                <div class="group">      
                    <input type="text" required v-model="ingredient.selected">
                    <label >Вес (g)</label>
                </div>
              </md-card-actions>
              <div class="buttons">
                  <md-button @click="remove(ingredient.id)" class="md-raised ">-</md-button>
                  <md-button @click="add(ingredient.id)" class="md-raised ">+</md-button>
              </div>
            </md-ripple>
            </md-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
    <!-- <v-footer color="yellow" class="white--text" app>
      <span>Заказать пиццу – первое, что приходит на ум, когда сильно проголодался. Пицца – это идеально для компании, которая собралась за просмотром фильма или настолками. Главное, чтобы ее вкус и цена никого не </span>
      <v-spacer>Pizza constructor</v-spacer>
      <span> Практика</span>
    </v-footer> -->
    <v-container fluid grid-list-md>
    <v-data-iterator
      :items="items"
      :rows-per-page-items="rowsPerPageItems"
      :pagination.sync="pagination"
      content-tag="v-layout"
      row
      wrap
    >
      <template v-slot:item="props">
        <v-flex
          xs12
          sm6
          md4
          lg3
        >
          <v-card>
            <v-card-title><h4>{{ props.item.name }}</h4></v-card-title>
            <v-divider></v-divider>
            <v-list dense>
              <v-list-tile>
                <v-list-tile-content>Калории:</v-list-tile-content>
                <v-list-tile-content class="align-end">{{ props.item.calories }}</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content>Жиры:</v-list-tile-content>
                <v-list-tile-content class="align-end">{{ props.item.fat }}</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content>Карбоны:</v-list-tile-content>
                <v-list-tile-content class="align-end">{{ props.item.carbs }}</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content>Белки:</v-list-tile-content>
                <v-list-tile-content class="align-end">{{ props.item.protein }}</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content>Нитрины:</v-list-tile-content>
                <v-list-tile-content class="align-end">{{ props.item.sodium }}</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content>Кальций:</v-list-tile-content>
                <v-list-tile-content class="align-end">{{ props.item.calcium }}</v-list-tile-content>
              </v-list-tile>
              <v-list-tile>
                <v-list-tile-content>Железо:</v-list-tile-content>
                <v-list-tile-content class="align-end">{{ props.item.iron }}</v-list-tile-content>
              </v-list-tile>
            </v-list>
          </v-card>
        </v-flex>
      </template>
    </v-data-iterator>
  </v-container>
    </v-app>
  </div> 
    <!-- <md-app>
      <md-app-toolbar class="md-primary" md-elevation="0">
        <md-button class="md-icon-button" @click="toggleMenu" v-if="!menuVisible">
          <md-icon>menu</md-icon>
        </md-button>
        <span class="md-title">Order Menu</span>
      </md-app-toolbar>

      <md-app-drawer :md-active.sync="menuVisible" md-persistent="full">
        <md-toolbar class="md-transparent" md-elevation="0">
          <h2>Your Order</h2>
          <div class="md-toolbar-section-end">
            <md-button class="md-icon-button md-dense" @click="toggleMenu">
              <md-icon>keyboard_arrow_left</md-icon>
            </md-button>
          </div>
        </md-toolbar>

        <md-list v-for="i in list"
          :key="i.id">
          <md-list-item> 
            <img class="small-img" :src="`${i.image}`" alt="none">
            
          </md-list-item>
          <md-list-item>
            <span class="md-list-item-text"><h2>weight:</h2><h4>{{i.selected}}(g)</h4></span>
            <span class="md-list-item-text"><h2>price:</h2> <h4>{{i.price}}₽</h4></span>
          </md-list-item>

        </md-list>
        <md-button @click="pay" class="md-accent">Pay: {{orderCost}}₽</md-button>
      </md-app-drawer>

      <md-app-content>
       <div>
      <md-card md-with-hover
      v-for="ingredient in ingredients"
      :key="ingredient.name">
      <md-ripple>   
      <md-card-header>
        <md-card-header-text>
          <div class="md-title">{{ingredient.name}} {{ingredient.cost}}₽ (100g)</div>
          <div class="md-subhead">current price:{{ingredient.cost*ingredient.selected*0.01}}₽</div>
        </md-card-header-text>
       
      </md-card-header>
         <md-card-media>
          <img :src="`${ingredient.image}`" alt="none" class="img">
        </md-card-media>
      <md-card-actions>
        <div class="group">      
            <input type="text" required v-model="ingredient.selected">
            <label >product weight (g)</label>
        </div>
      </md-card-actions>
      <div class="buttons">
          <md-button @click="remove(ingredient.id)" class="md-raised ">Remove</md-button>
          <md-button @click="add(ingredient.id)" class="md-raised ">Add</md-button>
      </div>
    </md-ripple>
    </md-card>
  </div>
      </md-app-content>
    </md-app>
 -->
</template>

<script>
  export default {
    name: 'PersistentMini',
    data: () => ({
      drawer: true,
      drawerRight: false,
      right: null,
      left: null,
      menuVisible: false,
      rowsPerPageItems: [4, 8, 12],
      pagination: {
        rowsPerPage: 4
      },
      items: [
        {
          name: 'Маргарита',
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          sodium: 87,
          calcium: '14%',
          iron: '1%'
        },
        {
          name: '4 сыра',
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          sodium: 129,
          calcium: '8%',
          iron: '1%'
        },
        {
          name: 'Гавайская',
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          sodium: 337,
          calcium: '6%',
          iron: '7%'
        },
        {
          name: 'Грибная',
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          sodium: 413,
          calcium: '3%',
          iron: '8%'
        },
        {
          name: 'Томатная',
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9,
          sodium: 327,
          calcium: '7%',
          iron: '16%'
        },
        {
          name: 'Ореховая',
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0,
          sodium: 50,
          calcium: '0%',
          iron: '0%'
        },
        {
          name: 'Сладкая',
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0,
          sodium: 38,
          calcium: '0%',
          iron: '2%'
        },
        {
          name: 'Детская',
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5,
          sodium: 562,
          calcium: '0%',
          iron: '45%'
        },
        {
          name: 'Диетическая',
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9,
          sodium: 326,
          calcium: '2%',
          iron: '22%'
        },
        {
          name: 'Овощная',
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7,
          sodium: 54,
          calcium: '12%',
          iron: '6%'
        }
      ],
      ingredients: [
          {id: 0, name: 'Колбаска', cost: 500, image:'https://e2.edimdoma.ru/data/recipes/0009/8680/98680-ed4_wide.jpg?1492714843', selected: null},
          {id: 1, name: 'Сырок', cost: 300, image:'https://g1.delphi.lv/images/pix/659x380/TZWi4ob8wn4/krievijas-siers-45927697.jpg', selected: null},
          {id: 2, name: 'Грибочки', cost: 250, image:'https://cdn-st2.rtr-vesti.ru/vh/pictures/hd/172/841/3.jpg', selected: null},
          {id: 3, name: 'Овощи', cost: 200, image:'https://e2.edimdoma.ru/data/posts/0001/6708/16708-ed4_wide.jpg?1495010933', selected: null},
          {id: 4, name: 'Фрукты', cost: 300, image:'https://goroskop.guru/wp-content/uploads/2017/11/Kakoj-Vy-frukt.jpg', selected: null},
      ],
      order:[],
      list:null,
      orderCost:0,
    }),
    props: {
    source: String
  },
    methods: {
      toggleMenu () {
        this.menuVisible = !this.menuVisible
      },
      add :function (id) {
        if(parseInt(this.ingredients[id].selected)){
          this.list = []
            if (id in this.order){
              this.order[id].selected += Number.parseInt(this.ingredients[id].selected);
              this.order[id].price += this.ingredients[id].selected * this.ingredients[id].cost *0.01;
            }
            else{
              this.order[id] = ({id: this.ingredients[id].id, selected: Number.parseInt(this.ingredients[id].selected), price: this.ingredients[id].selected * this.ingredients[id].cost *0.01, image: this.ingredients[id].image});
            }
            this.order.forEach(element => {
          if(element != undefined){
            this.list.push(element);
          }
        });
        this.orderCost += Number.parseFloat(this.ingredients[id].selected * this.ingredients[id].cost *0.01);
        }
        this.ingredients[id].selected = null
      },
      del:function(id){
        this.list = []
        this.orderCost -= Number.parseFloat(this.order[id].price);
        this.order[id] = null;
       
         this.order.forEach(element => {
          if(element != undefined){
            this.list.push(element);
          }
        });
           // eslint-disable-next-line no-console
           console.log(this.order,this.list,id);
      },
      remove :function (id) {
        if(parseInt(this.ingredients[id].selected)){
          this.list = []
            if (id in this.order){
                this.order[id].selected -= Number.parseInt(this.ingredients[id].selected);
                this.order[id].price -= this.ingredients[id].selected * this.ingredients[id].cost *0.01;
                this.orderCost -= Number.parseFloat(this.ingredients[id].selected * this.ingredients[id].cost *0.01);
            }
            else{
              alert('Этот товар еще не был добавлен');
            }
            this.order.forEach(element => {
          if(element != undefined){
            this.list.push(element);
          }
        });
        }
        
        this.ingredients[id].selected = null
      },
      pay:function(){
        alert('Функция в данный момент не доступна:)');
      }
    },
  }
</script>

<style lang="scss" scoped>
  .md-app {
    min-height: 800px;
    border: 1px solid rgba(#000, .12);
  }
   // Demo purposes only
  .md-drawer {
    width: 230px;
    max-width: calc(100vw - 225px);
  }
  .md-primary{
      background: rgb(193, 203, 238);
  }
  .md-card {
    width: 320px;
    margin: 4px;
    display: inline-block;
    border-radius: 75px;
    vertical-align: top;
    box-shadow: 0 0px 10px 0 rgba(0,0,0,0.5), 0 0px 10px 0 rgba(0,0,0,0.5), 0 0px 10px 0 rgba(0,0,0,0.5);
  }
.img{
    height: 200px !important;
    width: 90% !important  ;
}
.title-order{
  display: flex;
  height: 64px;
}
.title-order h2{
  padding-top:15px;
  margin: 0 auto;
}
  .group {
    position: relative;
    margin-top: 10px;
    margin-bottom: 30px;
  }
.buttons{
 width: 100%;
 padding-bottom: 10px;
}
.md-raised{
  width: 10%;
  border-radius: 45px;
  box-shadow: 0 16px 24px 2px  rgb(65, 201, 23), 0 6px 30px 5px rgb(65, 201, 23), 0 8px 10px 0  rgb(65, 201, 23);
}

input {
  width: 300px;
  font-size: 16px;
  padding: 10px;
  display: block;
  border: none;
  border-bottom: 1px solid rgb(65, 201, 23);
}
input:focus {
  outline: none;
}
label {
  color: #999;
  font-size: 18px;
  position: absolute;
  pointer-events: none;
  left: 10px;
  top: 15px;
  transition: 0.2s ease all;
  -moz-transition: 0.2s ease all;
  -webkit-transition: 0.2s ease all;
}
input:focus ~ label, input:valid ~ label {
  top: -10px;
  font-size: 14px;
  color: #5264AE;
}
.md-drawer.md-active{
box-shadow: 0 8px 10px 1px rgba(0,0,0,0.14), 0 3px 14px 3px rgba(0,0,0,0.12), 0 4px 5px 0 rgba(0,0,0,0.20);
}

.md-accent{
 background: rgb(248, 105, 105);
 box-shadow: 0 2px 4px 0 rgba(0,0,0,0.14), 0 3px 4px 0 rgba(0,0,0,0.12), 0 1px 5px 0 rgba(0,0,0,0.20);
}

.small-img{
  height: 100px !important;
  width: 100%;
}
.md-list{
  width: 90%; 
  margin-left: 5%;
  box-shadow: 0 2px 4px 0 rgba(0,0,0,0.14), 0 3px 4px 0 rgba(0,0,0,0.12), 0 1px 5px 0 rgba(0,0,0,0.20);
  margin-top: 10px;
}
.md-list-item-text{
  text-align: center;
}


</style>


