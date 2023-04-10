<div bgcolor = #152238> 
    <nav id = "navbar" > 
        <ul>
            <li><a class="nav-link" href="/">Home</a> </li> 
            <li><a class="nav-link" href="/locationfinder">Location Finder</a> </li> 
        </ul> 
    </nav> 
</div> 

<style>
*{
    margin: 0px;
    padding: 0px;
}

body {
    font-family: 'Open Sans' , sans-serif;
    font-size: 16px;
    text-align: center;
    color: #000000;
    background-color: #152238;
}

#navbar{
    background-color: rgba(9, 20, 67, 0.9);
    color: #ffffff;
    display: inline-block; 
    position: fixed;
    top: 0;
    width: 100%;
    height: 125px; 
    z-index: 999999999999;
    text-align: left;
    vertical-align: center; 
    overflow: hidden;
}

#navbar li:hover{
    background-color: #23395d;
    color:  #ffffff;
    text-decoration:underline;

}

#navbar li{
    display: inline-block;
    color:  rgba(43, 71, 116, 0.75); 
    vertical-align: center; 
    padding-left: 40px; 
    padding-right: 40px; 
    padding-top: 50px; 
    padding-bottom: 50px;
    transition: background-color 0.3s ease, color 0.3s ease;
}

#navbar li:hover {
    background-color: #23395d;
    color: #ffffff;
    text-decoration: none;
    box-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
}

a {
    text-decoration: none;
    transition: color 0.3s ease;
    color: #7966f8;
    font-size: 1.3rem;
}

a:hover {
    color: #ffffff;
    text-decoration: none;
}

.nav-link {
    display: inline-block;
    position: relative;
    padding-bottom: 5px;
}

.nav-link::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    width: 0;
    height: 2px;
    background-color: #ffffff;
    transition: width 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.nav-link:hover::after {
    width: 100%;
}
</style>