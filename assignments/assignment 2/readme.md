import logo from './logo.svg';
//  import './App.css';
import Header from "./components/Header";
import Cart from "./components/Cart";
import Total from "./components/Total";
import { useState } from "react";
let products =[
  {
    name:"redmi 4",
    path:"https://m.media-amazon.com/images/I/318rhU1q7qL._SX300_SY300_QL70_FMwebp_.jpg",
    price:9000,
  },
  {
    name:"oppoA1k",
    path:"https://img3.gadgetsnow.com/gd/images/products/additional/original/G108890_View_1/mobiles/smartphones/oppo-a1k-32-gb-black-2-gb-ram-.jpg",
    price:10000,
  },
  {
    name:"realme c2",
    path:"https://img1.gadgetsnow.com/gd/images/products/additional/large/G110680_View_1/mobiles/smartphones/realme-c2-16gb-black-2gb-ram-.jpg",
    price:8000,
  },
  {
    name:"IQZ6",
    path:"https://img4.gadgetsnow.com/gd/images/products/additional/large/G353511_View_1/mobiles/smartphones/iqoo-z6-5g-128-gb-chromatic-blue-6-gb-ram-.jpg",
    price:7000,
  },
  {
    name:"galaxyj6",
    path:"https://images.meesho.com/images/products/241167461/xyalv_512.webp",
    price:11000,
  },
]
function App(){
  let [cartItems,setCartItems]=useState(0)
  let[total,setTotal]=useState();
  function addCartItem(){
    setCartItems(cartItems + 1)
  }
  function decreaseCartItem(){
    setCartItems(cartItems-1)
  }
  function changeTotal(t){
    setTotal(total+t);
  }
  return (
    <>
    <Header cartItems={cartItems}/>
    <main>
    {products.map((e,i)=> 
    <Cart 
    changeTotal={changeTotal}
    addCartItem={addCartItem} 
    decreaseCartItem={decreaseCartItem} 
    key={i} 
    products={e}/>)}
    </main>
    <Total />
    </>
  )
}
export default App;
