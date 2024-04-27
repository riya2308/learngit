.sidebar {
    width: 0;
    height: 100vh;
    position: fixed;
    top: 0;
    left: 0;
    background-color: #333;
    overflow-x: hidden;
    transition: width 0.5s;
    z-index: 1000;
}

.sidebar a {
    padding: 10px 15px;
    text-decoration: none;
    font-size: 25px;
    color: white;
    display: block;
    transition: 0.3s;
}

.sidebar a:hover {
    color: #f1f1f1;
}

.sidebar:hover {
    width: 250px;
}


<div class="sidebar">
    <a href="#">Home</a>
    <a href="#">About</a>
    <a href="#">Services</a>
    <a href="#">Contact</a>
</div>
