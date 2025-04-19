<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Menú Restaurante</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background: url('https://images.unsplash.com/photo-1551218808-94e220e084d2?auto=format&fit=crop&w=1400&q=80') no-repeat center center fixed;
      background-size: cover;
      color: #fff;
    }
    h1, h2 { text-align: center; text-shadow: 1px 1px 2px #000; }
    .menu-item {
      background: rgba(0,0,0,0.7);
      padding: 15px;
      margin-bottom: 10px;
      border-radius: 10px;
      box-shadow: 0 0 8px rgba(0,0,0,0.5);
    }
    .precio { font-weight: bold; color: #90ee90; }
    .boton {
      display: block;
      margin: 15px auto;
      padding: 10px 20px;
      background: #28a745;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
    }
    .contador { margin-top: 10px; font-weight: bold; }
    .link-pago {
      display: block;
      margin: 10px auto;
      padding: 10px;
      background: #007bff;
      color: white;
      text-decoration: none;
      border-radius: 5px;
      max-width: 300px;
      text-align: center;
    }
    .categoria {
      background-color: rgba(255,255,255,0.2);
      padding: 10px;
      border-radius: 5px;
      margin-top: 30px;
      font-weight: bold;
      font-size: 18px;
      text-shadow: 1px 1px 2px #000;
    }
    #total, #mediosPago, #numeros {
      text-align: center;
      margin-top: 20px;
      font-weight: bold;
      display: none;
      text-shadow: 1px 1px 2px #000;
    }
    input, textarea {
      display: block;
      width: 90%;
      max-width: 400px;
      margin: 10px auto;
      padding: 10px;
      border-radius: 5px;
      border: none;
      font-size: 16px;
    }
    #WHATSAPP {
      display: none;
      margin: 15px auto;
      background: #25d366;
      padding: 12px;
      color: white;
      border-radius: 5px;
      text-align: center;
      font-weight: bold;
      text-decoration: none;
      max-width: 300px;
    }
  </style>
