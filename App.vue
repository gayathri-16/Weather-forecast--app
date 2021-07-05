<template>
  <div id="main" :class="isDay ? 'day' : 'night'" >
   
      <link rel="preconnect" href="https://fonts.gstatic.com">
       <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.10.1/css/all.css">
       <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@600&family=Courgette&family=ABeeZee&family=Castoro:ital@1&display=swap" rel="stylesheet"> 
      <link href="//maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet">  <br>
    

    <div class="wrapper">
  
      <div class="title">
        <h1>Weather App</h1>
      </div>
   
       <div class="box">
         <div></div>
         <div></div>
         <div></div>
         <div></div>
         <div></div>
         <div></div>
         <div></div>
         <div></div>
         <div></div>
         <div></div>
       </div>
    
    
    <div class="search-box" @click="showSlide=!showSlide" v-if="showSlide===false">
        <label for="city-name">Search for places</label> <div style="margin-top:-2rem;margin-left:25rem;"> <div style="width:35px;height:35px;border-radius:50%;background:gray;position:relative;"><i class="fas fa-crosshairs" style="color:white;position:absolute;padding:4px 5px 0 5px;font-size:25px;"></i></div></div>
    </div>
     <transition name="navbar">
    <div class="slidebar" v-if="showSlide">
      <form v-on:submit.prevent="getWeather">
       <input 
          name="city-name"
        type="text"  
        class="search-bar" 
        placeholder="Enter City Name" 
        v-model="query"
      /> </form>
      <button class="search" @click="showSlide=!showSlide" v-if="showSlide===true"><i class="fas fa-search" ></i></button>
    </div>
     </transition>
      <div class="card" v-if="visible">
       <div class="weather-wrap" >
         
         <div>
           <div style="width:200px;height:200px;position:absolute;margin-top:3rem;margin-left:-1rem;">
           <span class="cloudy" v-if="cloudy">
                  <div class="cloud one"></div>
                  <div class="cloud two"></div>
                  <div class="cloud three"></div>
              </span>
         </div>

          <div style="width:200px;height:200px;position:absolute;margin-top:3rem;margin-left:4.2rem;">
     
           <span class="night" v-if="haze">
             <div class="sky">
                <div class="drop"></div>
             </div>
           </span>
        
         </div>

          <div style="width:200px;height:200px;position:absolute;margin-top:3rem;margin-left:4.2rem;">
           <span class="rainy" v-if="rainy">
               <div style="margin-left:-8rem;margin-top:-5rem" class="sky"></div>
                     <div style="margin-left:7rem;margin-top:-3rem" class="blue sky"></div>
             <div class="rain">
             <div class="rain"></div>
              <div class="rain"></div>
              <div class="rain"></div>
                    <div class="rain"></div>  
                        <div class="rain"></div>
                          <div class="rain"></div>
                              <div class="rain"></div>
                                <div class="rain"></div>
                                    <div class="rain"></div>
                                      <div class="rain"></div>
                                         <div class="rain"></div>
                </div>                      
              </span>
         </div>

          <div style="width:200px;height:200px;position:absolute;margin-top:10rem;margin-left:4.2rem;">
           <span class="sunny" v-if="sunny">
                <div class="center">
                  <div class="sky"></div>
                  <ul class="sun">
                    <li></li>
                    <li></li>
                    <li></li>
                    <li></li>
                    <li></li>
                    <li></li>
                    <li></li>
                    <li></li>
                  </ul>
                </div>
               </span>
         </div>
           <div style="width:200px;height:200px;position:absolute;margin-top:3rem;margin-left:4.2rem;">
           <span class="snowy" v-if="snowy">
             <div style="margin-left:-8rem;margin-top:-5rem" class="sky"></div>
             <div style="margin-left:7rem;margin-top:-3rem" class="blue sky"></div>
               
              <div class="flake">
                <div class="snow"></div>
                <div class="snow"></div>
                <div class="snow"></div>
                <div class="snow"></div>
                <div class="snow"></div>
                <div class="snow"></div>
                <div class="snow"></div>
                <div class="snow"></div>
                <div class="snow"></div>
                <div class="snow"></div>
                <div class="snow"></div>
                <div class="snow"></div>
           </div>
             
              </span>
         </div>

        </div>

      <div class="weather-box">
        <div class="temp" v-if="showFaren===false"> {{weather.temperature}} &deg; c</div>
         
         <div class="temp" v-if="showCelsius"> {{weather.temperature}} &deg; c</div>
          <div class="temp" v-else-if="showFaren"> {{Math.round((weather.temperature)*1.8)+32}} &deg; F</div>
          <div style="margin-left:3.5rem;text-align:center;" class="weather"> {{weather.description}}</div>
      </div>
        <div class="location-box">
           <div class="date"> {{dateBuilder()}} </div>
         <div class="location"> 
          <i class="fas fa-map-marker-alt" style="color:black;font-size:25px;margin-left:-2rem;"></i>
            {{weather.cityName}}   {{weather.country}}</div>
        </div>
  </div>
