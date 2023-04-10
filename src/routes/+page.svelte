<script>
	import Navbar from '../components/Navbar.svelte';
    import Background from '../components/Background.svelte';
</script>

<Navbar/>
<Background />



<div class="divider"></div>

<div id = "home"> 
    <div id = "intro">
        <h1>Building Hackathons for Diversity.</h1> 
        <div class="info-item">
            <h2 id = "description">HackMap will make the hackathon space more equitable for everyone.</h2>
        </div>

        <hr align="center" width="100%">
        <button id = "button1"> <a  href= "locationfinder"> Get Started </a> </button> 
    </div>

    <div class="about">
        <div id = "blurb">
            <h2 id = "abttitle"> Our Purpose </h2> 
            <hr align="left" width="50%">
        </div> 
        <div id = "explanation"> 
            <p>Our mission at HackMap is to ensure that all students across the US can 
                have an equitable and accessible hackathon experience. Our map details the 
                locations of all open in person hackathons on devpost against underprivileged
                areas to help coders find hackathons near them and to help organizers plan 
                hackathons to better serve those who are historically marginalized
                from these experiences.</p>
        </div>
        <div id = "infocards">
            <div id = "card"> 
                <p>Across the United States</p>
                <h3> 53% </h3>
                <p> of high schools do not offer <br> any CS class to students </p> 
            </div>
            <div id = "card"> 
                <p> Schools in low income areas are </p>
                <h3> 4x </h3>
                <p> as likely to not offer any <br>  advanced CS class to students </p> 
            </div>
            <div id = "card"> 
                <p>Black Students have an</p>
                <h3> 11% </h3>
                <p> lower rate of access to CS classes <br> compared to white peers</p> 
            </div>
            
        </div> 
    </div> 
    
</div> 


<style>
        
@import url('https://fonts.googleapis.com/css?family=Open+Sans&display=swap');


h1 {
    font-size: 8em;
    background: linear-gradient(to right, #161e8b 0%, #7966f8 70%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: gradient-animation 5s ease infinite;
    font-weight: 700;
    padding-bottom: 20px;
}


@keyframes gradient-animation {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

#button1{
    border-radius: 15px; 
    border-color: #617af5;
    opacity: 1; 
    background-color: #a599f4; 
    color: rgb(255, 25rgb(9, 39, 74)); 
    width: 200px; 
    height: 50px; 
    z-index: 100;
    text-decoration: none; 
}

#description{
   color: white; 
   font-size: 25px; 
}

.info-item {
    margin-top: 20px;
}

p{
    font-size: 20px; 
}

#abttitle{
    text-align: center;
    font-size: 8em;
    background: linear-gradient(to right, #7966f8 0%, #ffffff 70%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: gradient-animation 5s ease infinite;
    font-weight: 700;
    vertical-align: center; 
}
#intro{
    width: 80%; 
    margin-left: auto;
    margin-right: auto;
    padding-bottom: 10%; 
}

#home{
    display: block;
    position: absolute;
    width: 100%;  
    z-index: 999; 
    background: transparent;
    top: 15%;
    /* center */
    left: 0;
    right: 0;
    margin-left: auto;
    margin-right: auto;
}

#card{
    text-align: center; 
    width: 33%;
    color: #ffffff; 
}

h3{
    color: #ffffff; 
    font-size: 65px; 
    font-weight: bold;
}
.about {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #112a8b;
    padding: 50px 0;
    min-height: 50vh;
    box-shadow: 0 -15px 15px rgba(255, 255, 255, 0.3);
}

#blurb {
    width: 80%;
    margin-bottom: 20px;
}

#explanation {
    width: 80%;
    color: #ffffff;
    text-align: justify;
}

#infocards {
    padding-top: 5%; 
    padding-bottom: 3%; 
    width: 78%;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

</style>



