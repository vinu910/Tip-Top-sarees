<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tip Top Fashion Collection</title>
<style>
  body {
    font-family: "Poppins", sans-serif;
    background: linear-gradient(135deg, #fff0f5, #ffe6f0);
    margin: 0;
    padding: 0;
    text-align: center;
    min-height: 100vh;
  }

  header {
    background: linear-gradient(90deg, #ff66a6, #ff3366);
    color: white;
    padding: 30px 10px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.3);
  }

  h1 {
    margin: 0;
    font-size: 2.5rem;
    text-shadow: 2px 2px 5px rgba(0,0,0,0.4);
  }
  h2 {
    font-size: 1rem;
    margin-top: 10px;
    color: #fffde7;
  }

  nav {
    background: #fff;
    display: flex;
    justify-content: center;
    gap: 12px;
    padding: 12px 0;
    box-shadow: 0 3px 5px rgba(0,0,0,0.1);
  }
  nav button {
    background: #ff3366;
    color: white;
    border: none;
    border-radius: 20px;
    padding: 8px 18px;
    cursor: pointer;
    font-weight: bold;
    transition: 0.3s;
  }
  nav button.active {
    background: black;
  }

  .color-buttons {
    margin: 25px 0;
  }
  .color-buttons button {
    width: 90px;
    height: 38px;
    margin: 6px;
    border: none;
    border-radius: 8px;
    font-weight: bold;
    text-transform: capitalize;
    cursor: pointer;
    color: white;
    transition: 0.3s;
  }
  .color-buttons button:hover { transform: scale(1.08); }
  .red{background:red;} .blue{background:blue;}
  .green{background:green;} .purple{background:purple;}
  .orange{background:orange;} .white{background:#ddd;color:black;}
  .mehroon{background:#800000;} .pink{background:hotpink;} .grey{background:grey;}

  .gallery {
    display: grid;
    grid-template-columns: repeat(auto-fit,minmax(220px,1fr));
    gap: 20px;
    padding: 0 20px 50px;
    max-width: 1100px;
    margin: auto;
  }
  .gallery img {
    width: 100%;
    height: 280px;
    object-fit: cover;
    border-radius: 12px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.3);
    cursor: pointer;
    transition: transform 0.3s;
  }
  .gallery img:hover { transform: scale(1.06); }

  /* Zoom Modal */
  .modal {
    display: none;
    position: fixed;
    z-index: 1000;
    left: 0; top: 0;
    width: 100%; height: 100%;
    background: rgba(0,0,0,0.8);
  }
  .modal-content {
    max-width: 80%;
    max-height: 80%;
    margin: 80px auto;
    display: block;
    border-radius: 10px;
  }
  .close {
    position: absolute;
    top: 30px;
    right: 50px;
    color: white;
    font-size: 40px;
    cursor: pointer;
  }
</style>
</head>
<body>

<header>
  <h1>💃 Tip Top Fashion Collection 💃</h1>
  <h2>Nagloi, Near Police Station — Zarur Aaiye 7982934770 </h2>
  <h3>Hamare yaha har Desing ke saree, Gown or Lahnga rakhte hai💐</h3>
</header>

<nav>
  <button class="cat active" data-cat="saree">Saree</button>
  <button class="cat" data-cat="gown">Gown</button>
  <button class="cat" data-cat="lehenga">Lehenga</button>
</nav>

<div class="color-buttons" id="colorButtons"></div>

<div class="gallery" id="gallery"></div>

<!-- Zoom Modal -->
<div id="imageModal" class="modal">
  <span class="close" onclick="closeModal()">&times;</span>
  <img id="modalImg" class="modal-content">
</div>

<script>
const products = {
  saree: {
    red: [
      "https://m.media-amazon.com/images/I/71zD6LNDRdL._SX679_.jpg",
      "https://m.media-amazon.com/images/I/61t5Rq80yeL._SY741_.jpg",
      "https://m.media-amazon.com/images/I/617d9lB90xL._SY741_.jpg",
      "https://m.media-amazon.com/images/I/617YAcTci7L._SX569_.jpg"
    ],
    blue: [
      "https://m.media-amazon.com/images/I/61oBB8q1FlL._SY741_.jpg",
      "https://encrypted-tbn1.gstatic.com/shopping?q=tbn:ANd9GcSuLzUWa6UZDrT1vSRgt68CZSVOYIxuAl9Pd18p8MiNsuqPUuL2MhlgdDnbNcJeQPxFaPzDoLWQSpuCYAUzM-UAN0NKEuPhS1HM3nlEVOvG0my6nkLFSJK86A&usqp=Cac",
      "https://encrypted-tbn3.gstatic.com/shopping?q=tbn:ANd9GcSKqdcCBX0hTNNGopmqocLnZlvrFgX11rVh3xEltUKbhDzjeoEASJ8027p01x14UpqGwD2hQLlkuEzEaCKQESYMj9rcxCx9yTM4IL-ZtLplq2n-y93RCrBjEig&usqp=Cac",
      "https://encrypted-tbn1.gstatic.com/shopping?q=tbn:ANd9GcQyqtUGj3Tc2EG7GYmKBiZRYJf1MibP3c2_4clRtmAdcLRBAyR1nd4y9kogW1EoQj5V_TJmLEezz4AVc0hHmtZyjnsixsZcmu_pEvij7Lv43gZjliJZENVkHxs8mW8LkXz2c5adZevqGyU&usqp=Cac"
    ],
    green: [
      "https://encrypted-tbn2.gstatic.com/shopping?q=tbn:ANd9GcQ1rc2-j46Ngbxfk6Zeat8fbXeRiJ2bzbI1zpYkwtGx8ELNgPnf0BlWMTY6ddwqNzVYVR5xo4r99aANJb5UaW6lP4wpNv1pQKU5ilbGLJokiYO9eDcrMMPQ57A&usqp=Cac",
      "https://encrypted-tbn1.gstatic.com/shopping?q=tbn:ANd9GcQ7-fB61n5mIOIqHgEBO80EaSs3HAc0Hm_fHAIOVKTZP_IURtR1n1IqnW7iscE0KYBRMkvnxx_futbiW4smjOGVRTFM9BVFLpQqlq49VU8m8EU--qflBGnXCdE&usqp=Cac",
      "https://encrypted-tbn3.gstatic.com/shopping?q=tbn:ANd9GcSNUoyKYTz6tWIcfee9VMqCDIfu-MEU_Kgdd872cU668DqzmP7ZN9LkLe6ptV9dlztTXJYrFAsXQfAmbM1OImmE3uu5ubYL7pRWGGmeV-w4",
      "https://encrypted-tbn1.gstatic.com/shopping?q=tbn:ANd9GcTks4YcybVlMbITyTbxBEearyet-_xAQMpVkjzoNnjeaE4NvZCy9slbHNgcBKTuyQrdm2pNeetwQXs1QuU0SDhqW3xDV028qUkY7GM5p9hd4P9XpwbrtwWJUw"
    ],
    purple: [], orange: [], white: [], mehroon: [], pink: [], grey: []
  },

  gown: {
    red: ["https://images.unsplash.com/photo-1605949405968-0398e6e0d685"],
    blue: ["https://images.unsplash.com/photo-1593032465173-8e7d4e1d40e7"],
    green: ["https://images.unsplash.com/photo-1620915418412-5cf4a4f54324"],
    purple: ["https://images.unsplash.com/photo-1616469821191-c7c39524359c"],
    orange: [], white: [], mehroon: [], pink: [], grey: []
  },

  lehenga: {
    red: ["https://images.unsplash.com/photo-1619165168589-7d88ff3f8d5f"],
    blue: ["https://images.unsplash.com/photo-1632969882064-692f418d0ad5"],
    green: ["https://images.unsplash.com/photo-1631216971251-3e8e9c765cfb"],
    purple: ["https://images.unsplash.com/photo-1613670673551-04a097c13d05"],
    orange: [], white: [], mehroon: [], pink: [], grey: []
  }
};

const colors = ["red","blue","green","purple","orange","white","mehroon","pink","grey"];
const colorDiv = document.getElementById("colorButtons");
const gallery = document.getElementById("gallery");
let currentCat = "saree";

colors.forEach(c=>{
  const btn=document.createElement("button");
  btn.className=c;
  btn.textContent=c;
  btn.onclick=()=>showGallery(currentCat,c);
  colorDiv.appendChild(btn);
});

document.querySelectorAll(".cat").forEach(btn=>{
  btn.onclick=()=>{
    document.querySelectorAll(".cat").forEach(b=>b.classList.remove("active"));
    btn.classList.add("active");
    currentCat=btn.dataset.cat;
    showGallery(currentCat,"red");
  }
});

function showGallery(cat,color){
  gallery.innerHTML="";
  const imgs=products[cat][color];
  if(!imgs || imgs.length===0){
    gallery.innerHTML=`<h3 style='color:${color};'>No ${cat} available in ${color} color yet!</h3>`;
    return;
  }
  imgs.forEach(url=>{
    const img=document.createElement("img");
    img.src=url;
    img.onclick=()=>openModal(url);
    gallery.appendChild(img);
  });
}

function openModal(src){
  const modal=document.getElementById("imageModal");
  const modalImg=document.getElementById("modalImg");
  modal.style.display="block";
  modalImg.src=src;
}

function closeModal(){
  document.getElementById("imageModal").style.display="none";
}

showGallery("saree","red");
</script>
</body>
</html>