</div>
  
  <div class="deg-convert">
     <div class="celsius" @click="showCelsius=!showCelsius"> <sup>&deg;</sup> C</div>
     <div class="faren" @click="showFaren=!showFaren"><sup>&deg;</sup>F</div>
     
  </div>
      
  <div class="weather_forecast">
   <div class="daily_weather" >
        <h3>Tomorrow</h3>
        <img src='./assets/01n.gif' alt="">
     <div class="max-temp">
       <h4 v-if="showFaren===false"><strong>{{weather.highTemp}}&deg;C</strong></h4>
        <h4 v-if="showCelsius"><strong>{{weather.highTemp}}&deg;C</strong></h4>
       
       <h4 v-else-if="showFaren"><strong>{{Math.round((weather.highTemp)*1.8)+32}}&deg;F</strong></h4>
        
     </div>
     <span>
       <div class="min-temp">
 
         <h4 v-if="showFaren===false" style="font-size:16px;">{{weather.lowTemp}}&deg;C</h4>
          <h4 v-if="showCelsius"><strong>{{weather.lowTemp}}&deg;C</strong></h4>
   
          <h4 v-else-if="showFaren"><strong>{{Math.round((weather.lowTemp)*1.8)+32}}&deg;F</strong></h4>
              
       </div>
     </span>
   </div>

    <div class="daily_weather" >
            <h3>Mon,5 Jul</h3>
        <img src='./assets/09d.gif' alt="">
     <div class="max-temp">
     <h4 v-if="showFaren===false"><strong>{{weather.highTemp}}&deg;C</strong></h4>
        <h4 v-if="showCelsius"><strong>{{weather.highTemp}}&deg;C</strong></h4>
       
       <h4 v-else-if="showFaren"><strong>{{Math.round((weather.highTemp)*1.8)+32}}&deg;F</strong></h4>
        
     </div>
     <span>
       <div class="min-temp">
          <h4 v-if="showFaren===false" style="font-size:16px;">{{weather.lowTemp}}&deg;C</h4>
          <h4 v-if="showCelsius"><strong>{{weather.lowTemp}}&deg;C</strong></h4>
   
          <h4 v-else-if="showFaren"><strong>{{Math.round((weather.lowTemp)*1.8)+32}}&deg;F</strong></h4>
            
       </div>
     </span>
   </div>

    <div class="daily_weather" >
            <h3>Tue,6 Jul</h3>
        <img src='./assets/02d.gif' alt="">
     <div class="max-temp">
       <h4 v-if="showFaren===false"><strong>{{weather.highTemp}}&deg;C</strong></h4>
        <h4 v-if="showCelsius"><strong>{{weather.highTemp}}&deg;C</strong></h4>
       
       <h4 v-else-if="showFaren"><strong>{{Math.round((weather.highTemp)*1.8)+32}} &deg;F</strong></h4>
        
     </div>
     <span>
       <div class="min-temp">
        <h4 v-if="showFaren===false" style="font-size:16px;">{{weather.lowTemp}} &deg;C</h4>
          <h4 v-if="showCelsius"><strong>{{weather.lowTemp}} &deg;C</strong></h4>
   
          <h4 v-else-if="showFaren"><strong>{{Math.round((weather.lowTemp)*1.8)+32}}&deg;F</strong></h4>
            
         </div>
     </span>
   </div>

    <div class="daily_weather" >
            <h3>Wed,7 Jul</h3>
        <img src='./assets/04n.gif' alt="">
     <div class="max-temp">
    <h4 v-if="showFaren===false"><strong>{{weather.highTemp}}&deg;C</strong></h4>
        <h4 v-if="showCelsius"><strong>{{weather.highTemp}}&deg;C</strong></h4>
       
       <h4 v-else-if="showFaren"><strong>{{Math.round((weather.highTemp)*1.8)+32}}&deg;F</strong></h4>
        
     </div>
     <span>
       <div class="min-temp">
        <h4 v-if="showFaren===false" style="font-size:16px;">{{weather.lowTemp}}&deg;C</h4>
          <h4 v-if="showCelsius"><strong>{{weather.lowTemp}}&deg;C </strong></h4>
   
          <h4 v-else-if="showFaren"><strong>{{Math.round((weather.lowTemp)*1.8)+32}} &deg;F</strong></h4>
            
       </div>
     </span>
   </div>

    <div class="daily_weather" >
            <h3>Thu,8 Jul</h3>
        <img src='./assets/09n.gif' alt="">
     <div class="max-temp">
       <h4 v-if="showFaren===false"><strong>{{weather.highTemp}}&deg;C</strong></h4>
        <h4 v-if="showCelsius"><strong>{{weather.highTemp}}&deg;C</strong></h4>
       
       <h4 v-else-if="showFaren"><strong>{{Math.round((weather.highTemp)*1.8)+32}}&deg;F</strong></h4>
        
     </div>
     <span>
       <div class="min-temp">
         <h4 v-if="showFaren===false" style="font-size:16px;">{{weather.lowTemp}}&deg;C</h4>
          <h4 v-if="showCelsius"><strong>{{weather.lowTemp}}&deg;C</strong></h4>
   
          <h4 v-else-if="showFaren"><strong>{{Math.round((weather.lowTemp)*1.8)+32}}&deg;F</strong></h4>
           
       </div>
     </span>
   </div>
  </div>

 <div class="today">  <h5 class="grid" style="margin-top:-5rem;text-align:left;font-size:20px;">Today's Highlight</h5></div> <br><br>
      
      <div class="grid">
        <div class="highlight_container">
          <h3 class="hilights">Wind status</h3>
          <h1 style="margin-top:-0.5rem;"><strong>{{weather.windstatus}}</strong><span>mph</span></h1>
          <div class="plane" style=""> <i class="fas fa-paper-plane" style="position:absolute;margin-top:0.5rem;margin-left:-0.5rem; color:black;"> </i></div><br><h5 class="wsw" style="">WSW</h5>
        </div>
        <div class="highlight_container1">
           <h3>Humidity</h3>
            <h1 style="margin-top:-0.5rem;"><strong>{{weather.humidity}}</strong>%</h1>
            <div style="margin-top:-3rem; display:flex; flex-direction:row;align-items:center;justify-content:space-between;padding:2.5rem;"><span style="">0</span> <span style="">50</span><span>100</span></div>
            <div style="width:80%;height:8px;background:white;margin-left:2.5rem;margin-top:-2rem;border-radius:10px;"><div style="width:50%;height:8px;border-radius:10px;background:yellow;"></div></div> <sub style="text-align:right;margin-right:3rem;margin-top:-1rem;">%</sub>
        </div>
        <div class="highlight_container2">
           <h3>Visibility</h3>
            <h1 style="margin-top:-1rem;"><strong>{{weather.visibility}}</strong>miles</h1>
           
        </div>
        <div class="highlight_container3">
           <h3>Air Pressure</h3>
            <h1 style="margin-top:-1rem;"><strong>{{weather.pressure}}</strong>mb</h1>
           
        </div>
      </div>
  </div>
 </div>
