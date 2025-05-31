## Pasos para comenzar

 **Ingresa al sitio oficial**  
   Abre tu navegador y ve al siguiente enlace:  
   👉 [https://n8n.io/](https://n8n.io/)

 **Crea una cuenta nueva**  
   Haz clic en el botón `Get started` ubicado en la página principal.
   ![1](n8n_inicio.png)  

 **Regístrate**  
   Llena el formulario de registro con tus datos. Asegúrate de definir un **nombre de usuario** que utilizarás para ingresar posteriormente.
![1](n8n_formulario.PNG)
 ![1](n8n-usuario.PNG)  

 ## Crear un nuevo flujo en n8n

Haz clic en el botón **"New Workflow"** 

 ![1](nuevo_flujo.png)  


Haz clic en el ícono **`+`** ubicado en la parte superior izquierda para agregar un nuevo nodo.

![1](+.png)  


## 🔧 Pasos para construir el flujo

### 1. Crear el nodo Webhook 

 ![1](webhook.png)  

- Agrega un nodo **Webhook**.
- Método: `GET`
- Selecciona: "Respond with other node".
- Path: Cambiar path por dashboard

![Paso 1 - Webhook](webhook2.png)

---

### 2. Agrega un nodo **Code**

Este nodo genera:
- Un nombre personalizado
- Un campo `Ambiente`
- Un número aleatorio del 1 al 1025 para obtener un Pokémon.

```javascript
for (const item of $input.all()) {
  item.json.displayName = "Malubita";
  item.json.Ambiente = "-test";
  item.json.RandomPokemon = Math.floor(Math.random() * 1025) + 1;
}
return $input.all();
```
![Paso 1 - Webhook](code.PNG)

### 3. Nodo HTTP Request - Consultar PokéAPI

- **Método:** `GET`  
- **URL:**

```bash
https://pokeapi.co/api/v2/pokemon/{{ $json.RandomPokemon }}
```
⚙️ Este paso permite obtener los datos del Pokémon generado aleatoriamente.

![Paso 1 - Webhook](http.PNG)


### 4. Nodo HTML

- Operación: Generate HTML Template
- Contenido: copia el HTML desde el archivo `index.html` incluido en este repositorio.

Puedes utilizar expresiones dentro del HTML como:
```bash
{{ $('Code').item.json.displayName }}
```
🖼️ Esto permite insertar valores dinámicos en la plantilla HTML.

![Paso 1 - Webhook](html_node.PNG)

### 5. Nodo Respond to Webhook

-Tipo de respuesta: Text
-Contenido de respuesta:

```bash
{{ $json.html }}
```
✅ Este paso devuelve el HTML generado al cliente que realizó la solicitud al Webhook.

![Paso 1 - Webhook](webhook3.png)

![Paso 1 - Webhook](flujo.PNG)


😎 Genial ahora vamos a crear los siguiente workflows de feriados

### 1. Crear el nodo Webhook

 ![1](webhook.png)  

- Agrega un nodo **Webhook**.
- Método: `GET`
- Selecciona: "Respond with other node".
- Path: Cambiar path por "feriados"

![Paso 2 - Webhook](webhook2.png)

---

### 2. Agrega un nodo **Code**

Este nodo genera:
- Un filtro para tener los feriados del futuro

```javascript
function isFuture(dateInput) {
  const target = dateInput instanceof Date ? dateInput : new Date(dateInput);
  if (Number.isNaN(target.getTime())) {
    throw new Error('Fecha inválida');
  }
  const now = Date.now();      // Momento actual
  return target.getTime() > now;
}

for (const item of $input.all()) {
  item.json.myNewField = 1;
  item.json.data = item.json.data.filter(e=>isFuture(e.date))  
}

return $input.all();
```

### 2. Agrega un nodo **Code**

Este nodo genera:
- Un filtro para tener los feriados del futuro

```javascript
function isFuture(dateInput) {
  const target = dateInput instanceof Date ? dateInput : new Date(dateInput);
  if (Number.isNaN(target.getTime())) {
    throw new Error('Fecha inválida');
  }
  const now = Date.now();      // Momento actual
  return target.getTime() > now;
}

for (const item of $input.all()) {
  item.json.myNewField = 1;
  item.json.data = item.json.data.filter(e=>isFuture(e.date))  
}

return $input.all();
```


### 5. Nodo Respond to Webhook

-Tipo de respuesta: JSON
-Contenido de respuesta:

```bash
{{ $json.data }}
```


😎😎😎 Genial ahora vamos a crear los siguiente workflows de Valores Api



### 1. Crear el nodo Webhook

 ![1](webhook.png)  

- Agrega un nodo **Webhook**.
- Método: `GET`
- Selecciona: "Respond with other node".
- Path: Cambiar path por "feriados"

![Paso 2 - Webhook](webhook2.png)

### 3. Crear HTTP Request

- Visitar el sitio  https://docs.boostr.cl/reference/holidays
- Agrega un nodo **HTTP Request**.
- Selecciona: `Import Curl`

![Paso 2 - Webhook](http_request.png)

---

### 3. Agrega un nodo **Code**

Este nodo genera:
- Un filtro para tener los feriados del futuro

```javascript
function isFuture(dateInput) {
  const target = dateInput instanceof Date ? dateInput : new Date(dateInput);
  if (Number.isNaN(target.getTime())) {
    throw new Error('Fecha inválida');
  }
  const now = Date.now();      // Momento actual
  return target.getTime() > now;
}

for (const item of $input.all()) {
  item.json.myNewField = 1;
  item.json.data = item.json.data.filter(e=>isFuture(e.date))  
}

return $input.all();
```


### 4. Nodo Respond to Webhook

-Tipo de respuesta: JSON
-Contenido de respuesta:

```bash
{{ $json.data }}
```



😎😎😎 Genial ahora vamos a crear los siguiente workflows de Valores APi

### 0. Crear el nodo Webhook

Nuestro workflow ser vera como el de la siguiente imagen, con procesos en pararelos.

![Paso 2 - Webhook](valores-workflows.png)

### 1. Crear el nodo Webhook

 ![1](webhook.png)  

- Agrega un nodo **Webhook**.
- Método: `GET`
- Selecciona: "Respond with other node".
- Path: Cambiar path por "valoresApi"

![Paso 2 - Webhook](webhook2.png)

---

### 2. Agrega 3 nodos de  **HTTP Request**

- Visitar los siguientes  sitio  https://api.boostr.cl/economy/indicator/uf.json ,  https://api.open-meteo.com/v1/forecast?latitude=-33.4266&longitude=-70.6085&hourly=temperature_2m ,  https://mindicador.cl/api
- Agrega un nodo **HTTP Request**.
- Selecciona: `Import Curl`

![Paso 2 - Webhook](http_request.png)


### 3. Crea un nodo de   **Merge**

- Ajusta la configuración como la de la foto

![Paso 2 - Webhook](valores-merge.png)


### 4. Crea un nodo de   **Edit Fiels**

- Cambiar el valor por el siguiente

```javascript
{
  "valorUF": {{ $json.data.value }},
  "valorDolar": {{ $json.dolar.valor }},
  "temperatura":{{ $json.hourly.temperature_2m[0] }}
}
```

![Paso 2 - Webhook](valores-editfields.png)



### 5. Nodo Respond to Webhook

-Tipo de respuesta: JSON
-Contenido de respuesta:

```bash
{{ $json }}
```





😎😎😎 Genial ahora vamos a crear los siguiente workflows de Spotify

### 1. Crear el nodo Webhook

 ![1](webhook.png)  

- Agrega un nodo **Webhook**.
- Método: `GET`
- Selecciona: "Respond with other node".
- Path: Cambiar path por "spotify"

![Paso 2 - Webhook](spotify-webhook1.png)

---

### 2. Agrega un nodo **Spotify**

- Configura el nodo igual que en la imagen de a continuación

```bash
{{ $json.query.query }}
```
![Paso 2 - Webhook](spotify-search.png)

### 2. Agrega un nodo **Spotify**



```bash
{{ $json.album.artists[0].uri }}
```

![Paso 2 - Webhook](spotify-player.png)


A distrutar nuestro sitio


### Bonus track ###

Agregar ia ?