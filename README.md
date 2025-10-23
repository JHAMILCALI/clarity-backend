# 🚀 Clarity Backend - API para Contratos Inteligentes en Stacks

Backend Flask que proporciona una API REST para interactuar con contratos inteligentes en la blockchain de Stacks, integrado con inteligencia artificial de DeepSeek para interpretar comandos en lenguaje natural.

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-3.0.0-green.svg)](https://flask.palletsprojects.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 📋 Tabla de Contenidos

- [Características](#-características)
- [Requisitos Previos](#-requisitos-previos)
- [Instalación](#-instalación)
- [Configuración](#-configuración)
- [Uso](#-uso)
- [Endpoints de la API](#-endpoints-de-la-api)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [Tecnologías](#-tecnologías)
- [Contribuir](#-contribuir)
- [Licencia](#-licencia)

## ✨ Características

- 🔗 **Integración con Stacks Blockchain**: Lee y ejecuta funciones de contratos inteligentes en Clarity
- 🤖 **IA con DeepSeek**: Interpreta comandos en lenguaje natural y los convierte en acciones sobre el contrato
- 🌐 **API REST**: Endpoints simples y bien documentados
- 🔒 **CORS habilitado**: Listo para integrarse con frontends web
- 📊 **Parseo inteligente**: Convierte respuestas de Clarity a formatos legibles
- 🛠️ **Fácil configuración**: Variables de entorno mediante archivo `.env`

## 🔧 Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- Python 3.8 o superior
- pip (gestor de paquetes de Python)
- Una cuenta en [DeepSeek](https://platform.deepseek.com/) para obtener tu API Key
- Un contrato inteligente desplegado en Stacks (testnet o mainnet)

## 📦 Instalación

1. **Clona el repositorio**

```bash
git clone https://github.com/JHAMILCALI/clarity-backend.git
cd clarity-backend
```

2. **Crea un entorno virtual (recomendado)**

```bash
python -m venv venv

# En Windows
venv\Scripts\activate

# En Linux/Mac
source venv/bin/activate
```

3. **Instala las dependencias**

```bash
pip install -r requirements.txt
```

## ⚙️ Configuración

1. **Crea un archivo `.env` en la raíz del proyecto** con las siguientes variables:

```env
# Configuración del Contrato de Stacks
CONTRACT_ADDRESS=tu_direccion_del_contrato
CONTRACT_NAME=nombre_del_contrato
STACKS_NETWORK=testnet  # o mainnet

# API Key de DeepSeek
DEEPSEEK_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

2. **Ejemplo de configuración**:

```env
CONTRACT_ADDRESS=ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM
CONTRACT_NAME=counter-contract
STACKS_NETWORK=testnet
DEEPSEEK_API_KEY=sk-f984577379764c759173c5762d9c25ec
```

## 🚀 Uso

1. **Inicia el servidor**

```bash
python app.py
```

El servidor estará disponible en `http://127.0.0.1:5000`

2. **Verifica que el servidor está funcionando**

```bash
curl http://127.0.0.1:5000/
```

Respuesta esperada:
```json
{
  "status": "ok",
  "message": "✅ Backend Flask funcionando correctamente"
}
```

## 📡 Endpoints de la API

### 1. **GET /** - Health Check

Verifica que el servidor está funcionando.

**Request:**
```bash
GET http://127.0.0.1:5000/
```

**Response:**
```json
{
  "status": "ok",
  "message": "✅ Backend Flask funcionando correctamente"
}
```

---

### 2. **GET /get-count** - Leer Contador

Lee el valor actual del contador desde el contrato inteligente en Stacks.

**Request:**
```bash
GET http://127.0.0.1:5000/get-count
```

**Response:**
```json
{
  "count": 7,
  "raw_debug": "0x0703..."
}
```

**Descripción:**
- Realiza una llamada de solo lectura al contrato de Stacks
- Parsea la respuesta de Clarity (formato `u7`, `ok u7`, etc.)
- Devuelve el valor como un número entero natural

---

### 3. **POST /chat** - Chat con IA

Envía un mensaje en lenguaje natural a la IA de DeepSeek para interpretar comandos sobre el contrato.

**Request:**
```bash
POST http://127.0.0.1:5000/chat
Content-Type: application/json

{
  "message": "incrementa el contador"
}
```

**Response:**
```json
{
  "action": "increment",
  "message": "Voy a incrementar el contador en 1."
}
```

**Acciones posibles:**
- `"increment"`: Incrementar el contador
- `"read"`: Leer el valor actual
- `"none"`: Ninguna acción específica

**Ejemplos de mensajes:**

| Mensaje del usuario | Acción detectada |
|---------------------|------------------|
| "incrementa el contador" | `increment` |
| "suma 1" | `increment` |
| "cuánto vale el contador" | `read` |
| "dame el valor actual" | `read` |
| "hola" | `none` |

---

## 📁 Estructura del Proyecto

```
clarity-backend/
│
├── app.py                  # Aplicación principal de Flask
├── requirements.txt        # Dependencias de Python
├── .env                    # Variables de entorno (NO subir a Git)
├── .gitignore             # Archivos a ignorar en Git
└── README.md              # Este archivo
```

---

## 🛠️ Tecnologías

Este proyecto utiliza las siguientes tecnologías:

| Tecnología | Versión | Descripción |
|-----------|---------|-------------|
| **Python** | 3.8+ | Lenguaje de programación principal |
| **Flask** | 3.0.0 | Framework web minimalista |
| **Flask-CORS** | 4.0.0 | Manejo de CORS para peticiones cross-origin |
| **python-dotenv** | 1.0.0 | Carga de variables de entorno |
| **requests** | 2.31.0 | Cliente HTTP para peticiones a APIs externas |
| **DeepSeek API** | - | IA para procesamiento de lenguaje natural |
| **Stacks Blockchain** | - | Blockchain para contratos inteligentes en Clarity |

---

## 🔍 Detalles Técnicos

### Parseo de Respuestas de Clarity

El contrato inteligente en Stacks devuelve valores en formato Clarity:

- `u7` → unsigned integer 7
- `ok u7` → resultado exitoso con valor 7
- `0x0703...` → valor hexadecimal codificado

El backend realiza el parseo automático de estos formatos para devolver números enteros naturales.

### Integración con DeepSeek

La IA de DeepSeek se configura con un prompt de sistema específico:

```
"Eres un asistente para interpretar comandos hacia un contrato inteligente 
en la blockchain de Stacks. Devuelve SIEMPRE una respuesta JSON con las claves: 
'action' (por ejemplo: 'increment', 'read', 'none') y 'message' (explicación breve)."
```

Esto garantiza respuestas estructuradas y predecibles.

---

## 🐛 Solución de Problemas

### Error: `ModuleNotFoundError: No module named 'flask'`

**Solución:** Instala las dependencias
```bash
pip install -r requirements.txt
```

### Error: `KeyError: 'CONTRACT_ADDRESS'`

**Solución:** Asegúrate de tener el archivo `.env` configurado correctamente con todas las variables necesarias.

### Error SSL con DeepSeek API

**Solución:** Verifica que tu `DEEPSEEK_API_KEY` sea válida y esté correctamente configurada en el `.env`.

### El contador devuelve un número gigante

**Solución:** El código ya incluye parseo automático. Si persiste el problema, verifica la consola para ver los logs de debug.

---

## 🤝 Contribuir

¡Las contribuciones son bienvenidas! Si deseas contribuir:

1. Haz un Fork del proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

---

## 📝 Licencia

Este proyecto está bajo la Licencia MIT. Consulta el archivo `LICENSE` para más detalles.

---

## 👤 Autor

**JHAMILCALI**

- GitHub: [@JHAMILCALI](https://github.com/JHAMILCALI)
- Repositorio: [clarity-backend](https://github.com/JHAMILCALI/clarity-backend)

---

## 🌟 Agradecimientos

- [Stacks](https://www.stacks.co/) - Blockchain para contratos inteligentes
- [DeepSeek](https://www.deepseek.com/) - IA para procesamiento de lenguaje natural
- [Flask](https://flask.palletsprojects.com/) - Framework web para Python

---

## 📞 Soporte

Si tienes problemas o preguntas, por favor:

1. Revisa la sección de [Solución de Problemas](#-solución-de-problemas)
2. Abre un [Issue](https://github.com/JHAMILCALI/clarity-backend/issues) en GitHub
3. Consulta la [documentación de Stacks](https://docs.stacks.co/)

---

<div align="center">

**⭐ Si este proyecto te fue útil, considera darle una estrella en GitHub ⭐**

Hecho con ❤️ por JHAMILCALI

</div>