</template>


<script>


  export default {
    name:"App",
    components:{
   },
    data(){
      return{
        isDay:true,
        showCelsius:false,
        showFaren:false,
        showSlide:false,
        visible:true,
        haze:false,
        snowy:false,
        cloudy:false,
        sunny:false,
        rainy:false,
        data:null,
    
         

             weather:{
             cityName:'Tamil Nadu',
             country:'IN',
             temperature:30,
             description:'Mostly Sunny',
             lowTemp:"19",
             highTemp:"30",
             humidity:0,
             windstatus:7,
             visibility:0,
             pressure:998,
             icon:"01n",
          

             },

      };
    },     

    methods:{
      getWeather: async function(){
        console.log(this.query);
        const key="25473f353e7c959aeb1bf093f8cc8154"
       const baseURL=`https://api.openweathermap.org/data/2.5/weather?q=${this.query}&appid=${key}&units=metric`;
        
        const response =await fetch(baseURL);
        const data = await response.json();
        console.log(data);
        this.query="";
        this.weather.cityName=data.name;
        this.weather.country= data.sys.country;
        this.weather.temperature=Math.round(data.main.temp);
        this.weather.description=data.weather[0].description;
        this.weather.lowTemp=Math.round(data.main.temp_min);
        this.weather.highTemp=Math.round(data.main.temp_max);
        this.weather.pressure=data.main.pressure;
        this.weather.windstatus=Math.round(data.wind.speed);
        this.weather.visibility=data.visibility;
        this.weather.humidity=data.main.humidity;

       const timeOfDay = data.weather[0].icon;
       
        //check for time of day
        if(timeOfDay.includes("n")){
          this.isDay=false;
        }else {
          this.isDay=true;
        }

        const mainWeather = data.weather[0].main;
        if(mainWeather.includes("Clouds")){
           this.stormy=false;
           this.snowy=false;
           this.cloudy=true;
           this.sunny=false;
           this.rainy=false;
           this.haze=true;
        }
          
           if(mainWeather.includes("Clouds") && this.isDay){
           this.stormy=false;
           this.snowy=false;
           this.cloudy=false;
           this.sunny=true;
           this.rainy=false;
           this.clearNight=false;
        }
           if(mainWeather.includes("Clouds")){
           this.stormy=false;
           this.snowy=false;
           this.cloudy=true;
           this.sunny=false;
           this.rainy=false;
           this.haze=false;
        }
        if(mainWeather.includes("Snow")){
           this.stormy=false;
           this.snowy=true;
           this.cloudy=false;
           this.sunny=false;
           this.rainy=false;
           this.clearNight=false;
        }
           if(mainWeather.includes("Rain")){
           this.stormy=false;
           this.snowy=false;
           this.cloudy=false;
           this.sunny=false;
           this.rainy=true;
           this.clearNight=false;
        }

        this.visible = true;
                
      },
         dateBuilder(){
           let d = new Date();
           let months = ["January", "February", "March", "April", "May", "June", "July", "August",
           "September", "October", "November", "December"];
           let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thurseday", "Friday", "Saturday"];

           let day = days[d.getDay()];
           let date = d.getDate();
           let month = months[d.getMonth()];
           let year = d.getFullYear();
            
            return `${day} ${date} ${month} ${year}`;
         }
     
    },
     
  };
</script>

<style>
@import './assets/style.css';
</style>