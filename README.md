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

 ![1](n8n-usuario.PNG)  


Haz clic en el ícono **`+`** ubicado en la parte superior izquierda para agregar un nuevo nodo.

![1](n8n-usuario.PNG)  


## 🔧 Pasos para construir el flujo

### 1. Crear el nodo Webhook

 ![1](n8n-usuario.PNG)  

- Agrega un nodo **Webhook**.
- Método: `GET`
- Selecciona: "Respond with other node".

![Paso 1 - Webhook](./Workflow1-Web.png)

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
