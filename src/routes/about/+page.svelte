<script>
	import Navbar from '../../components/Navbar.svelte';
</script>

<Navbar/>
<div id = "about">
    <h1 style="" id = "title"> About </h1> 
        <hr align="left" width="50%">
</div>

<style> 
#about{
    display: block;
    position: absolute;
    width: 80%;  
    z-index: 999; 
    background: transparent;
    top: 15%;
    /* center */
    left: 0;
    right: 0;
    margin-left: auto;
    margin-right: auto;
}

#title{
    padding-top: 10%; 
}
</style> 