</head>
<body>
  <h1>Menú Restaurante</h1>
  <div id="menu"></div>
  <div id="total">Total a pagar: $<span id="totalValor">0</span></div>
  <button class="boton" onclick="finalizarCompra()">Finalizar Compra</button>
  <button class="boton" onclick="reiniciar()">Reiniciar</button>

  <!-- Extras -->
  <input type="text" id="direccion" placeholder="Escribe tu dirección de entrega..." />
  <textarea id="comentario" placeholder="¿Algún comentario adicional?"></textarea>
  <a id="WHATSAPP" href="#" target="_blank">Enviar pedido por WhatsApp</a>

  <div id="mediosPago">
    <h2>Medios de Pago</h2>
    <a href="intent://send?phone=+573152553101#Intent;scheme=nequi;package=com.nequi.mobile.app;end" class="link-pago">Pagar con Nequi</a>
    <a href="intent://send?phone=+573152553101#Intent;scheme=daviplata;package=com.davivienda.daviplata;end" class="link-pago">Pagar con Daviplata</a>
    <div id="numeros">
      <p>📱 3152553101</p>
    </div>
  </div>

  <script>
    const menu = [
      { categoria: "Hamburguesas", nombre: "Hamburguesa Sencilla", descripcion: "Carne artesanal(100g), queso doble crema, verduras, salsas, pan artesanal.", precio: 10000 },
      { categoria: "Hamburguesas", nombre: "Hamburguesa De Patakon", descripcion: "Carne artesanal(100g), queso, verduras, salsas, patacón maduro.", precio: 13000 },
      { categoria: "Hamburguesas", nombre: "Hamburguesa Especial", descripcion: "Carne(150g), tocineta, huevo, queso, verduras, salsas, pan.", precio: 15000 },
      { categoria: "Hamburguesas", nombre: "Hamburguesa Tetakeo", descripcion: "Carne y pechuga (150g c/u), tocineta, huevo, verduras, pan.", precio: 22000 },
      { categoria: "Perros Calientes", nombre: "Perro Caliente", descripcion: "Salchicha americana, cebolla, papas, queso, salsas, pan.", precio: 8000 },
      { categoria: "Perros Calientes", nombre: "Perro Especial Mechiperro", descripcion: "Salchicha, carne esmechada, papas, queso, verduras, pan.", precio: 15000 },
      { categoria: "Salchipapas", nombre: "Salchipapa Sencilla", descripcion: "Salchicha, papas, queso, salsas.", precio: 10500 },
      { categoria: "Salchipapas", nombre: "Salchipapa Especial", descripcion: "Carne esmechada, salchicha, papas, verduras, huevo codorniz.", precio: 20500 },
      { categoria: "Salchipapas", nombre: "Salchipapa Especial para dos", descripcion: "Carne, salchichas, papas dobles, verduras, huevo codorniz.", precio: 26500 },
      { categoria: "Salchipapas", nombre: "Salchipapa Especial de Pollo", descripcion: "Pollo, carne, salchicha, papas, verduras, huevo codorniz.", precio: 23500 },
      { categoria: "Otros", nombre: "Picada", descripcion: "Carnes surtidas, papas, verduras, salsas, huevo codorniz.", precio: 38500 },
      { categoria: "Otros", nombre: "Mazorca", descripcion: "Carnes, papas, queso, maíz, salsas.", precio: 38500 },
      { categoria: "Otros", nombre: "Papas a la Francesa (Porción)", descripcion: "Porción de papas a la francesa", precio: 7000 },
      { categoria: "Bebidas", nombre: "Bebida Coca-Cola", descripcion: "Coca-Cola personal 500ml", precio: 4000 },
      { categoria: "Bebidas", nombre: "Bebida Sprite", descripcion: "Sprite personal 400ml", precio: 4000 },
      { categoria: "Bebidas", nombre: "Bebida Cuatro", descripcion: "Cuatro personal 400ml", precio: 4000 },
      { categoria: "Bebidas", nombre: "Bebida Kola Román", descripcion: "Kola Román personal 400ml", precio: 4000 },
      { categoria: "Combos", nombre:"Combo Sencillo", descripcion: "Hamburguesa sencilla + bebida + papas.", precio: 20000 },
      { categoria: "Combos", nombre:"Combo Especial", descripcion: "Hamburguesa especial + bebida + papas.", precio: 26000 },
      { categoria: "Combos", nombre:"Combo Tetakeo", descripcion: "Hamburguesa Tetakeo + bebida + papas.", precio: 30000 },
      { categoria: "Promociones", nombre: "Promo Martes: 2 Hamburguesas Sencillas", descripcion: "2 Hamburguesas por $18.000", precio: 18000 },
      { categoria: "Promociones", nombre: "Promo Martes: 2 Perros Calientes", descripcion: "2 Perros Calientes por $12.000", precio: 12000 },
      { categoria: "Promociones", nombre: "Promo Martes: Mazorcada Sencilla", descripcion: "Mazorcada sencilla por $22.500", precio: 22500 }
    ];

    const contenedorMenu = document.getElementById("menu");
    let categoriaActual = "";
    let total = 0;
    const cantidades = new Array(menu.length).fill(0);

    menu.forEach((item, index) => {
      if (item.categoria !== categoriaActual) {
        const cat = document.createElement("div");
        cat.className = "categoria";
        cat.textContent = item.categoria;
        contenedorMenu.appendChild(cat);
        categoriaActual = item.categoria;
      }
      const div = document.createElement("div");
      div.className = "menu-item";
      div.innerHTML = `
        <h3>${item.nombre}</h3>
        <p>${item.descripcion}</p>
        <p class='precio'>$${item.precio.toLocaleString()}</p>
        <p class='contador'>Cantidad: <span id="cantidad-${index}">0</span></p>
        <button onclick="agregar(${item.precio}, ${index})">Agregar</button>
        <button onclick="quitar(${item.precio}, ${index})">Quitar</button>
      `;
      contenedorMenu.appendChild(div);
    });

    function actualizarTotal() {
      document.getElementById("totalValor").textContent = total.toLocaleString();
    }

    function agregar(precio, index) {
      total += precio;
      cantidades[index]++;
      document.getElementById(`cantidad-${index}`).textContent = cantidades[index];
      document.getElementById("total").style.display = "block";
      actualizarTotal();
      enviarPorWhatsApp();
    }

    function quitar(precio, index) {
      if (cantidades[index] > 0) {
        total -= precio;
        cantidades[index]--;
        document.getElementById(`cantidad-${index}`).textContent = cantidades[index];
        actualizarTotal();
        enviarPorWhatsApp();
      }
    }

    function finalizarCompra() {
      document.getElementById("mediosPago").style.display = "block";
      document.getElementById("numeros").style.display = "block";
      document.getElementById("WHATSAPP").style.display = "block";
      enviarPorWhatsApp();
      window.scrollTo(0, document.body.scrollHeight);
    }

    function enviarPorWhatsApp() {
      let mensaje = "*🛍️ Pedido desde el Menú:*%0A%0A";
      for (let i = 0; i < menu.length; i++) {
        if (cantidades[i] > 0) {
          mensaje += `✅ ${menu[i].nombre} x${cantidades[i]} = $${(menu[i].precio * cantidades[i]).toLocaleString()}%0A`;
        }
      }
      mensaje += `%0A💰 *Total a pagar:* $${total.toLocaleString()}%0A`;

      const direccion = document.getElementById("direccion").value.trim();
      const comentario = document.getElementById("comentario").value.trim();

      if (direccion) mensaje += `%0A📍 *Dirección:* ${direccion}`;
      if (comentario) mensaje += `%0A📝 *Comentario:* ${comentario}`;

      mensaje += `%0A%0A🚚 Por favor confirmar disponibilidad.`;

      const telefono = "573152553101";
      const url = `https://wa.me/${telefono}?text=${mensaje}`;
      document.getElementById("WHATSAPP").href = url;
    }

    function reiniciar() {
      total = 0;
      for (let i = 0; i < cantidades.length; i++) {
        cantidades[i] = 0;
        document.getElementById(`cantidad-${i}`).textContent = 0;
      }
      actualizarTotal();
      document.getElementById("total").style.display = "none";
      document.getElementById("mediosPago").style.display = "none";
      document.getElementById("numeros").style.display = "none";
      document.getElementById("WHATSAPP").style.display = "none";
      document.getElementById("direccion").value = "";
      document.getElementById("comentario").value = "";
    }

    document.getElementById("direccion").addEventListener("input", enviarPorWhatsApp);
    document.getElementById("comentario").addEventListener("input", enviarPorWhatsApp);
  </script>
</body>
</html>
